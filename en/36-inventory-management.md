# 36. Inventory Management

### Real-time inventory synchronization

- Verify that a stock change made directly in the database (or via ERP/admin) is reflected on the product page within the defined sync interval (e.g., ≤ 5 seconds for real-time, ≤ 15 minutes for batch sync).
- Simulate an ERP push update for a product and confirm the updated stock level appears in the storefront without a full page reload or cache bust being required.
- Trigger a stock update via the admin backend and verify the change is visible simultaneously on product page, category page, and search results.
- Test sync behavior when the ERP/warehouse system is temporarily unavailable: confirm the shop does not show stale "in stock" data indefinitely and surfaces an appropriate fallback or warning.
- Perform a stock update during peak traffic (50+ concurrent users on the same product page) and verify all sessions receive the updated availability status, not a cached outdated value.
- Confirm that stock synchronization logs are written (timestamp, source system, product SKU, old value, new value) and are accessible in the admin log viewer.
- Verify that bulk stock imports (CSV/ERP batch) update inventory atomically — no partial states where some variants are updated and others are not mid-import.
- Test sync with variant-level granularity: update stock for only one color/size combination and confirm other variants are unaffected.
- Confirm that a webhook or callback from the warehouse system (e.g., after physical inventory count) triggers a full re-sync and the storefront reflects the corrected quantity within the expected timeframe.
- Verify that concurrent sync events from multiple sources (admin UI + ERP push simultaneously) do not cause a race condition resulting in an incorrect final stock value (test with database-level locking or optimistic concurrency).

### "Sold out" display when stock = 0

- Set a product's stock to exactly 0 in the backend and verify the "Sold out" label appears on the product page, category listing, search results, and wishlist within one page refresh.
- Confirm the "Add to Cart" and "Buy Now" buttons are disabled or hidden when stock = 0, and that no quantity can be entered.
- Verify that a sold-out product cannot be added to the cart via direct API call (e.g., `POST /cart/add` with `product_id` and `qty=1`) — the server must reject the request with an appropriate error.
- Test that the "Sold out" state persists correctly after a browser hard refresh (no stale cache serving an "In Stock" badge).
- Verify that a product showing "Sold out" is correctly excluded from "In Stock" filters on category pages.
- Confirm that structured data (Product Schema `availability` field) switches to `https://schema.org/OutOfStock` when stock = 0.
- Test the transition from stock = 1 to stock = 0 triggered by a purchase: verify the "Sold out" badge appears on the product page immediately after the order is confirmed, without requiring admin intervention.
- Verify behavior in the cart: if an item in the cart becomes sold out (stock drops to 0 while the customer is browsing), confirm the cart page flags the item as unavailable before the customer proceeds to checkout.
- Confirm that sold-out products cannot be reordered from order history without a stock check at the time of the reorder action.
- Test that a "Sold out" product still renders its full product page (title, description, images, SEO tags) and does not 404 or redirect.

### "Only X left in stock" notice for low stock

- Define the low-stock threshold (e.g., stock ≤ 5) in the admin settings and verify the "Only X left in stock" message appears on the product page exactly when stock hits that threshold.
- Verify the displayed number X is accurate and matches the actual database stock value — not a static marketing string.
- Confirm the low-stock notice updates dynamically: if stock decreases from 5 to 3 due to purchases during the session, the notice reflects "Only 3 left" on the next page load or in real time (depending on implementation).
- Test that the low-stock notice disappears when stock rises above the threshold (e.g., after a return or stock replenishment) and does not remain visible indefinitely due to caching.
- Verify the low-stock threshold is configurable per product and per category (global default overridable at product level).
- Confirm the low-stock message is not shown when stock is above the threshold, even by 1 unit (boundary value: threshold = 5, stock = 6 → no message, stock = 5 → message shown).
- Test with stock = 1: verify the message reads "Only 1 left in stock" (singular, not "1 items") — check for grammatical correctness.
- Verify the low-stock notice appears consistently on product page, quick-view modal, cart line item, and wishlist entry.
- Confirm the low-stock warning is not shown if the product is set to "allow backorders" mode — in that case overselling is intentional.
- Test that the low-stock badge is correctly excluded from products with infinite/untracked stock (stock tracking disabled).

### Prevention of overselling (concurrency: 2 users buy last item simultaneously)

