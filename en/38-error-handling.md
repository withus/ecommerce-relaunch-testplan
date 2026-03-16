# 38. Error Handling & Graceful Degradation

### Behavior on payment gateway outage (fallback or error message)

- Simulate Stripe API outage by blocking `api.stripe.com` at the network level (via `/etc/hosts` or firewall rule); verify the checkout page displays a user-friendly message ("Payment is temporarily unavailable, please try again later") instead of a raw exception
- Simulate PayPal endpoint timeout by throttling the connection to PayPal's API to 0 bytes/sec using `tc netem` or a proxy tool; verify the system times out gracefully within a defined threshold (e.g., 10 seconds) and does not hang indefinitely
- Simulate Klarna service returning HTTP 503; verify the Klarna payment option is hidden or disabled in the payment method selection and an alternative is offered rather than breaking the entire checkout
- Configure the payment gateway mock to return HTTP 500 mid-transaction (after the user submits card details); verify the order is NOT created in the database, the customer is informed, and no money is charged
- Simulate partial gateway outage (gateway accepts requests but always returns `"status": "failed"`); verify the error message shown to the user is human-readable and does not expose response body details or internal service names
- After payment gateway recovery, verify the checkout flow resumes normally without requiring a page reload or cache clear
- Test fallback behavior: if the primary payment provider is down, verify whether a configured secondary provider is offered automatically or whether the user is clearly instructed on how to proceed
- Verify that failed payment attempts due to gateway outage are logged server-side with timestamp, error code, and masked payment details for support purposes
- Simulate gateway returning a network-level TCP reset (connection refused); verify the application does not display a PHP/Node.js stack trace or database error to the user
- Check that monitoring/alerting triggers (e.g., Sentry, PagerDuty) fire within the configured threshold when the payment gateway returns repeated errors

### Behavior on database connection loss

- Stop the database server (e.g., `systemctl stop mysql`) while the shop is running; verify the homepage responds with a generic error page ("Service temporarily unavailable") and does not expose database credentials, server IP, or query details
- Simulate a database connection timeout by setting `connect_timeout` to 1 second in the DB config and blocking the DB host; verify the application fails fast and does not queue thousands of waiting connections
- During an active checkout, kill the database connection mid-flow (after address entry, before order placement); verify the user receives a clear error, the session is preserved, and the customer can retry without re-entering all form data
- Simulate read replica lag/failure (replica returns stale or no data); verify the application falls back to the primary database or clearly communicates data unavailability
- Simulate database connection pool exhaustion (all connections in use) under load; verify new requests receive a "service busy" response rather than an application crash or infinite wait
- Verify that static pages (homepage, CMS pages) served from cache remain accessible even when the database is offline
- After database recovery, verify the connection pool re-establishes automatically without requiring an application restart
- Simulate intermittent database errors (1 in 5 queries fails); verify retry logic is in place for transient failures and errors are logged with context (query hash, timestamp, user ID)
- Verify that the admin backend shows a clear "Database unavailable" notice rather than a white screen or exception dump when the DB is offline
- Check that no sensitive database error messages (table names, column names, SQL syntax errors) are ever returned in HTTP responses to end users

### Behavior on external API timeout (shipping, tax, etc.)

- Simulate the shipping cost API (e.g., DHL rates endpoint) timing out by inserting a 30-second artificial delay via a proxy; verify the checkout shows a fallback shipping cost or a "shipping cost cannot be calculated at this time" notice within a reasonable timeout (5–10 seconds)
- Simulate the tax calculation service returning HTTP 504; verify VAT is calculated using a local fallback rate rather than displaying a broken price or blocking checkout
- Throttle the address validation API to simulate slow responses; verify the checkout form does not block submission while waiting, and the user can proceed after a timeout with a warning
- Simulate the product recommendation service being unreachable; verify the recommendation widget is simply hidden or replaced with a static fallback rather than causing layout breakage
- Simulate the review platform API (Trustpilot/Ekomi) returning a timeout; verify the product page still loads fully and shows a "Reviews temporarily unavailable" placeholder rather than an empty broken section
- Test circuit breaker behavior: after N consecutive timeouts from an external service, verify the application stops sending requests to that service for a cooldown period (circuit open state) and serves cached or fallback data
- Simulate the geolocation/IP detection API being unavailable; verify default country/currency is used as fallback and no JavaScript errors appear in the console
- Simulate the chat widget (Zendesk/Intercom) JS failing to load; verify it does not block page rendering, the chat button is simply absent, and no console errors affect other page functionality
- Use browser DevTools Network throttling (set to "Offline") to simulate complete API unavailability after initial page load; verify that partially loaded pages degrade gracefully without JavaScript exceptions
- Verify that all external API calls have explicit timeout values configured (not relying on OS defaults) and that these are documented and tested

