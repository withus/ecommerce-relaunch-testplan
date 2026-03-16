# 13. Coupons / Discounts / Promotions

### Percentage coupon

- Apply a valid 10% percentage coupon to a cart and verify the discount amount equals exactly 10% of the subtotal (before shipping).
- Apply a valid 50% coupon and verify the resulting total is not negative and VAT is recalculated correctly on the discounted amount.
- Apply a valid 100% coupon and verify the order total becomes 0.00, the checkout can still be completed, and no payment is charged.
- Apply a percentage coupon where the calculated discount would produce a fraction (e.g., 10% of €19.99 = €1.999) and verify correct rounding behavior (round half up / half down per shop policy).
- Apply an expired percentage coupon and verify a clear error message is displayed and no discount is applied.
- Apply a percentage coupon code with incorrect capitalization (e.g., `save10` vs `SAVE10`) and verify whether the system is case-insensitive or returns a clear error.
- Enter a percentage coupon code containing extra whitespace (leading/trailing spaces) and verify the system trims it or rejects it gracefully.
- Apply a percentage coupon that is restricted to a specific customer group (e.g., B2B) as a standard customer and verify the coupon is rejected.
- Apply a percentage coupon after changing the cart quantity and verify the discount recalculates correctly on the new subtotal.
- Verify the percentage discount is displayed as a line item in the cart, order summary, confirmation email, and invoice PDF.

### Fixed amount coupon

- Apply a €10 fixed coupon to a €50 cart and verify the total is exactly €40 (plus shipping where applicable).
- Apply a €10 fixed coupon to a €9 cart and verify the total is clamped at €0.00, no negative amount is charged, and checkout can be completed.
- Apply a €10 fixed coupon to a cart where the coupon value exactly equals the cart value (€10 cart + €10 coupon) and verify the total is €0.00.
- Apply a fixed coupon to a cart containing products with mixed VAT rates and verify the discount is distributed across tax buckets proportionally (or per shop/legal policy).
- Apply a fixed amount coupon that exceeds the order value and verify that the overage is not issued as store credit or refund unless explicitly configured.
- Apply a fixed coupon in a currency other than the shop's base currency (if multi-currency) and verify the correct currency-converted amount is deducted.
- Verify that a fixed coupon is clearly labeled (e.g., "Voucher SAVE10: -€10.00") in the cart summary, checkout, confirmation email, and invoice.
- Apply the same fixed coupon code twice in the same cart session and verify the second application is rejected with an appropriate message.
- Remove a fixed coupon from the cart and verify the total reverts to the original amount instantly.
- Apply a fixed coupon and then remove an item from the cart so the subtotal drops below the minimum order value threshold; verify the coupon is invalidated and the customer is notified.

### Free shipping coupon

- Apply a free shipping coupon and verify all shipping method costs are reduced to €0.00.
- Apply a free shipping coupon and verify the cheapest shipping method is made free while express shipping still shows its surcharge (or all methods are free — per policy).
- Apply a free shipping coupon when the cart already qualifies for free shipping (above threshold) and verify no error occurs and the total is correct.
- Apply a free shipping coupon and then change the shipping country to one with a surcharge (e.g., island delivery) and verify whether the coupon still covers the full cost.
- Apply a free shipping coupon via the cart coupon field and verify the shipping cost in the checkout step updates immediately.
- Apply an expired free shipping coupon and verify the error message specifically states the coupon is no longer valid.
- Apply a free shipping coupon to an order that contains a bulky-goods or special surcharge and verify whether the surcharge is also waived or only the standard shipping fee.
- Verify the free shipping coupon is reflected in the order summary, confirmation email (shipping line shows €0.00), and invoice.
- Remove a free shipping coupon mid-checkout and verify the shipping cost is immediately restored.
- Apply a free shipping coupon in a session where express checkout (Apple Pay / Google Pay) is used and verify the discounted shipping is correctly communicated to the payment sheet.

### Minimum order value coupon