- Set stock to 1. Open two browser sessions simultaneously. Both sessions add the product to the cart at the same second. Proceed both to checkout and attempt to place the order at the same time. Verify only one order succeeds; the second receives a clear "item no longer available" error before or at payment confirmation.
- Repeat the above scenario with automated load testing (e.g., 10 concurrent `POST /order/place` requests for the same product with stock = 1) and verify the database ends with stock = 0 and exactly 1 successful order.
- Verify the stock decrement uses a database-level atomic operation (e.g., `UPDATE products SET stock = stock - 1 WHERE id = ? AND stock > 0`) rather than a read-then-write pattern that is vulnerable to race conditions.
- Test pessimistic locking: if the implementation uses row-level locks, verify that a second transaction attempting to decrement stock while the first holds the lock waits and then correctly fails (not corrupts) when the first transaction commits and stock is 0.
- Test optimistic locking: if the implementation uses a version/timestamp column, verify that the second concurrent write fails with a conflict error and does not silently overwrite the first.
- Confirm that the error message presented to the losing customer is user-friendly ("Sorry, this item just sold out") and does not expose technical details.
- Verify the cart still contains the item after the failed order attempt, allowing the customer to remove it or choose an alternative — do not silently empty the cart.
- Test the same race condition scenario for a product with stock = 2 and 3 simultaneous purchasers: verify exactly 2 orders succeed and the third fails.
- Confirm the race condition protection applies to variant-level stock, not just product-level (two users buying the last "Red / Size M" variant simultaneously).
- Verify that the race condition protection also applies to quantity changes in the cart: if a user increases cart quantity from 1 to 3 when only 2 units remain, the system must reject the change and display the correct maximum.

### Stock reservation during checkout (temporary lock)

- Add a product with stock = 1 to the cart and begin checkout. Verify the item is temporarily reserved (stock decremented in a "reserved" state or a reservation record created) so a second user cannot purchase it during the checkout window.
- Confirm the reservation has a defined timeout (e.g., 15 minutes). After the timeout expires without order completion, verify the reservation is released and the stock becomes available to other buyers.
- Test the reservation release on explicit cart abandonment: if the first user removes the item from the cart during checkout, the reservation should be released immediately, not wait for the timeout.
- Verify reservation release on session expiry: if the customer's session times out during checkout, the reserved stock is returned to available inventory.
- Test that the reserved item is not shown as "In Stock" to other users during the reservation window — the product page should show "Sold out" or "Temporarily unavailable."
- Confirm that the reservation system handles concurrent reservations for the same product: if stock = 2 and two users are in checkout simultaneously, both reservations succeed; a third user entering checkout should be blocked.
- Verify that completing the checkout converts the reservation into a permanent stock decrement and the reservation record is removed cleanly (no orphan reservations accumulating in the database).
- Test reservation behavior when a payment fails: the reservation must be released so the customer can retry and so other customers can purchase the item.
- Confirm reservation cleanup is handled by a scheduled job or cron task and verify the job runs on schedule — test by disabling the job and confirming expired reservations pile up, then re-enabling it to confirm cleanup occurs.
- Test that two tabs in the same browser (same session) do not create two separate reservations for the same item, leading to a doubled "reserved" quantity.

### Automatic stock reduction after purchase

- Place a completed order for quantity 3 of a product with stock 10. Verify the stock is decremented to exactly 7 in the database immediately after payment confirmation.
- Verify the stock reduction is triggered only after successful payment, not at cart add or order placement (unless the shop uses reservation-then-confirm flow — in that case verify the reservation converts to a confirmed decrement).
- Test stock reduction for an order containing multiple different products: confirm each product's stock is decremented correctly and independently.
- Test stock reduction for an order with multiple line items of the same product (e.g., 2 units of product A): confirm stock is decremented by the total ordered quantity, not by 1.
- Simulate a payment gateway timeout where the payment status is ambiguous: verify the system does not decrement stock until payment is confirmed, or implements a safe reconciliation process.
- Verify that if the order placement transaction fails (database error mid-process), stock is not decremented — the operation must be atomic (order record + stock decrement in the same transaction or with compensating logic).
- Confirm the stock reduction is logged with order ID, timestamp, product SKU, quantity before and after — verifiable in the admin log.
- Test with a payment method that has a delay (e.g., bank transfer / prepayment): stock should only be decremented after payment is confirmed, not when the order is placed with pending payment status.
- Verify stock reduction propagates to all sales channels simultaneously (if the shop feeds marketplaces like Amazon/eBay): the warehouse system should receive the decrement signal and update channel availability.
- Test back-to-back rapid orders (two orders placed 1 second apart for the same product): confirm both stock decrements are applied correctly without one overwriting the other.

