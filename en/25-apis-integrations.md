# 25. APIs / Integrations

---

### Payment Gateway APIs (Stripe, PayPal)

- Verify that a successful Stripe charge returns HTTP 200 with a `PaymentIntent` object containing `status: "succeeded"`; confirm the order is marked as paid in the database within 5 seconds
- Simulate a declined card (Stripe test card `4000000000000002`) and verify the API returns HTTP 402 with error code `card_declined`; confirm the customer sees a human-readable error message and the order remains unpaid
- Test Stripe 3D Secure flow: use test card `4000002500003155`, verify the redirect to the authentication page, confirm successful authentication results in `status: "succeeded"` and failed authentication returns `status: "requires_payment_method"`
- Trigger a Stripe API timeout by simulating a 30-second delay; verify the frontend shows a timeout error message, no duplicate charge is created, and the PaymentIntent is not in a stuck `processing` state
- Verify Stripe webhook signature validation: send a webhook event with an invalid `Stripe-Signature` header and confirm the endpoint returns HTTP 400 and does not process the event
- Send a `payment_intent.succeeded` webhook from Stripe with a valid signature; verify the order status changes to "paid" exactly once (idempotency), even if the webhook is delivered twice
- Send a `charge.refunded` webhook from Stripe; verify the order status updates to "refunded" and the refund amount is logged correctly
- Test PayPal order creation: verify the API returns HTTP 201 with an `id` and `status: "CREATED"`; confirm the approval URL is present and redirects the customer to PayPal
- Simulate a PayPal IPN/webhook with `payment_status=Completed`; verify order is marked paid and confirmation email is sent
- Test PayPal payment capture failure (e.g., `INSTRUMENT_DECLINED`): verify the error is caught, the customer is redirected back to payment selection, and no order is finalized
- Verify that PayPal webhook notifications are validated using the PayPal SDK signature verification before any order state changes occur
- Test concurrent payment attempts on the same order (race condition): submit two simultaneous payment requests and verify only one succeeds; the second returns an appropriate error and no double-charge occurs
- Verify that payment API credentials (API keys, secrets) are never exposed in frontend JavaScript, network responses, or error messages

---

### Shipping Provider API (DHL, DPD, GLS, Hermes, UPS, FedEx)

- Verify that a DHL label creation request returns HTTP 200 with a valid `shipmentTrackingNumber` and a base64-encoded PDF label; confirm the label downloads correctly
- Test label creation with an invalid postal code; verify the API returns a 422 or provider-specific error code and the order is not marked as shipped
- Submit a shipment request to UPS with a missing required field (e.g., no recipient phone number); verify validation catches this before the API call and returns a user-friendly error
- Simulate a DPD API timeout (>10 seconds); verify a retry is attempted up to the configured maximum (e.g., 3 attempts), an alert is logged, and the admin is notified of the failure
- Test the shipment tracking endpoint: query a valid tracking number and verify the response includes current status, location, and estimated delivery date; confirm the data is displayed correctly in the customer account
- Query a tracking number that does not exist; verify the API returns an appropriate not-found response (e.g., HTTP 404 or provider error code) and the UI shows "Tracking information not available"
- Test rate calculation API: send a request with package weight, dimensions, and destination; verify the returned shipping cost matches the amount displayed in checkout (tolerance: 0 EUR difference)
- Simulate a GLS API outage; verify the checkout does not crash, fallback shipping options are shown or a clear error is displayed, and the incident is logged
- Verify that international shipment requests include all required customs fields (HS code, declared value, contents description) for non-EU destinations
- Test address validation API call during checkout: submit an unrecognized address and verify the system either flags it with a warning or proceeds with a logged risk marker

---

### ERP Integration

