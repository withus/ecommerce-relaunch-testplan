# 24. Analytics / Tracking

### Google Analytics 4 integrated

- Verify GA4 measurement ID (`G-XXXXXXXXXX`) is present in the page source or via GTM on all page types (homepage, category, product, cart, checkout, confirmation, CMS pages).
- Open browser DevTools → Network tab, filter by `collect?v=2`, confirm the request fires on page load with the correct `tid` (measurement ID).
- In GA4 DebugView, load the homepage while in debug mode (`?_dbg=1` or GTM Preview mode) and confirm a `page_view` event appears with parameters: `page_title`, `page_location`, `page_referrer`.
- Confirm GA4 is NOT loaded (or fires in cookieless/denied mode) when the user has not yet given consent — no `collect` requests should appear before consent is granted.
- Check that GA4 is loaded exactly once per page (no duplicate measurement IDs, no double-firing of `page_view`).
- Verify the GA4 data stream configuration matches the live domain (not a staging/dev domain).
- In GA4 → Realtime report, navigate through the site and confirm active users and page_view events appear within 30 seconds.

### GA4 events correct (page_view, view_item, add_to_cart, purchase)

- **page_view**: Confirm fires on every page navigation including SPA route changes. Parameters checked: `page_title` (not empty), `page_location` (full URL), `page_referrer`.
- **view_item**: On a product detail page, confirm the event fires with `items` array containing: `item_id` (SKU), `item_name`, `item_brand`, `item_category`, `item_variant`, `price` (numeric, no currency symbol), `currency` (ISO 4217, e.g. `EUR`), `index` (position).
- **view_item_list**: On category/search result pages, confirm the event fires with `item_list_id`, `item_list_name`, and an `items` array for all visible products. Each item must include `index` (1-based position).
- **add_to_cart**: Click "Add to Cart" on a product page. Confirm the event fires immediately with `currency`, `value` (price × quantity), and `items` array matching the added product including `quantity`.
- **remove_from_cart**: Remove an item from the cart. Confirm the event fires with the same item parameters and the removed `quantity`.
- **begin_checkout**: Navigate to checkout step 1. Confirm the event fires with `currency`, `value` (cart total), and full `items` array.
- **add_payment_info**: After selecting a payment method in checkout, confirm the event fires with `payment_type` (e.g. `credit_card`, `paypal`) plus `currency`, `value`, `items`.
- **add_shipping_info**: After selecting a shipping method, confirm the event fires with `shipping_tier` (e.g. `standard`, `express`) plus `currency`, `value`, `items`.
- **purchase**: On the order confirmation page, verify the event fires exactly once with: `transaction_id` (unique order number), `value` (net or gross as configured, documented), `tax`, `shipping`, `currency`, `coupon` (if applicable), and `items` array with all purchased products including `quantity` and `price`.
- Reload the order confirmation page: confirm the `purchase` event does NOT fire again (deduplication in place — e.g., sessionStorage/cookie check or server-side rendered flag).
- Check that `item_id` values are consistent across all events — the same SKU format used everywhere (no mixing of internal IDs and EAN codes).

### Enhanced e-commerce tracking

- Confirm the GA4 property has Enhanced Measurement enabled for relevant automatic events (scrolls, outbound clicks, site search, video engagement, file downloads).
- Verify `view_promotion` fires when a banner/promo element enters the viewport, with `promotion_id`, `promotion_name`, `creative_name`, `creative_slot`.
- Verify `select_promotion` fires when a user clicks a promotional banner.
- Verify `view_item_list` fires for every product list (homepage widgets, cross-sell blocks, recently viewed) with the correct `item_list_name`.
- Verify `select_item` fires when a user clicks a product in any list, with matching `item_list_name` and `index` consistent with the preceding `view_item_list`.
- Confirm product `quantity` in `add_to_cart` updates correctly when the user changes the quantity selector before adding.
- In GA4 → Monetisation → E-commerce Purchases, confirm purchase data appears within 24 hours with correct revenue figures matching the order management system.
- Verify `refund` event fires (or is sent server-side) when an order is refunded in the backend, with `transaction_id` and optionally `items` for partial refunds.

### Conversion tracking (Google Ads, Meta Pixel)

