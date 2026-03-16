# 39. Fraud Prevention

### Bot detection on login (brute force protection)

- Verify that 5 consecutive failed login attempts with the same email address within 60 seconds triggers an account lockout (temporary, e.g. 15 minutes)
- Verify that after lockout, the user receives a notification email with instructions to unlock or wait
- Verify that the lockout counter resets after a successful login (not just after the timeout)
- Verify that lockout applies per account, not just per IP — attempting from a second IP with the same credentials still hits the lockout
- Verify that a locked account still receives a generic error message ("invalid credentials or account locked") without revealing which condition triggered it
- Verify that administrative accounts require a stricter threshold (e.g. 3 attempts) compared to regular customer accounts
- Verify that failed attempt counters are stored server-side (not in a cookie or hidden field that can be reset by the client)
- Verify that automated login sequences (e.g. credential stuffing at 10+ requests/second) are detected and the source IP is throttled or blocked before the per-account lockout threshold is reached
- Verify that a CAPTCHA challenge is presented after 3 failed attempts even if the account is not yet locked
- Verify that brute force events are written to a security log with timestamp, IP, user agent, and targeted account identifier
- Verify that an admin alert (email or Slack) is sent when more than 20 failed login attempts for the same account occur within 10 minutes

### Rate limiting on form submissions

- Verify that the contact form accepts no more than 5 submissions per IP address per 10-minute window; the 6th submission returns HTTP 429 with a human-readable "too many requests" message
- Verify that the registration form is rate-limited to 3 new account creations per IP per hour
- Verify that the password-reset form is limited to 3 reset requests per email address per hour to prevent email flooding
- Verify that the newsletter sign-up form is limited to 2 submissions per IP per 5 minutes
- Verify that the "add to wishlist" and "add to cart" endpoints are rate-limited to prevent automated scraping/bulk additions (threshold: >60 requests/minute per session triggers a soft block)
- Verify that rate-limit counters use a sliding window algorithm rather than a fixed window to prevent burst abuse at window boundaries
- Verify that legitimate users who hit the rate limit see a retry-after value in the response so they know when to try again
- Verify that rate limits are applied at the application layer (not only at the CDN) so they still function when the CDN is bypassed
- Verify that rate-limit state is shared across application instances (stored in Redis or equivalent), preventing bypass by hitting a different server
- Verify that POST endpoints in the checkout flow (address save, shipping selection, place order) each enforce independent rate limits of no more than 20 requests/minute per session

### CAPTCHA/reCAPTCHA at critical points (login, registration, checkout)

- Verify that reCAPTCHA v3 (or equivalent) is active on the login form and that scores below 0.5 trigger an explicit CAPTCHA challenge (v2 checkbox or image selection)
- Verify that the registration form always presents a CAPTCHA challenge (v2 or v3 with fallback) and that submission fails if the CAPTCHA token is missing or invalid
- Verify that the checkout "Place Order" button triggers a background CAPTCHA score check; scores below 0.3 block order placement and present a manual challenge
- Verify that the contact form requires CAPTCHA validation and that replaying a previously valid CAPTCHA token does not bypass the check (one-time use enforced server-side)
- Verify that the password-reset request form uses CAPTCHA to prevent automated bulk reset requests
- Verify that CAPTCHA is also active on the "notify me when back in stock" and "ask a question" forms
- Verify that CAPTCHA failures return a clear user-facing error (not a generic 403) explaining what the user needs to do
- Verify that the CAPTCHA library version is current and not using a deprecated endpoint (e.g. v1 reCAPTCHA is not in use)
- Verify that CAPTCHA challenges are accessible: an audio alternative is available for visually impaired users
- Verify that failed CAPTCHA attempts are logged and that repeated failures from the same IP within 5 minutes trigger IP-level rate limiting

### Honeypot fields in forms

- Verify that the registration form contains at least one hidden honeypot field (CSS `display:none` or `visibility:hidden` with a plausible name like `website` or `phone2`) that must remain empty for submission to succeed
- Verify that the contact form contains a honeypot field and that any submission with that field populated is silently discarded (HTTP 200 returned to mislead bots, but no data is stored and no email is sent)
- Verify that honeypot fields are not visible in any browser viewport (desktop, tablet, mobile) and cannot be inadvertently triggered by autofill
- Verify that honeypot fields do not have `autocomplete="off"` only — they should also carry `tabindex="-1"` so keyboard users cannot accidentally fill them
- Verify that the newsletter sign-up form uses a honeypot field and that submissions with a filled honeypot are counted and logged for trend monitoring
- Verify that honeypot validation is performed server-side and cannot be bypassed by removing the field from the POST body and resubmitting
- Verify that honeypot field names are not identical across all forms (using the same name sitewide allows bots to learn to skip it)
- Verify that honeypot hits are logged with timestamp, IP, user agent, and form identifier so patterns can be analyzed

### Credit card testing detection (many failed payments)