- Verify that a new order triggers an ERP sync within the configured SLA (e.g., within 2 minutes); confirm the ERP system receives all required fields (order ID, line items, quantities, prices, customer data)
- Test ERP sync for an order with 50+ line items; verify all items are transmitted without truncation and the ERP acknowledges receipt
- Simulate an ERP API returning HTTP 503; verify the shop queues the sync job for retry, does not block the customer-facing order confirmation, and retries within the configured interval (e.g., 5 minutes)
- Submit duplicate order data to the ERP (same order ID twice); verify the ERP or integration layer handles idempotency correctly and does not create a duplicate order
- Verify that an ERP order status update (e.g., "shipped") is received via webhook or polling and reflected in the shop's order management within the expected timeframe
- Test data type consistency: verify that decimal prices, date formats, and character encoding (UTF-8, including umlauts) are correctly transmitted to and from the ERP

---

### Inventory Management Sync

- After a purchase, verify that the inventory management system receives a stock deduction event within the configured SLA; confirm the quantity is reduced by the exact ordered amount
- Simulate a stock sync returning a negative inventory value; verify the shop does not display negative stock counts and treats the product as out of stock
- Test real-time stock sync: update stock in the inventory system to 0 for a product currently in an active shopping session; verify the product is marked unavailable before the customer completes checkout
- Verify that variant-level stock (e.g., size M, color blue) is synced independently; changing stock for one variant does not affect another variant's availability
- Simulate an inventory API timeout during checkout; verify the order is not blocked but a stock reservation is logged for manual review, and an alert is sent to the admin
- Test bulk stock import: upload a CSV with 1,000 product stock updates; verify all updates are processed, no records are silently dropped, and a success/failure report is generated

---

### Accounting Export

- Verify that completed orders are included in the accounting export within the configured period (daily/weekly); confirm exported data matches the shop database (order total, VAT, discounts)
- Export an order containing a partial refund; verify the export reflects the net amount after refund and VAT is recalculated correctly
- Test export file format compliance: open the exported CSV/XML/DATEV file in the target accounting software and confirm it imports without errors
- Simulate an export for a month with 10,000+ orders; verify the export completes within an acceptable time limit and the file is not truncated
- Verify that no customer payment credentials (card numbers, IBAN) are included in the accounting export
- Test that the export is only accessible to authorized admin roles; a non-admin user requesting the export endpoint receives HTTP 403

---

### Product Feed (Google Shopping, Meta)

- Fetch the Google Shopping product feed URL and verify it returns valid XML conforming to the Google Merchant Center schema; validate with the official feed validator
- Verify that all required attributes are present for each product in the feed: `id`, `title`, `description`, `link`, `image_link`, `price`, `availability`, `condition`
- Check that out-of-stock products are either excluded from the feed or marked with `availability: out of stock`; verify no products with `stock: 0` appear as `in stock`
- Test feed generation performance: the feed for a catalog of 10,000+ products must be generated and served within 30 seconds
- Verify that sale prices in the feed (`sale_price`) match the discounted price shown on the product page exactly; test with at least 5 products on promotion
- Fetch the Meta/Facebook product catalog feed and validate the required fields (`id`, `title`, `description`, `availability`, `condition`, `price`, `link`, `image_link`) for every item
- Verify that the feed URL is accessible without authentication but the feed generation script is not exploitable (no path traversal, no sensitive data leakage)
- Test incremental feed update: after a price change on one product, verify the next feed generation reflects the updated price

---

### Warehouse Management Sync

- Verify that new orders are pushed to the warehouse management system (WMS) with correct pick-list data (SKU, quantity, bin location) within the configured SLA
- Simulate a WMS confirmation of shipment dispatch; verify the shop updates the order status to "Shipped" and triggers the shipping confirmation email
- Test sync for an order with split fulfillment (items from multiple warehouses); verify each sub-shipment is tracked independently and the customer receives separate tracking numbers
- Simulate a WMS API returning an error for an unknown SKU; verify the error is logged, the admin is notified, and the order is flagged for manual processing
- Verify that WMS sync respects stock reservations: an item reserved in checkout is not reallocated to another order during the reservation window

---

### CRM Integration

