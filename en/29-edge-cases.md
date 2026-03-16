# 29. Edge Cases / Special Scenarios

### Product becomes unavailable during checkout

- Add a product with stock level 1 to the cart. While the checkout is open in browser tab A, simulate a second purchase of the same product in tab B that depletes the stock. Attempt to complete the order in tab A. Expected: The system detects the stock depletion before payment is processed and shows a clear error message. The customer is not charged.
- Add a product to the cart. Before reaching payment, mark the product as "disabled" or "out of stock" in the admin panel. Click "Place Order". Expected: The order is rejected with an informative message. The customer is returned to the cart page where the unavailable item is clearly marked.
- Add a product to the cart. Before completing checkout, change the product's stock to 0 via the admin backend or API. Proceed through checkout and attempt to complete payment. Expected: Payment is not captured. Order is not created. The customer receives a message explaining the item is no longer available.
- Add two units of a product with stock level 1 to the cart. Expected: Either the cart prevents adding more than the available quantity, or a validation error appears before or during checkout that corrects the quantity.
- A product is discontinued mid-checkout (variant deleted, not just out of stock). Attempt to place the order. Expected: Graceful error handling — no orphaned payment intents, no corrupted order records.
- A bundle or kit contains a component that goes out of stock during checkout. Expected: The entire bundle is treated as unavailable, and the customer is notified before payment is captured.
- After the unavailability error, verify the customer's payment method was not charged. Check payment gateway logs to confirm no successful charge exists.
- After the unavailability error, verify the cart is updated to reflect current availability — the out-of-stock item is either removed or marked with a warning rather than silently remaining.

### Price changes during checkout

- Add a product to the cart. While on the payment page, change the product price in the admin backend (e.g., from €49.99 to €59.99). Complete the order. Expected: Either the order is placed at the price valid at cart-creation time (price locked), or the customer is notified of the price change and must confirm before completing the order. The payment amount must match the displayed amount.
- Apply a coupon to an order. While on the payment page, expire or delete the coupon in the admin backend. Complete the order. Expected: The system re-validates the coupon at order submission. If invalid, the discount is removed and the customer is informed before payment.
- Add a product to the cart. While at the payment step, activate a sitewide sale that reduces the product price. Complete the order. Expected: Consistent behavior — either the new lower price is applied, or the originally displayed price is honored. The customer should not be charged more than the amount shown on the payment page.
- Add a product to the cart. While at the payment step, remove a sale price in the admin (price increases). Complete the order. Expected: The customer is not silently charged more than the price they saw. Either the original price is honored or the customer is warned.
- Verify that the amount charged by the payment provider (Stripe/PayPal) exactly matches the total shown on the order confirmation page. A mismatch here is a critical bug.
- Place an order with a tiered shipping rate. Change the shipping rate configuration during checkout. Expected: The shipping cost shown to the customer is the cost charged — no silent changes.
- Check audit logs or order records: the stored order total should match the payment capture amount and the amount displayed in the confirmation email.

### Session expires during checkout

- Begin checkout as a guest, fill in all address fields, proceed to payment. Wait for the session to expire (simulate by deleting session cookies or waiting past the timeout). Attempt to complete the order. Expected: The user is shown a clear message that the session has expired. They are redirected to restart checkout, ideally with form data retained or a warning about what was lost.
- Begin checkout as a logged-in user, proceed to the payment page, let the session expire, then click "Place Order". Expected: The user is prompted to re-authenticate. After logging in, they should be returned to checkout — not sent to the homepage — and their cart should still be intact.
- During checkout, let the session expire mid-form-fill (address step). Expected: Form inputs already entered are not silently lost. Either client-side persistence (localStorage/sessionStorage) preserves them, or a clear message informs the user what they need to re-enter.
- Simulate session expiry exactly when the "Place Order" button is clicked. Expected: No duplicate order is created. No payment is captured. The user receives a recoverable error, not a silent failure.
- Verify that a session expiry during checkout does not leave orphaned payment intents or partial order records in the database.
- After session expiry, confirm that the cart contents are still available when the user re-authenticates or starts a new session (for logged-in users whose cart is server-persisted).
- Test with an extremely short session timeout (e.g., 1 minute) to verify the expiry handling works reliably, not just with default timeouts.