- Apply a coupon with a minimum order value of €50 to a cart worth €49.99 and verify the coupon is rejected with a message stating the minimum requirement (ideally showing the missing amount).
- Apply the same coupon to a cart worth exactly €50.00 and verify it is accepted (boundary condition — inclusive minimum).
- Apply the coupon to a cart worth €50.00, then reduce the quantity so the cart drops below €50, and verify the coupon is automatically invalidated and the customer is notified.
- Verify whether the minimum order value check is applied to the subtotal before or after other discounts (e.g., sale prices) and document the expected behavior.
- Apply a coupon with a minimum order value to a cart where the minimum is only met if shipping costs are included in the total, and verify the system uses the correct base for comparison.
- Apply a coupon with a minimum order value and then add more items to reach the threshold mid-session; verify the coupon becomes valid without requiring re-entry.
- Display a dynamic hint in the cart ("Add €X more to use coupon SUMMER20") and verify the amount updates correctly as items are added or removed.
- Apply a minimum order value coupon to a cart containing only items that are excluded from promotions; verify the coupon is rejected even if the subtotal of non-excluded items meets the threshold.
- Verify the minimum order value check respects the currency and locale (e.g., a €50 minimum does not accidentally apply as $50 in a multi-currency shop).

### Single-use vs. multi-use coupon

- Apply a single-use coupon code that has already been redeemed (by the same or any customer) and verify it is rejected with a clear "coupon already used" error message.
- Apply a single-use coupon code that has not yet been redeemed and verify it is accepted.
- Complete an order with a single-use coupon and then attempt to use the same code again (in a new session or as a different user) and verify it is now invalid.
- Cancel an order that used a single-use coupon and verify whether the coupon is reinstated for future use (per shop policy).
- Apply a multi-use coupon 10 times across different accounts and verify it continues to work up to its defined usage limit.
- Apply a multi-use coupon that has just hit its maximum usage count and verify it is rejected as if it were a single-use coupon.
- Verify that under high concurrency (two users applying the last available use of a single-use coupon simultaneously) only one transaction succeeds and the other receives an error.
- Verify that the admin backend correctly reflects the usage count and remaining uses for multi-use coupons.

### Coupon limited per customer

- Apply a coupon that allows one use per customer once, complete the order, then attempt to use the same code again with the same account and verify it is rejected.
- Apply the same per-customer coupon with two different customer accounts and verify both uses are accepted (the limit is per customer, not global).
- Apply a per-customer coupon as a guest, complete the order, then register with the same email address and attempt to use the coupon again; verify it is rejected (the system ties the guest order to the email).
- Apply a per-customer coupon that allows two uses per customer, use it once, and verify a second use is still accepted; verify a third use is rejected.
- Attempt to bypass the per-customer limit by using slightly different email addresses (e.g., test+1@example.com vs test@example.com) and verify whether the system detects this (fraud prevention).
- Verify the admin backend shows per-customer usage data, allowing staff to manually reset or override the limit if needed.

### Time-limited coupon (expiry date)

- Apply a coupon one day before its expiry date and verify it is accepted.
- Apply a coupon on its exact expiry date (boundary) and verify whether same-day expiry is inclusive or exclusive, and document the expected behavior.
- Apply a coupon one day after its expiry date and verify it is rejected with a clear "coupon expired" message showing the expiry date.
- Apply a coupon that has a start date in the future and verify it is rejected with a "not yet valid" message.
- Begin checkout with a valid coupon, leave the page open past midnight (when the coupon expires), then submit the order; verify the system re-validates the coupon at order placement and rejects it if expired.
- Verify that coupon expiry uses the server's timezone (not the client's) to prevent timezone abuse.
- Verify the coupon expiry date is displayed to the customer in the cart or coupon confirmation message.
- Attempt to apply a coupon whose validity window has not started yet (start date is tomorrow) and verify it is rejected.

### Coupon for specific products/categories

- Apply a category-restricted coupon to a cart containing only eligible products and verify the full discount is applied.
- Apply a category-restricted coupon to a cart containing only ineligible products and verify the coupon is rejected with a message explaining the restriction.
- Apply a category-restricted coupon to a mixed cart (some eligible, some not) and verify the discount is applied only to eligible items' subtotal.
- Apply a product-specific coupon where the coupon covers exactly one product and the cart has multiple quantities of that product; verify whether the discount applies once or to all units.
- Apply a coupon restricted to a product that is currently out of stock (but still in the cart as a pre-order) and verify the coupon behavior is documented and consistent.
- Apply a coupon for a specific brand/manufacturer to a cart containing items from that brand and other brands, and verify correct partial discount.
- After applying a category-restricted coupon, add a qualifying item to the cart and verify the discount updates to include the new item.
- After applying a category-restricted coupon, remove the last qualifying item from the cart and verify the coupon is invalidated and the customer is notified.
- Verify that the cart and order summary clearly indicate which items are discounted and which are not when a restricted coupon is active.