- **Google Ads**: Confirm the Google Ads conversion tag fires on the order confirmation page with: `conversion_id`, `conversion_label`, `value` (matching the order total), `currency`, `transaction_id` (for deduplication).
- **Google Ads remarketing**: Confirm `AW-XXXXXXXXX/remarketing` tag fires on product pages with `dynx_itemid` (product ID), `dynx_totalvalue`, `dynx_pagetype` (`product`).
- **Meta Pixel (fbq)**: Confirm `fbq('track', 'Purchase')` fires on the confirmation page with `value`, `currency`, `content_ids` (array of SKUs), `content_type` (`product`), `num_items`.
- **Meta Pixel — PageView**: Confirm `fbq('track', 'PageView')` fires on every page, but ONLY after consent is granted.
- **Meta Pixel — ViewContent**: Confirm fires on product detail pages with `content_ids`, `content_name`, `content_category`, `value`, `currency`.
- **Meta Pixel — AddToCart**: Confirm fires when a product is added to the cart with matching parameters.
- **Meta Pixel — InitiateCheckout**: Confirm fires when the user enters the checkout with `num_items` and `value`.
- Verify deduplication: both browser-side Pixel and server-side Conversions API events should include the same `eventID` so Meta can deduplicate — confirm `eventID` is unique per event instance and matches between browser and server payloads.
- In Google Ads → Conversions, confirm the conversion action shows "Recording conversions" status within 48 hours of a test purchase.

### Google Tag Manager correct

- Open GTM Preview/Debug mode and load each key page type; confirm all expected tags fire and no tags fire with errors.
- Verify the GTM container ID (`GTM-XXXXXXX`) is present in both the `<head>` snippet and the `<noscript>` `<body>` snippet.
- Confirm the GTM container is NOT blocked by Content Security Policy (CSP): check for CSP errors in the browser console related to `googletagmanager.com`.
- Check that all GTM triggers are scoped correctly — no tags fire on every page that should only fire on specific pages (e.g., `purchase` tag must be scoped to the confirmation page only).
- Confirm GTM variables (e.g., `dataLayer` variables for `transactionId`, `transactionTotal`) are resolving correctly by inspecting their values in GTM Preview.
- Verify no deprecated Universal Analytics (UA) tags remain active in GTM.
- Confirm the GTM environment (Live, Staging) is set correctly for production — not publishing the Staging container to the live site.
- Check tag firing order: confirm Consent Initialization tags fire before any analytics/marketing tags in the tag sequencing.

### Server-side tracking (if applicable)

- Confirm the server-side GTM (sGTM) container URL is configured as the custom endpoint in the browser-side GA4 tag (e.g., `https://sst.yourdomain.com`).
- Verify that the sGTM container receives events by checking server logs or using the sGTM Tag Assistant; confirm event parameters are passed correctly without data loss.
- Test that the `purchase` event is sent from the server side directly after order creation, independent of the client-side tag (simulating a case where the user closes the browser before the confirmation page loads).
- Confirm HTTP response codes from the sGTM endpoint are `200` — log any `4xx`/`5xx` errors and verify retry logic is in place.
- Verify IP address anonymization: client IP should not be forwarded unmasked to third-party analytics endpoints via the server-side container.
- Confirm server-side Meta Conversions API sends matching `event_name`, `event_time`, `user_data` (hashed), and `eventID` for deduplication with the browser Pixel.
- Check that server-side events fire even when an ad blocker is active in the browser — purchase revenue should be captured regardless.

### Consent Mode v2 (Google)