### Browser back button in checkout

- On the order confirmation page (after a successful purchase), press the browser back button. Expected: The user is NOT taken back to the payment form with the order still submittable. They should see a neutral page (e.g., cart) or a message that the order was already placed, preventing duplicate orders.
- On the payment step of checkout, press back to return to the shipping step. Change the shipping method. Expected: The system correctly recalculates shipping costs and updates the payment intent amount. The user is not charged the original amount.
- Complete a multi-step checkout (address → shipping → payment → confirmation). Use back navigation to return to the address step and change the country. Expected: Shipping costs and available payment methods are recalculated correctly for the new country.
- On the payment step, press back, then immediately press forward (browser forward button). Expected: The payment form is not auto-submitted. No duplicate payment attempt is triggered.
- Press back from checkout to the product page, change a product option or quantity, then use the forward button to return to checkout. Expected: The cart reflects the change, and the checkout totals are updated accordingly.
- After a failed payment, press back to return to the payment form. Expected: The form is pre-filled appropriately (or cleared for security), and the user can attempt payment again without creating a second order record.
- Navigate back from checkout to the cart, remove an item, then proceed to checkout again. Expected: The checkout reflects the updated cart. No ghost items from the previous checkout session appear.

### Double-click on "Place Order"

- Click the "Place Order" button twice in rapid succession. Expected: Exactly one order is created. Exactly one payment is captured. The server-side implementation must use idempotency keys or database-level deduplication to prevent duplicate charges.
- Verify via the payment gateway dashboard (Stripe/PayPal) that only one payment attempt was made after a double-click. Two successful charges for the same order is a critical bug.
- Simulate a slow server response (throttle network in DevTools to 3G) and double-click "Place Order". Expected: The button is disabled after the first click, preventing a second submission regardless of server response time.
- Check that the "Place Order" button is visually disabled or replaced with a loading spinner immediately after the first click. A user should receive clear visual feedback that the action is in progress.
- After the first click, attempt to trigger form re-submission via keyboard (pressing Enter while focus is on the button). Expected: No duplicate submission.
- After double-clicking, verify the order confirmation page shows one order number — not two, and not a generic error caused by a database constraint violation from the duplicate attempt.
- Test with JavaScript disabled: verify the form has server-side idempotency protection that prevents duplicates even without client-side button disabling.

### Very long product name

- Create a product with a name of 255+ characters. View the product listing page. Expected: The name is truncated with an ellipsis (or equivalent) without breaking the grid layout. No text overflows outside its container.
- View the product detail page for a product with a very long name. Expected: The name wraps correctly in the H1, does not overlap the price or add-to-cart button, and does not cause horizontal scrolling.
- Add a product with a very long name to the cart. View the cart page and mini-cart. Expected: The name is truncated or wrapped without distorting the cart layout.
- Proceed through checkout with a very long product name in the cart. View the order summary panel. Expected: The name fits within its designated area. No layout breakage on mobile.
- View the order confirmation email for an order containing a product with a very long name. Expected: The name wraps correctly in the email template and does not break the email layout in common clients (Gmail, Outlook, Apple Mail).
- View the order in the admin backend. Expected: The long product name does not break the admin order detail view or order list table.
- Add a product with a name containing 255+ characters to the wishlist. View the wishlist page. Expected: Correct display without overflow.
- Test a product name consisting entirely of a single long word without spaces (e.g., a URL-like string). Expected: CSS `word-break` or `overflow-wrap` handles the overflow correctly.

### Very many variants (100+ options)

- Load a product page that has 100+ size or color variants. Measure the page load time. Expected: Load time does not significantly degrade compared to a simple product. The variant selector renders without freezing the browser.
- On a product with 100+ variants, select a variant via the selector. Expected: Price, availability, and image update within 300ms without perceptible lag.
- Scroll through a dropdown or swatch selector with 100+ options on mobile. Expected: The UI remains scrollable and responsive.
- On a product with 100+ variants where many are out of stock, verify that out-of-stock variants are visually distinguished (grayed out, strikethrough) without causing render performance issues.
- Add a product with a specific variant selected (from a set of 100+) to the cart. Verify the cart shows the correct variant (correct SKU, correct attributes).
- Search for a product with 100+ variants using the site search. Expected: The product appears in results without performance issues related to variant count.
- On a product with a combination configurator (e.g., size × color = 100+ combinations), select an invalid combination. Expected: The system correctly identifies and communicates unavailability without lengthy processing delays.
- Test the admin product page for a product with 100+ variants. Expected: The admin UI loads and remains manageable (pagination or lazy loading of variants if necessary).