### Stock increase after cancellation/return

- Cancel an order that has not yet shipped. Verify the ordered quantity is added back to the product's stock immediately upon cancellation confirmation.
- Process a partial cancellation (cancel 1 of 3 ordered items). Verify only the cancelled item's stock is restored, not all items in the order.
- Submit a return request for a delivered order and confirm the stock is not increased until the returned item is physically received and marked as "received" in the admin backend — not at the time the return request is submitted.
- Test the return-received workflow: mark a return as received in the admin and verify the stock increment is applied to the correct SKU and variant.
- Verify stock is not restored for returned items marked as "damaged" or "non-resalable" — the system should allow the admin to choose whether the returned item goes back to saleable stock.
- Test an automatic stock increase triggered by a webhook from the warehouse system (return received at warehouse) and confirm it matches the manually processed flow.
- Confirm the stock increase after cancellation is correctly logged (order ID, reason, SKU, quantity before/after, timestamp).
- Verify that a cancellation of an order that was placed during a reservation period correctly returns the stock to the available (not reserved) pool.
- Test concurrent cancellation and new purchase of the same SKU: user A cancels order (stock +1), user B simultaneously places order for the same SKU — verify the final stock is correct (net change: 0) without race conditions.
- Verify that a full order refund without a physical return does not automatically increase stock — the admin should decide whether the item is coming back.

### "Notify when available" feature

- On a sold-out product, verify the "Notify me when available" form is displayed in place of the "Add to Cart" button.
- Submit the notification form with a valid email address and confirm a success message is shown and the subscription is recorded in the backend.
- Submit the form with an invalid email address and verify client-side and server-side validation rejects it with a clear error message.
- Submit the form twice with the same email for the same product and verify the system prevents duplicate subscriptions (shows "You are already subscribed" or silently deduplicates).
- Restore stock for a sold-out product and verify the notification email is sent to all subscribers for that product within the expected timeframe.
- Verify the notification email contains the product name, a direct link to the product page, and an unsubscribe/manage link — confirm it does not contain irrelevant marketing content unless the user consented.
- Test the notification for a variant-specific sold-out item (e.g., "Blue / Size L"): confirm the notification is triggered only when that specific variant comes back in stock, not when a different variant of the same product is restocked.
- Verify that restocking with a quantity of 1 sends exactly the right number of notifications (or batches them) — avoid notifying 50 people when only 1 unit is available, creating a new oversell scenario.
- Confirm that notified customers who click the product link land on the correct product page with the correct variant pre-selected (if variant-level notification).
- Test that a subscriber who has already purchased the product is removed from the notification list automatically, or that the system correctly handles a re-subscription scenario.
- Verify GDPR compliance: the notification signup does not implicitly subscribe the user to marketing emails; consent is granular and documented.

### Variant-specific stock (color/size has own inventory)

- Create a product with 3 size variants (S: 5 units, M: 0 units, L: 10 units). Verify on the product page that selecting "M" shows a "Sold out" state, "S" shows available, and "L" shows available — each variant reflects its own stock level independently.
- Confirm the "Add to Cart" button is disabled specifically for the sold-out variant and enabled for available variants on the same product page without a reload.
- Verify that adding variant "S" to the cart decrements only the "S" variant stock and leaves "M" and "L" stock values unchanged.
- Test a purchase of the last unit of variant "L" (stock = 1 → 0) and confirm only "L" transitions to "Sold out"; "S" remains available.
- Verify the low-stock notice ("Only X left") is shown per variant: if S has 2 units and the threshold is 5, the notice appears when "S" is selected and not when "L" (10 units) is selected.
- Test variant availability updates in the cart: a customer adds "Red / M" (stock = 1) to the cart; a second customer buys the last "Red / M"; the first customer's cart should flag the item as no longer available before checkout.
- Verify the "Notify when available" signup is variant-aware: subscribing on a sold-out "Blue / XL" variant does not trigger a notification when "Blue / S" is restocked.
- Confirm the product-level availability display on category/search pages correctly aggregates variant stock — a product with at least one available variant should not show "Sold out" at the product level.
- Test bulk stock import for variants: import a CSV with updated stock for 50 variant combinations and verify each variant's stock is updated to the correct value without affecting unrelated variants.
- Verify that variant stock data is correctly included in structured data (e.g., `Offer` schema per variant) so search engines receive accurate availability signals.
- Test the race condition for variant-specific stock: two users simultaneously select and add to cart the last unit of "Green / S" — verify only one succeeds and the variant-level stock does not go negative.