### Coupon stacking (multiple simultaneously)

- Apply two valid coupons simultaneously when stacking is disabled and verify the second coupon is rejected with a "coupons cannot be combined" message.
- Apply two valid coupons simultaneously when stacking is enabled and verify both discounts are applied correctly and the combined discount does not produce a negative total.
- Apply a percentage coupon and a fixed coupon together (stacking allowed) and verify the calculation order (e.g., percentage applied first, then fixed deducted from remaining total — or vice versa per policy).
- Apply two free-shipping coupons simultaneously (stacking enabled) and verify the result is still €0.00 shipping, not a negative shipping cost.
- Apply a coupon that is explicitly marked as non-combinable alongside a store-wide automatic discount and verify the correct priority rule is applied.
- Attempt to apply three coupons when the system allows a maximum of two; verify the third is rejected.
- Verify that the cart summary correctly lists each active coupon as a separate line item, showing the individual discount amount for each.
- Remove one stacked coupon and verify the other coupon's discount remains and the total is recalculated correctly.

### Automatic discounts (e.g., 10% off from 3 items)

- Add exactly 3 qualifying items to the cart and verify the automatic discount triggers.
- Add 2 qualifying items and verify the automatic discount does not trigger; verify a hint is shown ("Add 1 more item for 10% off").
- Add 3 qualifying items, trigger the automatic discount, then remove one item and verify the discount is automatically removed.
- Add 5 qualifying items and verify the automatic discount applies to all 5 (or only 3 per rule definition, per shop policy).
- Verify that automatic discounts and manual coupon codes interact correctly (both apply, or one takes precedence per policy).
- Add a mix of qualifying and non-qualifying items; verify the automatic discount only counts qualifying items toward the threshold.
- Verify the automatic discount is displayed in the cart without requiring any customer action, and is labeled clearly (not confused with a coupon code).
- Verify the automatic discount appears in the order confirmation email and on the invoice.
- Test the automatic discount at the exact boundary (add item 3, discount appears; remove item 3, discount disappears) multiple times to check for race conditions or UI glitches.

### Volume discounts / tiered pricing

- Add quantity 1 of a product with tiered pricing and verify the base price applies.
- Add quantity at the first tier threshold (e.g., 5 units) and verify the tier-1 price per unit applies to all units.
- Add quantity at the second tier threshold (e.g., 10 units) and verify the tier-2 price applies and the total is less than 10 × tier-1 price.
- Add a quantity that falls between tiers (e.g., 7 units where tiers are 5 and 10) and verify the correct tier applies (tier-1 for 5–9).
- Verify the tier pricing table is clearly displayed on the product page and updates dynamically as the quantity selector is changed.
- Add tiered-priced items to the cart, then change the quantity to drop below the current tier; verify the price per unit updates upward immediately.
- Mix the same product across multiple add-to-cart actions (e.g., add 3, then add 2 more) and verify that the combined quantity of 5 triggers the correct tier.
- Verify that tiered pricing interacts correctly with a percentage coupon (discount applies to the already-reduced tier price, not the original price).
- Verify that volume discount pricing is correctly reflected on the invoice and in the order confirmation email (not the original list price).
- Add a quantity that would trigger a tier, then use express checkout; verify the tier price is communicated correctly to the payment sheet.

### Sale prices / strikethrough prices

- Navigate to a product on sale and verify the original (strikethrough) price and the sale price are both displayed.
- Verify the strikethrough price shown is the genuine previous price (not an inflated reference price), in compliance with Omnibus Directive / price indication regulations.
- Verify the sale price is also reflected correctly in the cart, checkout, order summary, confirmation email, and invoice (no accidental reversion to full price).
- Verify the sale badge/label (e.g., "SALE", "-20%") is displayed correctly on category pages, product pages, and in the cart.
- Apply a coupon code on top of a sale price and verify the discount is calculated on the sale price (not the original price), unless configured otherwise.
- Set a sale price with an end date; verify the price reverts to the original price after the sale ends and the strikethrough display disappears.
- Verify that products where the "sale price" equals the original price do not show a misleading strikethrough.
- Verify that structured data (Product Schema) reflects the correct current price and, if applicable, the original price in the `priceValidUntil` and `highPrice`/`lowPrice` fields.
- Verify currency formatting of strikethrough prices is consistent with the rest of the shop (same decimal separator, currency symbol position).