- Before any consent interaction, open DevTools → Network and confirm that GA4 and Google Ads requests are sent with `gcs=G100` (analytics denied, ads denied) or fire in "cookieless ping" mode — no identifying cookies (`_ga`, `_gid`) should be set.
- After accepting all cookies, confirm `gcs=G111` (analytics granted, ads granted) appears in subsequent requests and `_ga` cookie is created.
- After rejecting all cookies, confirm GA4 still sends pings (for modelling) with `gcs=G100` but NO `_ga` cookie is set and no user identifier is transmitted.
- Verify Consent Mode is initialized BEFORE the GTM container loads by checking that `gtag('consent', 'default', {...})` (or equivalent CMP push) appears in the page source before the GTM `<script>` tag.
- Test the "accept only necessary" scenario: `analytics_storage: denied`, `ad_storage: denied`, `ad_user_data: denied`, `ad_personalization: denied` — confirm corresponding `gcs` parameter.
- Test partial consent: user grants `analytics_storage` but denies `ad_storage` — confirm `gcs=G110` and that remarketing tags do not fire with user identifiers.
- Confirm consent state is restored on subsequent page visits (persisted via cookie/localStorage by the CMP) — the user should not be prompted again on return visits within the consent validity period.
- Verify `url_passthrough: true` is configured (if used) and that `_gl` parameters in URLs correctly restore the client ID for cross-domain measurement without cookies.
- Confirm Google Signals is only active when consent is granted (`analytics_storage: granted`).

### Facebook/Meta Pixel

- Confirm the Meta Pixel base code is present in the `<head>` of every page and fires `PageView` on load — but only after consent.
- Use Meta Pixel Helper browser extension to verify the Pixel ID is correct and events are being received without warnings.
- Verify no duplicate Pixel IDs fire on any page (common GTM misconfiguration).
- Test `AddToCart` event: add a product, then check Meta Events Manager → Test Events tool to confirm the event appears with correct `value`, `currency`, `content_ids`.
- Test `Purchase` event in Meta Events Manager → Test Events: place a test order and confirm the event arrives with `value`, `currency`, `num_items`, `content_ids`, `order_id` (as `eventID`).
- Confirm Advanced Matching parameters (`em` = hashed email, `ph` = hashed phone) are passed in the Pixel when the user is logged in or has completed checkout — verify the values are SHA-256 hashed, not plaintext.
- Verify the Pixel does NOT fire when cookies are rejected — check that the GTM trigger or CMP integration blocks the tag correctly.
- Confirm there is no Pixel data leakage via `fbclid` URL parameters being passed to server logs in plain text containing PII.

### Pinterest Tag (if relevant)

- Confirm the Pinterest base tag fires `PageVisit` on all pages after consent, with correct Pinterest Tag ID.
- Verify `AddToCart` event fires with `product_id`, `product_name`, `value`, `order_quantity`, `currency`.
- Verify `checkout` event fires on the confirmation page with `value`, `order_quantity`, `currency`, `line_items` array.
- Confirm the tag does not fire before consent is granted.
- Use Pinterest Tag Helper or the Pinterest Ads Manager → Conversions panel to confirm events are received and attributed correctly.
- Check that `order_id` is passed with the `checkout` event for deduplication if Conversions API is also configured.

### Heatmap tool (Hotjar, Clarity)

- Confirm the heatmap script (Hotjar tracking code or Microsoft Clarity snippet) loads on the relevant page types (not necessarily all pages — e.g., excluded from checkout for PCI reasons).
- Verify the heatmap tool is blocked / not initialized when the user rejects analytics/targeting cookies.
- In Hotjar/Clarity dashboard, confirm a recording session appears after a manual test visit — check that the session is captured and the page renders correctly in the recording.
- Confirm sensitive fields (credit card numbers, passwords) are masked in session recordings — Hotjar masks `<input type="password">` by default; verify no custom CSS class suppression is missing on payment iframes.
- Verify heatmap tool does NOT capture content from payment iframes (Stripe Elements, PayPal buttons) — these are sandboxed and should appear blank in recordings.
- Check that the heatmap script does not significantly delay page load (no render-blocking): confirm it loads asynchronously and does not appear in the critical rendering path.
- Confirm IP anonymization is configured in the heatmap tool's settings if required by the privacy policy.

### A/B testing tool

- Confirm the A/B testing script (e.g., Google Optimize, VWO, Optimizely, AB Tasty) loads correctly and does not cause visible "flicker" (original content shown briefly before the variant).
- Verify the anti-flicker snippet (if used) is placed before the GTM/analytics snippet in `<head>`.
- Confirm that the A/B test variant assignment is consistent across page reloads for the same user (same variant shown on return visit within the test period).
- Verify that A/B test variant data is passed to GA4 — check the `dataLayer` for experiment ID and variant name being pushed, and confirm a GA4 custom dimension or event carries this information.
- Confirm A/B testing tool respects consent: no cookie-based assignment or tracking before consent is given.
- When an A/B test is concluded and disabled, confirm the winning variant (or original) is consistently shown to all users with no residual experiment code running.
- Verify that tracked conversions in the A/B testing tool match the GA4 purchase counts within an acceptable margin (< 5% discrepancy).