### Empty cart → go directly to checkout

- With an empty cart, manually navigate to the checkout URL (e.g., `/checkout`). Expected: The user is redirected to the cart page or homepage with a message explaining the cart is empty. The checkout page itself is NOT rendered.
- With an empty cart, attempt to navigate to the checkout URL. Verify no server errors (500) occur — the redirect is handled gracefully.
- Complete an order successfully, then use the browser back button to return to the checkout page. Expected: The checkout page detects that the cart is now empty (or the order is already placed) and redirects appropriately.
- Log in as a user with an empty saved cart. Attempt to access the checkout URL directly. Expected: Same behavior as above — redirect with a clear message.
- Test with a manipulated cart session: set the cart total to €0 manually (no items, no coupon) and submit the checkout form. Expected: Server-side validation catches the empty cart and rejects the submission.
- After checkout completion, all items should be removed from the cart. Navigating back to checkout should show the empty cart message, not a summary of the just-placed order.

### Cart with 50+ items

- Add 50 different products to the cart. Load the cart page. Expected: All 50 items are displayed correctly. The page loads within an acceptable time (under 3 seconds). No items are silently dropped.
- Add 50+ items to the cart and proceed to checkout. Verify the order summary on the checkout page correctly lists or summarizes all items and shows the correct total.
- With 50+ items in the cart, apply a percentage discount coupon. Expected: The discount is calculated correctly across all items, and the total is accurate to the cent.
- With 50+ items, apply a category-specific coupon. Expected: The discount is applied only to eligible items, and the calculation is correct even with a large number of line items.
- Place an order with 50+ items. Verify the order confirmation email renders correctly without truncating items or displaying garbled totals.
- With 50+ items in the cart, change the quantity of one item. Expected: Only that item's quantity changes; the rest remain unchanged. The cart total updates correctly.
- Test the remove-all-items (clear cart) function with 50+ items. Expected: All items are removed in a single action. The cart shows as empty immediately.
- Add the maximum quantity of one product (e.g., 99) plus 49 other products. Expected: No cart overflow error. Totals remain correct.

### Order with 0 EUR (coupon covers everything)

- Apply a coupon that exactly covers the total order value (subtotal + shipping = coupon discount). Expected: The order total shows €0.00. The "Place Order" flow completes without requiring payment method entry, or the payment step is skipped. No payment is captured.
- When placing a €0 order, verify no payment intent is created at the payment provider. No authorization for €0 should appear in Stripe/PayPal unless that is explicitly expected behavior.
- Place a €0 order successfully. Verify the order is created in the admin backend with status "Paid" (or an equivalent zero-payment status). An order confirmation email is sent.
- Place a €0 order as a guest. Expected: The order is created and the confirmation email is sent, even without a payment transaction ID.
- Verify the invoice for a €0 order is generated correctly, showing the full original amount, the coupon discount, and €0.00 total due.
- If the €0 coupon is a one-time-use coupon, verify it is marked as used after the order is placed, even though no payment was made.
- Test a €0 order where the coupon covers the product cost but not shipping. Expected: Only the shipping amount is charged — the system correctly separates the coupon discount from the shipping cost.

### Coupon higher than order value

- Enter a coupon code worth €50 on a cart with a total of €20. Expected: The cart total is displayed as €0.00 (no negative totals). The system does not attempt to charge a negative amount or issue an automatic refund of the difference.
- Verify that the order record stores the applied discount correctly (e.g., discount: €20, not €50). The coupon's face value should not be recorded as the actual discount if it exceeds the order total.
- Apply a coupon that exceeds the order total to a cart. Remove the coupon. Expected: The cart total returns to the correct original amount without errors.
- Apply a coupon that exceeds the order total. Then add more items to the cart until the total exceeds the coupon value. Expected: The coupon is still applied, and the remaining balance is charged correctly.
- A coupon that exceeds the order total on a cart with no items (empty cart): attempting to apply it should either be blocked (cart is empty) or have no visible effect.
- Verify the display: the cart should show the original total, a discount line (capped at the order total), and a final €0.00 — not a negative line showing "Discount: -€50" on a €20 cart without a €0 floor.
- Check that the invoice generated for such an order correctly documents the applied discount without showing a negative "Amount Due".