### User-friendly error messages (no stack trace, no technical details)

- Trigger a PHP fatal error by accessing a route that deliberately throws an uncaught exception in staging; verify the production environment returns a branded error page with no file paths, line numbers, class names, or stack traces
- Submit a malformed JSON payload to a checkout API endpoint; verify the response is HTTP 400 with a user-readable message ("Invalid request") and no internal error details
- Trigger a 500 error via a database constraint violation; verify the error response contains a generic message and the full error is only written to the server-side log
- Test error messages for all form validation failures (checkout, registration, contact form); verify messages are in the user's language, actionable ("Please enter a valid email address"), and do not reveal backend field names or validation rule internals
- Simulate a session deserialization failure; verify the user is redirected to the login page with a friendly message rather than seeing a raw exception
- Verify that HTTP response headers on error pages do not leak server software versions (remove `X-Powered-By: PHP/8.x`, `Server: Apache/2.x`)
- Test error states across all supported browsers (Chrome, Firefox, Safari, Edge); verify error pages render correctly and are fully readable without JavaScript
- Verify that AJAX error responses (used in cart updates, address validation) return structured JSON with a `message` field in plain language, not raw exception objects
- Simulate a file upload exceeding the server limit; verify the error message clearly states the file size limit rather than a generic server error
- Check that error pages do not include embedded analytics or tracking pixels that could trigger secondary errors in the browser console

### 404 page with search function and navigation help

- Request a URL that does not exist (e.g., `/products/deleted-item-slug`); verify a custom 404 page loads with HTTP status code 404 (not 200 with "not found" text, not a redirect)
- Verify the 404 page contains a functional search field; enter a product name and confirm it navigates to the correct search results page
- Verify the 404 page contains links to the main navigation categories (homepage, top-level categories, contact) to allow the user to recover without using the back button
- Simulate a migrated old URL that should have a 301 redirect but does not; confirm the 404 page is shown and that a recommendation or search result for similar content is displayed
- Request a 404 URL with query parameters (e.g., `/old-page?ref=newsletter`); verify the 404 page is returned correctly and query parameters do not cause unexpected behavior
- Verify the 404 page renders correctly on mobile viewports; all CTAs must be reachable via touch and above the fold or clearly visible
- Check that the 404 page includes recommendation widgets ("Popular products", "Recently viewed") to recover user intent
- Verify that the 404 page response includes the correct HTTP 404 status code (not being served as 200, which would cause soft-404 SEO issues) using `curl -I <url>`
- Access a 404 URL as a screen reader user (using NVDA or VoiceOver); verify the page heading clearly states "Page not found" and the search field has a proper label
- Verify that 404 errors are logged server-side (URL, referrer, user agent, timestamp) to allow identification of broken links post-relaunch

### 500 error: automatic admin notification

- Trigger a controlled 500 error in staging (via a test route that throws an unhandled exception); verify Sentry (or configured monitoring tool) receives the event within 60 seconds including stack trace, request URL, and user session context
- Verify that the admin notification email or Slack/webhook alert contains actionable information: URL, error message, timestamp, environment (production vs. staging), and a link to the full error report
- Simulate repeated identical 500 errors (same exception 50 times); verify the notification system applies deduplication or rate limiting and does not flood the admin inbox
- Verify that 500 errors during the checkout flow trigger high-priority alerts (separate from general 500s), given the revenue impact
- Trigger a 500 error while logged in as a known customer; verify the error report includes the customer ID (or anonymized identifier) for support purposes but no payment card data
- After a 500 error occurs, verify the server automatically returns to a healthy state for subsequent requests (no cascading failures due to corrupted shared state)
- Simulate a 500 error on the order confirmation page (after payment was already processed); verify the alert to admins includes a flag that payment may have been captured without an order being created
- Verify that the 500 error page itself is static and does not rely on the database or application server to render (to avoid a second error while displaying the first)
- Confirm that error notification thresholds are configured: a single 500 error does not trigger an alert, but N errors within M minutes does (to avoid alert fatigue)
- Test the escalation path: verify that if the primary notification channel (email) fails, a secondary channel (SMS or PagerDuty) is triggered