- Verify that a new customer registration triggers a CRM contact creation within the configured SLA; confirm all mapped fields (name, email, address, phone) are transmitted correctly
- Test that an order completion event updates the customer's purchase history in the CRM (last order date, total order value, order count)
- Simulate a CRM API rate limit response (HTTP 429); verify the integration queues the request, respects the `Retry-After` header, and eventually syncs successfully
- Verify that customer data deletions (GDPR "right to be forgotten") propagate to the CRM within the agreed timeframe and the contact record is anonymized or deleted
- Test that CRM sync does not expose customer data in logs (no email addresses, phone numbers, or addresses in plain-text log files)

---

### Email Service Provider (Newsletter)

- Subscribe a new email address via the newsletter form; verify the ESP receives the contact, the double opt-in email is sent within 60 seconds, and the contact status is "pending" until confirmation
- Confirm the double opt-in link; verify the contact status changes to "subscribed" in the ESP and the welcome email is triggered
- Test unsubscribe: click the unsubscribe link in a newsletter; verify the contact is marked unsubscribed in the ESP within 5 minutes and no further emails are sent to that address
- Verify that the ESP API returns the correct subscription status when queried; a subscribed contact returns `status: subscribed`, an unsubscribed contact returns `status: unsubscribed`
- Simulate an ESP API outage during newsletter signup; verify the signup request is queued, the customer sees a success message, and the sync completes when the ESP is available again
- Test list segmentation: verify that customers who opted in during checkout are added to the correct list/segment in the ESP, not a general list
- Verify that transactional emails (order confirmations) are sent via a separate ESP stream/IP from marketing emails, and delivery logs are accessible for debugging

---

### Review Platform API (Trustpilot, Ekomi)

- Verify that a review invitation is sent to the customer via the review platform API within the configured delay after delivery (e.g., 7 days); confirm the API call returns HTTP 200 or 201
- Simulate a review platform API returning HTTP 401 (invalid API key); verify the error is logged, an admin alert is triggered, and no review invitation is silently dropped
- Fetch aggregated review data from the platform API; verify the returned average rating and review count match what is displayed on the shop's homepage and product pages
- Test the review import/sync: after a new review is submitted on the platform, verify it appears on the shop within the configured sync interval
- Verify that the review widget loaded from the external platform does not block page rendering (loaded asynchronously) and does not cause CLS (Cumulative Layout Shift)
- Simulate a review platform API timeout; verify the shop page loads correctly without the review data and a fallback (cached data or empty state) is shown

---

### Price Comparison Portal Feed

- Fetch the idealo/Billiger.de product feed and validate it against the portal's required schema; verify all mandatory fields are present and correctly formatted
- Verify that price updates are reflected in the feed within the configured refresh interval (e.g., within 24 hours); test by changing a product price and checking the next feed generation
- Check that discontinued or out-of-stock products are removed from the feed or flagged as unavailable, preventing customers from clicking through to a 404 or unavailable product
- Test deep link accuracy: every product link in the feed must resolve to the correct product page with a 200 status code; scan for any 301 chains longer than 2 hops or 404s
- Verify that the feed access URL is either publicly accessible (correct) or protected with a token, depending on the portal's requirements; confirm no unintended sensitive data is included

---

### Marketplace Integrations (Amazon, eBay)

- Verify that a new order received from Amazon Seller Central is imported into the shop's order management system within the configured polling interval; confirm all order fields map correctly
- Test order status sync: when the shop marks an Amazon order as shipped with a tracking number, verify the tracking information is pushed back to Amazon within 30 minutes and meets Amazon's SLA
- Simulate an eBay listing update (price or stock change); verify the change is reflected on eBay within the configured sync interval and no stale data causes overselling
- Test error handling for a marketplace API returning HTTP 429 (rate limit); verify the integration backs off according to the `Retry-After` header and queues pending updates
- Verify that marketplace order imports do not create duplicate orders if the same order ID is received twice (idempotency check)
- Test currency and tax handling: Amazon orders in foreign currencies are converted and stored correctly in the shop's base currency with the exchange rate logged