### Special characters in names/addresses (umlauts, accents)

- Place an order with a first name containing umlauts: "Jörg Müßig-Schäfer". Verify the name is stored correctly in the database (UTF-8), displayed correctly in the order confirmation, and printed correctly on the PDF invoice.
- Use an address with a French accent: "12 Rue de l'Église, Châlons-en-Champagne". Verify it appears correctly at every stage: cart summary, checkout summary, confirmation email, admin backend, shipping label generation.
- Enter a customer name with Eastern European characters: "Ján Novák" or "Katarzyna Wiśniewska". Verify correct storage and display throughout the entire order lifecycle.
- Use special characters in the address field that could be misinterpreted as HTML or SQL: `O'Brien`, `<script>test</script>`, `'; DROP TABLE`. Expected: Characters are properly escaped — the name is stored and displayed literally, no XSS or injection occurs.
- Enter an email address with a plus sign: `customer+shop@example.com`. Verify the confirmation email is sent to this address correctly.
- Enter an address with a very long German compound street name: "Donaudampfschifffahrtsgesellschaftsstraße 42". Verify it fits in the address fields and is not truncated in emails or PDF documents.
- Test a company name with an ampersand: "Müller & Söhne GmbH". Verify the ampersand is not HTML-encoded as `&amp;` in plain-text email outputs.
- Use Arabic or Cyrillic characters in address fields (if the shop is international). Verify correct UTF-8 storage and display, and that PDF invoices render these characters correctly (font support).

### Very long addresses

- Enter a street address of 100+ characters in the address field. Expected: The field accepts the input (if within the database column limit), it is stored and displayed without truncation in emails and admin views.
- Enter a company name of 100+ characters. Verify it appears correctly on the PDF invoice and shipping label without overflowing the designated print area.
- Enter a city name of 50+ characters (real examples exist: "Llanfairpwllgwyngyllgogerychwyrndrobwllllantysiliogogogoch" in Wales). Expected: The checkout form accepts this input, and it is stored and displayed correctly.
- Enter the maximum allowed characters in every address field simultaneously. Verify the checkout form submits successfully and the order is created without database truncation errors.
- Enter a very long address in the "Additional address line" or "Address line 2" field. Verify it is included in shipping API calls (if applicable) and on the PDF invoice.
- Test a very long address in combination with special characters (accented letters + long string). Verify combined handling is correct.
- Enter a postal code in a format that doesn't match the expected pattern for the selected country (e.g., letters in a German postal code field). Expected: Validation rejects this with a clear message before submission.

### Simultaneous purchase of same item (race condition)

- Open the same product page (stock: 1) in two different browsers or incognito windows simultaneously. Add to cart in both and proceed to checkout simultaneously. Attempt to place the order in both tabs at approximately the same moment. Expected: Exactly one order succeeds. The other receives a "no longer available" error. Verify via the database and payment gateway that only one payment was captured.
- Simulate a race condition using an automated script: send two simultaneous POST requests to the order placement endpoint for the same stock-1 product. Expected: The database transaction locks prevent both from succeeding. Verify using database-level checks (e.g., `SELECT COUNT(*) FROM orders WHERE product_id = X`).
- After a race condition test where one order succeeds, verify the stock level is correctly reduced to 0 and not a negative number.
- After the failed concurrent order, verify the losing customer receives a clear error message — not a generic server error or a timeout.
- Test with a product that has stock level 5 and 10 simultaneous purchase attempts for 1 unit each. Expected: Exactly 5 orders succeed; 5 fail with a stock depletion message. Total units sold must not exceed 5.
- Verify the payment provider was not charged for the failed concurrent order. If a payment intent was created but the order failed, verify the intent is cancelled.
- After the race condition test, check admin order history: no partial or zombie orders should exist for the failed concurrent attempt.

### Slow internet connection