### Form recovery after error (inputs not lost)

- Fill in the checkout address form completely, submit with an intentionally invalid field (e.g., malformed postal code), and verify all previously entered valid data is repopulated in the returned form
- Fill in the multi-step checkout through to payment, simulate a payment failure, and verify the user is returned to the payment step with address and shipping data intact (not reset to step 1)
- Fill in the registration form, submit with an email that already exists, and verify all other fields (name, password) are retained and only the email field is highlighted with an error
- Fill in the contact form with a long message, submit with an empty required field, verify the full message text is preserved in the textarea after the validation error is shown
- Simulate a network interruption during form submission (disconnect mid-POST using DevTools); verify the user sees a connection error message and the form data is preserved for resubmission
- Test form recovery in Safari on iOS; verify that the browser's native form restoration (bfcache) works correctly or the application explicitly preserves state
- Verify that password fields are intentionally cleared after a failed login attempt (security requirement) while the email/username field is retained
- Test a form with a file upload field; after a validation error, verify that only the file upload field requires re-selection while all text fields are preserved
- Submit the checkout form with JavaScript disabled; verify the server-side rendered error response repopulates all fields through HTML `value` attributes, not only via JavaScript
- Verify that form error states are preserved across a soft page refresh (F5); entered data and error highlights should not be lost due to a simple reload

### Circuit breaker pattern for external services

- Configure the circuit breaker threshold (e.g., 5 failures in 30 seconds opens the circuit) for the shipping API; simulate 5 consecutive failures and verify the 6th request is rejected immediately without attempting the external call
- While the circuit is open for the shipping API, verify the user-facing behavior: the checkout shows a static fallback shipping cost or a "live rates unavailable" message, not an error
- After the circuit opens, wait for the configured cooldown period (e.g., 60 seconds) and verify the circuit transitions to half-open state, allowing a probe request through
- Simulate a successful probe request during half-open state; verify the circuit closes and normal operation resumes, including restoring live shipping rates in the checkout
- Simulate the probe request during half-open state failing again; verify the circuit returns to open state and the cooldown timer resets
- Verify that circuit breaker state is shared across all application instances (not per-process) when running on multiple servers or containers
- Test circuit breaker behavior for the payment gateway: confirm the breaker opens after repeated gateway errors and that the fallback message is shown consistently across all checkout entry points (cart page, express checkout, standard checkout)
- Verify that circuit breaker state transitions are logged with timestamps for post-incident analysis
- Simulate a cascading failure scenario: the circuit breaker for the tax service opens, which triggers a fallback; verify the fallback itself does not call another failing service (no cascade)
- Test circuit breaker behavior under concurrent load (multiple requests hitting the open circuit simultaneously); verify all are served the fallback response without queuing or timeouts

### Offline tolerance / service worker caching

- Load the shop in Chrome, then switch DevTools Network to "Offline"; verify a service worker serves a cached offline page with a user-friendly message and navigation options rather than the browser's default "No internet" error
- With service worker caching active, revisit a previously loaded product page while offline; verify the cached version renders correctly (images, price, product name) with a banner indicating the page may be outdated
- Clear the service worker cache, load the shop, and simulate going offline immediately; verify the fallback offline page is shown (not a blank page), even with an empty cache
- Verify that the cart and checkout pages are explicitly excluded from service worker caching to prevent stale pricing or stock data from being shown offline
- Test service worker update behavior: deploy a new version of the service worker and verify it activates without requiring a hard reload, and that stale assets are not served to users
- Simulate a flaky network connection (intermittent packet loss using `tc netem loss 20%`); verify that cached resources are served without visible errors and failed requests are retried transparently
- Verify that the service worker does not cache responses containing personal data (order confirmation page, account dashboard) to prevent data leakage on shared devices
- Check that the offline page includes a "Retry" button that re-attempts the original navigation when the connection is restored
- Verify service worker behavior in Safari on iOS (which has different caching limits and lifecycle behavior); confirm the offline fallback and cached pages work equivalently across Chrome, Firefox, and Safari
- Verify that analytics and tracking calls that fail while offline are queued and retransmitted when the connection is restored, so conversion data is not lost due to temporary connectivity issues