- Verify that more than 3 payment failures (declined card) within 60 seconds from the same IP address triggers an IP-level soft block on payment submissions for 30 minutes
- Verify that more than 2 consecutive payment failures on the same customer account triggers a temporary hold on that account's ability to place orders, with a notification sent to the account email
- Verify that the system detects rapid sequential use of different card numbers from the same IP (carding pattern: >5 distinct card numbers attempted within 10 minutes) and blocks the IP with an alert sent to the fraud team
- Verify that payment failures are logged with full details: timestamp, IP, session ID, card BIN (first 6 digits), failure reason code, and order amount
- Verify that a fraud alert is sent to the shop admin when 10 or more payment failures occur sitewide within 5 minutes (potential coordinated carding attack)
- Verify that after a card testing block is applied, the affected IP cannot bypass it by starting a new session or clearing cookies
- Verify that small test amounts (e.g. €0.01, €1.00) in combination with rapid sequential attempts trigger additional scrutiny even if individually successful
- Verify that the payment gateway's own fraud scoring (e.g. Stripe Radar rules) is configured and active, and that high-risk charges are placed in review rather than automatically captured
- Verify that customers blocked due to card testing receive a generic error message ("payment could not be processed, please contact support") rather than a message indicating they have been flagged
- Verify that blocked IP/account incidents are reviewed by an admin within 24 hours and that false positives can be unblocked manually without code changes

### IP-based anomaly detection

- Verify that orders originating from IP addresses on known commercial proxy or VPN ranges are flagged for manual review (not automatically rejected, but queued)
- Verify that orders from Tor exit nodes are either blocked outright or require additional verification (e.g. email confirmation with a unique link before fulfillment begins)
- Verify that a single IP placing more than 5 orders within 60 minutes is flagged as a velocity anomaly, regardless of whether the orders use different accounts or guest checkout
- Verify that geolocation mismatch is detected: billing address country does not match IP country → flag for review and log the discrepancy
- Verify that IP reputation scoring (via a service such as MaxMind minFraud or equivalent) is integrated at checkout and that scores above a defined risk threshold (e.g. 75/100) block order completion pending manual review
- Verify that the anomaly detection log is accessible to admins and includes IP, geolocation, order count, risk score, and triggered rule name for each flagged event
- Verify that flagged orders do not visibly stall the customer's checkout — the order is accepted with a pending status while the backend review happens, except for automatically blocked cases
- Verify that IP blocks applied by the fraud system do not affect shared IP addresses (e.g. mobile carrier NAT) in a way that prevents legitimate customers from purchasing; time-based expiry (max 1 hour auto-block) is in place
- Verify that the IP anomaly thresholds are configurable by admins without a code deployment
- Verify that alerts are sent in real time (within 2 minutes of trigger) to the fraud monitoring channel when a high-risk IP event occurs

### Address Verification System (AVS) for credit cards

- Verify that AVS is enabled for all credit card transactions and that the AVS response code is stored with each transaction record
- Verify that orders with AVS result "No Match" (both street and postal code unmatched) are automatically held for manual review rather than fulfilled immediately
- Verify that orders with AVS result "Postal Code Match Only" are allowed to proceed but flagged in the order management system with a yellow risk indicator
- Verify that orders with AVS result "Full Match" are processed normally without any additional friction
- Verify that AVS failures do not expose the specific AVS code to the end user — the customer sees only a generic "address verification failed" message
- Verify that the AVS check result is logged and visible to admin users in the order detail view
- Verify that AVS is re-checked if the customer modifies their billing address after initial entry (not only on first submission)
- Verify that international orders where AVS is not supported by the issuing bank are not automatically rejected, but are instead scored by other available signals (IP match, order history, etc.)
- Verify that a combination of AVS failure + new customer + high order value (e.g. over €300) triggers automatic escalation to a manual fraud review queue
- Verify that refunds and chargebacks associated with AVS-mismatch orders are tracked and used to tune the AVS decision thresholds over time

### Velocity checks (unusually many orders per IP/account)

- Verify that placing more than 3 orders from the same customer account within 1 hour triggers a velocity alert and places subsequent orders in a review queue
- Verify that placing more than 5 orders from the same IP address within 1 hour (across any accounts or guest sessions) triggers an IP-level velocity block
- Verify that velocity checks consider total order value, not just order count: a single account reaching cumulative spend of €1,000 within 24 hours is flagged regardless of order count
- Verify that velocity thresholds apply across guest checkout as well — guest orders sharing the same IP or email domain as a blocked account are flagged
- Verify that the velocity check window is a rolling 60-minute window, not a fixed hourly bucket, to prevent gaming the system at window resets
- Verify that velocity rules cover the coupon redemption flow: more than 2 coupon uses per account per day, or more than 10 uses of the same coupon code across different accounts within 1 hour, triggers an alert
- Verify that orders blocked by velocity checks are not lost — they are queued with status "fraud review" and the customer receives an email stating the order is under review
- Verify that legitimate bulk buyers (B2B accounts explicitly whitelisted by an admin) are exempt from standard velocity thresholds and have their own configurable higher limits
- Verify that velocity check data (order timestamps, amounts, IPs, account IDs) is retained for at least 90 days for forensic investigation
- Verify that the fraud review queue triggers an admin notification (email + in-app) immediately when a new order is added, and that SLA for review is defined (e.g. within 4 business hours)
- Verify that repeated velocity triggers from the same account (3 or more velocity blocks within 30 days) result in a permanent account suspension pending manual review, not just another temporary hold