- Throttle the browser network to "Slow 3G" (via DevTools). Load the homepage. Expected: The page is usable — critical content (navigation, product images above fold, add-to-cart button) loads within a reasonable time. No JavaScript errors caused by resource load timeouts.
- Throttle to Slow 3G and attempt to place an order. Click "Place Order" once. Expected: A loading indicator appears immediately (before the server responds), preventing the user from thinking the click did nothing. The button is disabled to prevent double-submission during the slow response.
- Throttle to Slow 3G and upload a file (e.g., a custom design image) on a product configurator page. Expected: An upload progress indicator is shown. The upload does not silently time out without feedback.
- Throttle to Slow 3G and apply a coupon code. Expected: A loading spinner appears. If the request takes more than 5 seconds, a timeout message is shown — the user is not left waiting indefinitely.
- Throttle to Slow 3G and attempt to load a large product image gallery. Expected: Images load progressively (lazy loading). The page layout does not shift drastically as images load (low CLS score).
- Throttle to Slow 3G and attempt checkout. If the session times out due to the slow connection, verify the behavior matches the "Session expires during checkout" test cases above — no duplicate charges.
- Test with complete network loss mid-checkout (disable network in DevTools after the payment form is loaded). Expected: The payment submission fails gracefully with an error message, not a silent hang.

### Checkout abandonment + resumption

- Begin checkout (fill in address), then close the browser. Reopen the browser and navigate to the site. Expected: For a logged-in user, the cart is still intact and checkout can be resumed from where it was left off. For a guest, the cart should persist if session cookies are still valid.
- Begin checkout and abandon it at the payment step. Return 30 minutes later. Expected: The cart and entered address information are still present. No orphaned payment intents exist in the payment gateway (or they are handled cleanly).
- Begin checkout, abandon at the shipping step, and return via a cart abandonment email link (if applicable). Expected: The link takes the user directly back to their cart, not a generic page.
- Begin checkout as a guest, abandon, then return and log in. Expected: The guest cart is merged with the logged-in account's cart (if applicable), and checkout can be resumed.
- Complete checkout to the payment step, abandon the tab, then reopen a fresh browser session and navigate to the checkout URL. Expected: The checkout requires re-entering the cart or re-authenticating — the incomplete order is not accidentally completed.
- Abandon checkout after a payment pre-authorization was created (e.g., 3D Secure challenge triggered but not completed). Verify the authorization is voided or expires correctly without leaving a dangling hold on the customer's card.
- After an abandoned checkout is resumed and completed, verify only one confirmation email is sent — not multiple emails triggered by various abandonment and completion events.

### Multiple tabs open simultaneously

- Open the product page in two tabs. Add to cart in both tabs separately. Expected: The cart contains both additions (or the quantity is doubled for the same product) — depending on business logic. No items are silently lost.
- Open the cart in tab A. Add a product in tab B. Switch back to tab A without refreshing. Expected: Either the cart is auto-updated (WebSocket/polling), or when the user interacts with tab A's cart, a refresh or warning prompts them to reload.
- Begin checkout in tab A. Add a product to cart in tab B. Expected: The checkout in tab A either updates its summary automatically or warns the user that the cart has changed before allowing order submission.
- Open checkout in two tabs simultaneously. Fill in the address in tab A and submit. Then submit in tab B. Expected: No duplicate orders. The second submission should detect that the order was already placed.
- Open the login page in two tabs. Log in successfully in tab A. Attempt to submit the login form in tab B. Expected: Tab B recognizes the already-authenticated session gracefully, redirecting to the account page rather than showing an error.
- Open a session-sensitive page (e.g., "My Account") in two tabs. Log out in tab A. Switch to tab B and attempt an authenticated action. Expected: Tab B's action is rejected and the user is redirected to the login page — not a silent failure.
- Open checkout in tab A (logged in). Log out in tab B. Attempt to place the order in tab A. Expected: The order submission fails with an authentication error, not a server-side exception.

### Incognito mode