### Bundle discounts

- Configure a bundle of product A + product B with a 15% bundle discount and add both to the cart; verify the discount is applied.
- Add only product A from a bundle and verify no bundle discount is applied and no misleading discount hint is shown.
- Add the bundle products, then remove product B; verify the bundle discount is removed and the price reverts for product A.
- Add multiple quantities of a bundle (e.g., 2× the complete bundle) and verify the discount applies to each complete bundle set.
- Verify the bundle discount is clearly labeled in the cart (e.g., "Bundle discount: -€X").
- Apply a coupon code on top of a bundle discount and verify whether stacking is allowed and the combined calculation is correct.
- Verify that bundle components cannot each individually receive a category-restricted coupon that was intended for standalone purchases only.
- Add a bundle where one component is out of stock; verify the bundle discount is not applied and the customer is informed.
- Verify that the bundle discount appears on the invoice and in the confirmation email.

### New customer discount

- Complete a first purchase as a new customer using the new customer discount; verify the discount is applied.
- Attempt to use the new customer discount on a second order with the same account; verify it is rejected.
- Attempt to use the new customer discount as a guest for a second order using the same email address; verify it is rejected.
- Attempt to create a new account with a slightly different email to abuse the new customer discount and verify whether the system has fraud-prevention measures (e.g., IP rate limiting, email domain checks).
- Verify the new customer discount is automatically applied at checkout or clearly communicated how to apply it (coupon code vs. automatic).
- Verify the new customer discount appears correctly in the order summary, confirmation email, and invoice.
- Apply an additional coupon code on top of the new customer discount (if stacking is disabled) and verify only one discount applies.
- Cancel an order that used the new customer discount; verify whether the "new customer" status is reinstated or permanently consumed.

### Loyalty points / rewards program

- Complete a purchase and verify the correct number of loyalty points is credited to the customer account (per the configured earn rate).
- Verify points are credited after the order status reaches "paid" or "shipped" (not immediately on order placement, if that is the policy).
- Redeem available loyalty points at checkout and verify the correct monetary value is deducted from the order total.
- Attempt to redeem more points than the customer has and verify the input is clamped or rejected with a clear message.
- Redeem loyalty points and verify the remaining points balance in the customer account is updated immediately after order placement.
- Cancel an order where loyalty points were earned and verify the points are deducted from the account balance.
- Cancel an order where loyalty points were redeemed and verify the points are reinstated.
- Verify loyalty points have an expiry date (if applicable) and that expired points are no longer shown as available.
- Verify that points earned during a promotional event (e.g., double points) are calculated correctly.
- Verify the points balance, earn history, and redemption history are all visible and accurate in the customer account dashboard.
- Apply loyalty points redemption alongside a coupon code and verify both discounts are reflected correctly and the total is not negative.
- Verify that guest customers cannot earn loyalty points, and a prompt is shown to log in or register to participate.

### Gift certificates (purchase + redeem)

- Purchase a gift certificate for a specific value (e.g., €50) and verify the payment is processed, the certificate code is generated and delivered via email.
- Verify the gift certificate email is received promptly, contains the correct code and value, and the code is machine-readable (no OCR ambiguity).
- Redeem a gift certificate code at checkout and verify the correct amount is deducted from the order total.
- Redeem a gift certificate where the certificate value exceeds the order total; verify the remainder is stored as residual balance and the overage is not paid out as cash.
- Redeem a gift certificate where the certificate value is less than the order total; verify the remaining balance must be paid by another payment method.
- Attempt to redeem an already fully-used gift certificate and verify it is rejected with a clear message showing the remaining balance (€0.00).
- Attempt to redeem a gift certificate with an invalid or non-existent code and verify a clear error message is displayed.
- Redeem a gift certificate that has an expiry date, after that date, and verify it is rejected with an expiry message.
- Purchase a gift certificate and attempt to apply it to an order in a different currency than it was purchased in; verify the system handles the conversion correctly or rejects the cross-currency use per policy.
- Verify that a gift certificate can be used alongside a coupon code (or is blocked, per policy), and the correct calculation is applied.
- Verify that purchasing a gift certificate does not itself earn loyalty points (or does, per policy — document the expected behavior).
- Cancel an order that was fully or partially paid with a gift certificate; verify the correct refund is issued (back to the certificate balance or to the original payment method per policy).
- Verify the gift certificate recipient can check the remaining balance (via a balance-check page or in their account if registered).