---

### Webhook Processing

- Send a valid webhook payload to each registered endpoint; verify each endpoint returns HTTP 200 within 5 seconds and processes the event correctly
- Send a webhook with an invalid or missing signature; verify the endpoint returns HTTP 400 or 401 and logs the rejected request, without processing any state changes
- Send the same webhook event ID twice (simulating a delivery retry); verify the handler is idempotent — the second delivery is acknowledged (HTTP 200) but no duplicate action is performed
- Simulate a slow webhook handler (processing takes >30 seconds); verify the endpoint returns HTTP 200 quickly (acknowledge first, process asynchronously) so the sender does not mark the delivery as failed
- Send a webhook with a malformed JSON body; verify the endpoint returns HTTP 400, logs the error with the raw payload for debugging, and does not throw an unhandled exception
- Verify that all webhook events are logged with timestamp, event type, payload hash, processing status, and any errors; confirm logs are accessible to authorized admins
- Test webhook retry behavior: if the endpoint is unavailable and returns HTTP 503, verify the sending system retries with exponential backoff; after max retries, an admin alert is triggered
- Verify that webhook endpoints are not exploitable: test for SSRF by sending a webhook with a payload containing an internal IP address as a callback URL

---

### API Rate Limits

- Identify the rate limit for each external API (requests per second/minute/hour) and document it; verify the integration layer does not exceed these limits under normal operation
- Run a load test simulating peak traffic; verify that API calls to rate-limited services are queued or throttled rather than failing with HTTP 429 errors that reach the customer
- Simulate receiving HTTP 429 from Stripe during a high-traffic period; verify the integration respects the `Retry-After` header, queues the retry, and does not process the payment multiple times
- Verify that the shop has its own internal rate limiting on API-backed features (e.g., address autocomplete, real-time shipping rates) to prevent a single user from exhausting the quota
- Test rate limit recovery: after a 429 response, verify the integration resumes normal operation once the rate limit window resets, without requiring a manual restart

---

### API Error Handling

- For each integrated API, simulate an HTTP 500 Internal Server Error response; verify the shop does not crash, displays a user-friendly error message, and logs the full error details server-side
- Simulate an API returning a valid HTTP 200 response but with an unexpected JSON schema (missing required fields); verify the integration handles schema mismatch gracefully and does not cause a null pointer exception
- Test error propagation: when a non-critical API (e.g., recommendation engine) fails, verify it does not block the checkout or core page load; the feature degrades gracefully
- Verify that all API error responses are logged with sufficient detail (endpoint, request ID, status code, response body snippet) to enable debugging without exposing sensitive data
- Test behavior when an API returns an empty response body with HTTP 204; verify the integration handles this correctly and does not attempt to parse a null body
- Simulate network-level errors (DNS resolution failure, connection refused) for each integrated API; verify the circuit breaker or timeout handler catches these and triggers the appropriate fallback

---

### API Retry Logic

- Verify that transient errors (HTTP 429, 503, 504) trigger automatic retries with exponential backoff; confirm the retry intervals are (for example) 1s, 2s, 4s, 8s before giving up
- Test that non-transient errors (HTTP 400, 401, 403, 404) do NOT trigger retries; these errors are logged immediately and escalated without wasting retry budget
- Verify that retry logic is idempotent: retrying a payment capture or order creation does not result in duplicate charges or duplicate orders (use idempotency keys where supported, e.g., Stripe's `idempotency_key` header)
- Test maximum retry exhaustion: after the maximum number of retries is reached, verify the job is moved to a dead-letter queue, an admin alert is sent, and the failed event is retained for manual processing
- Verify retry logic under concurrent load: 100 simultaneous failed API calls all triggering retries must not cause a retry storm that overwhelms the external API or the shop's job queue
- Test that retry state persists across application restarts: if the server restarts while a job is in the retry queue, verify the job is not lost and is retried after restart