- Open the shop in an incognito/private window. Add products to the cart. Close and reopen the incognito window. Expected: The cart is empty (session cookie was not persisted). This is expected behavior — verify it is consistent and does not cause errors.
- Open the shop in incognito mode and complete a full checkout as a guest. Expected: The order is placed successfully, and a confirmation email is sent. Incognito mode should not block any checkout functionality.
- Open in incognito mode and attempt to log in to an existing account. Expected: Login works correctly. Session is maintained for the duration of the incognito session.
- Test the cookie consent banner in incognito mode. Expected: The banner appears (as if visiting for the first time). Previously set consent preferences are not inherited.
- Test "Keep me logged in" / remember-me functionality in incognito mode. Expected: The persistent login does not work after the incognito window is closed (this is the expected browser behavior — verify the site handles it gracefully without errors).
- In incognito mode, add to wishlist (if cookie-based). Close and reopen the incognito window. Expected: The wishlist is empty. No errors are thrown.
- Use a payment method (e.g., PayPal) that opens a popup window in incognito mode. Expected: Popup blockers or third-party cookie restrictions in incognito mode are handled gracefully, with a fallback or a clear message to the user.

### Cookies disabled

- Disable all cookies in the browser settings. Load the homepage. Expected: The site loads without a JavaScript crash. A message may inform the user that cookies are required for certain functionality.
- With cookies disabled, attempt to add a product to the cart. Expected: Either a cookie-based cart is not available and a clear error message is shown, or a fallback mechanism (URL-based cart, warning to enable cookies) is in place.
- With cookies disabled, attempt to log in. Expected: Login fails (sessions require cookies) and a clear, user-friendly message explains that cookies must be enabled for login to work.
- With cookies disabled, proceed as far as possible through checkout. Expected: No server-side exceptions or blank pages. The user receives an actionable error explaining why the checkout cannot be completed.
- With cookies disabled, verify that the cookie consent banner itself still renders correctly — it should not depend on a cookie to display.
- With cookies disabled, verify that no sensitive data is leaked through URL parameters as a fallback (e.g., session IDs in query strings), which would be a security vulnerability.

### JavaScript disabled (graceful degradation)

- Disable JavaScript in the browser (via DevTools or browser settings). Load the homepage. Expected: The page content (products, navigation, text) is still readable and navigable. Basic HTML navigation links work.
- With JavaScript disabled, attempt to use the search functionality. Expected: Either the search form submits via a standard HTML GET request and returns results on a new page, or a clear message explains that JavaScript is required.
- With JavaScript disabled, attempt to add a product to the cart. Expected: Either the add-to-cart form submits via a standard HTML POST request, or a visible message explains JavaScript is required for cart functionality.
- With JavaScript disabled, view the product configurator. Expected: Either static fallback inputs (HTML `<select>`, `<input>`) are available, or a clear non-JavaScript-dependent message explains the requirement.
- With JavaScript disabled, navigate to the checkout URL. Expected: At minimum, a readable error page or a redirect — not a blank screen or unhandled exception.
- With JavaScript disabled, verify that no critical security features (CSRF tokens in forms, required field validation fallbacks) are solely dependent on JavaScript. Server-side validation must catch all edge cases.
- Verify that the site does not display broken layout or overlapping elements when JavaScript fails to load (e.g., due to a CDN outage), rather than because of being explicitly disabled.

### Ad blocker active

- Enable a common ad blocker (uBlock Origin, AdBlock Plus) and load the homepage. Expected: The site loads correctly without broken layouts. No critical UI elements are blocked.
- With an ad blocker active, proceed through a full checkout. Expected: The checkout completes successfully. The payment widget (Stripe Elements, PayPal SDK) is not blocked by the ad blocker — these are typically not blocked, but verify this.
- With an ad blocker active, verify the cookie consent banner appears and functions correctly. Some CMPs (Consent Management Platforms) are blocked by ad blockers.
- With an ad blocker active, verify that tracking scripts (GA4, Meta Pixel) are blocked silently — their absence should not cause JavaScript errors that break site functionality.
- With an ad blocker active, verify that the live chat widget either loads or fails silently — a blocked chat widget must not cause an uncaught JavaScript exception that disrupts the page.
- With an ad blocker active, verify the trust badge / review widget (Trustpilot, Trusted Shops) either loads or degrades gracefully without a visual hole or JavaScript error.
- With an ad blocker active, test the product page for any third-party embed (product video, 360-degree view) that might be blocked. Expected: A fallback image or placeholder is shown, not a blank space or broken element.
- Verify that the Google Fonts or CDN-hosted icon fonts (Font Awesome) are either self-hosted or have a fallback so that ad blockers blocking CDN requests do not cause icon display failures.