### Tracking disabled after consent rejection

- Reject all cookies on first visit. Open DevTools → Application → Cookies and confirm no analytics cookies are set (`_ga`, `_gid`, `_fbp`, `_gcl_*`, `_hjid`, etc.).
- Reject all cookies and confirm no network requests are made to `google-analytics.com`, `analytics.google.com`, `connect.facebook.net`, `hotjar.com`, or other tracking domains.
- Accept all cookies, then navigate to cookie settings and withdraw consent. Confirm: (1) existing analytics cookies are deleted or expire, (2) no new tracking requests fire after the opt-out.
- Confirm that strictly necessary cookies (session, cart, CSRF token) ARE still set even after full rejection.
- Test the "reject all" state across a browser session: navigate to multiple pages and confirm no tracking requests appear on any page.
- Confirm consent state persists across browser restarts (the consent cookie or localStorage entry is present with the correct expiry matching the privacy policy).
- Verify that withdrawing consent does not break the site's core functionality (cart, login, checkout must still work).

### Revenue tracking correct (currency, amounts)

- Place a test order for a product with a known price (e.g., EUR 49.99). In GA4 DebugView confirm the `purchase` event `value` parameter equals `49.99` (not `4999`, not `"49,99"`).
- Verify `currency` parameter is always a valid ISO 4217 three-letter code (`EUR`, `USD`, `GBP`) — not a symbol (`€`).
- Place an order with a coupon discount. Confirm `value` in the `purchase` event reflects the discounted total (or gross revenue as defined by the tracking spec), and `coupon` parameter contains the coupon code.
- Place an order with multiple items. Confirm `value` = sum of all `(items[n].price × items[n].quantity)` and the sum matches the order total (excluding or including shipping, as per the documented tracking specification).
- Confirm `tax` and `shipping` parameters in the `purchase` event are numeric and not null — verify they match the order confirmation displayed to the user.
- Test a B2B order (net pricing): confirm whether `value` is set to net or gross amount, verify this is consistent and documented in the tracking spec.
- In GA4 → Monetisation → E-commerce Purchases, run a daily report and cross-reference total revenue against the order management system for the same period — variance should be < 1% (accounting for ad-blocker impact if server-side tracking is absent).
- Verify the `transaction_id` is unique per order: place two test orders and confirm two distinct `transaction_id` values appear in GA4.

### Funnel tracking (checkout steps)

- Define and verify each funnel step in GTM/GA4: Step 1 = `begin_checkout`, Step 2 = `add_shipping_info`, Step 3 = `add_payment_info`, Step 4 = `purchase`. Confirm each event fires exactly once per checkout session at the correct step.
- In GA4 → Explore → Funnel exploration, build a funnel using the four events above and confirm the step-through rates are plausible (no 0% or 100% steps caused by tracking errors).
- Test checkout abandonment: start checkout, do not complete, then return in a new session. Confirm the re-entry is tracked as a new `begin_checkout` and the previous incomplete session is correctly counted as dropped off.
- Verify `checkout_step` custom parameter (if used) increments correctly: confirm step numbers in the `dataLayer` match the visible checkout progress indicator.
- Confirm that navigating backwards in checkout (browser back button or "Edit" link) does not re-fire `purchase` or double-fire earlier step events.
- Test the guest checkout path and the logged-in checkout path separately — confirm funnel events fire in both scenarios.
- If express checkout (Apple Pay, Google Pay, PayPal Express) is available, confirm it fires `begin_checkout` and `purchase` events as well — the express flow should not be a tracking blind spot.
- Verify the checkout funnel in Google Ads linked to GA4: confirm the funnel steps are importable as GA4 conversion events and appear in Google Ads reporting.
- Check the `dataLayer` after each checkout step using GTM Preview: confirm the correct ecommerce object is present before each tag fires, with no stale data from a previous step (ecommerce object should be cleared between pushes: `dataLayer.push({ ecommerce: null })`).
