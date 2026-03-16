# Shopping Cart

### Add product (from product page)

- Navigate to a product page, select a valid variant (size, color, etc.), click "Add to Cart" — verify the mini cart counter increments by 1 and a success notification appears.
- Add a product without selecting a required variant — verify that the form blocks submission and displays a clear validation error indicating which option is missing.
- Add the same product twice using the "Add to Cart" button — verify the cart shows quantity 2 for that item (not two separate line items), unless the shop intentionally creates separate lines.
- Add a product with quantity set to 3 in the quantity selector — verify that the cart reflects exactly 3 units and the line item total equals unit price × 3.
- Add a product that is marked "only X left in stock" — verify it can be added up to the available stock quantity but not beyond.
- Add a product that is displayed as "out of stock" — verify the "Add to Cart" button is disabled or absent and cannot be bypassed via direct URL manipulation or console commands.
- Add a product, then immediately check the cart page — verify the correct product name, selected variant details, unit price, quantity, and line total are all displayed accurately.
- Add a product while not logged in — verify the item appears in the cart and persists across page navigations within the same session.
- Click "Add to Cart" rapidly multiple times in quick succession — verify the cart does not add duplicate entries beyond the intended quantity (debounce/throttle check).
- Add a configurable product with a price surcharge option — verify the cart displays the adjusted price, not the base price.

### Add product (from category page)

- On a category listing, click the quick-add "Add to Cart" button on a simple product (no variants) — verify the item is added and the cart counter updates without leaving the page.
- On a category listing, click "Add to Cart" on a product that requires variant selection — verify the user is either redirected to the product page to choose options or a modal/inline selector appears before adding.
- Add a product from a category page, then navigate to the full cart page — verify the line item data (name, SKU, price, quantity) matches the product shown in the category listing.
- Add multiple different products from the category page in sequence — verify all items accumulate correctly in the cart.
- On mobile, tap the "Add to Cart" button on a category page — verify the touch target works reliably and the cart state updates visibly.
- Add a product from a category page while already having items in the cart — verify existing cart contents are preserved and only the new item is appended.

### Cart display (mini cart / sidebar)

- Click the cart icon in the header after adding items — verify a mini cart or sidebar slides in/opens showing each line item with name, variant, quantity, unit price, and line total.
- Verify the mini cart displays a correct running subtotal that matches the sum of all line item totals.
- Add a product with a long name — verify the mini cart truncates or wraps the text gracefully without breaking the layout.
- Add 10+ different items — verify the mini cart remains usable (scrollable if needed) and does not obscure the page behind it uncontrollably.
- Verify the mini cart closes when clicking outside of it, pressing Escape, or clicking an explicit close button.
- Verify the mini cart includes a visible link or button to proceed to the full cart page and another to go directly to checkout.
- Verify the cart icon in the header shows the correct item count badge at all times, updating immediately after add/remove actions.
- Open the mini cart on a mobile viewport — verify it renders correctly, buttons are tappable, and it does not overflow the screen.

### Full cart page

- Navigate to the full cart page — verify each line item shows: product image, product name, variant details, SKU, unit price, quantity selector, line total, and a remove button.
- Verify the cart page displays a subtotal, shipping cost estimate (or "calculated at checkout"), VAT information, and a grand total.
- Verify a "Continue Shopping" link/button is present and returns the user to the previous page or category.
- Verify a "Proceed to Checkout" button is prominent and navigates to the first checkout step.
- Reload the full cart page — verify all items and quantities are still present (no data loss on page reload).
- Open the cart page in a new browser tab — verify the same cart contents are shown (session consistency).
- View the cart page on mobile — verify it is responsive, the layout does not break, and all action buttons are accessible without horizontal scrolling.
- Verify that product images in the cart are the correct images matching the selected variant (e.g., correct color image).

### Change quantity

- On the cart page, increase the quantity of a line item using the "+" button or by typing a new value — verify the line total and cart subtotal update immediately or after confirmation.
- Decrease the quantity of a line item to 1 using the "−" button — verify the "−" button becomes disabled or the quantity cannot go below 1 via this control (removal should use the dedicated remove button).
- Type a quantity of 0 directly into the quantity input field — verify the item is either removed with a confirmation prompt or the field reverts to 1 (no broken zero-quantity state).
- Enter a non-numeric value (e.g., "abc") in the quantity field — verify the input is rejected or reverted to the previous valid value.
- Change the quantity to exceed the available stock — verify an error message is shown and the quantity is capped at the maximum available stock.
- Change the quantity of one item and verify only that line item's total changes; all other line items remain unchanged.
- Change quantity and verify the updated subtotal, VAT amount, and grand total all recalculate correctly and consistently.

### Set quantity to 0 / remove product

- Type 0 into a quantity field and confirm/submit — verify the item is removed from the cart and the cart totals update accordingly.
- Set quantity to 0 on the only item in the cart — verify the cart transitions to the empty state with an appropriate message.
- Verify that after setting quantity to 0, a success or informational message briefly appears confirming the item was removed.
- After removing an item by setting quantity to 0, verify it does not reappear on page refresh.

### Remove individual product

- Click the remove/delete icon or button on a single line item — verify only that item is removed, all other items remain in the cart, and totals are recalculated.
- Verify a confirmation dialog or undo option appears before or immediately after removal, allowing the user to recover from accidental deletion.
- Remove a product that had an applied coupon discount — verify the coupon discount is recalculated or an appropriate message is shown if the minimum order value is no longer met.
- Remove a product from the mini cart — verify the change is immediately reflected on the full cart page as well.
- Remove all items one by one individually — verify the cart reaches the empty state correctly.

### Clear cart

- Click a "Clear Cart" or "Empty Cart" button — verify all line items are removed in a single action and the empty cart state is displayed.
- Verify a confirmation dialog is shown before clearing the cart to prevent accidental data loss.
- After clearing the cart, verify the cart counter in the header resets to 0.
- Clear the cart and then use the browser back button — verify the cart does not restore the removed items (no phantom cart restoration).
- Clear the cart as a logged-in user — verify the cleared state persists across sessions (the cart is empty on next login).

### Price calculation correct (unit price × quantity)

- Add a single item with quantity 1 — verify line total equals the unit price exactly.
- Add a single item with quantity 5 — verify line total equals unit price × 5 with correct decimal/currency formatting.
- Add multiple different items — verify each line total is independently correct, and the subtotal equals the sum of all line totals.
- Add a product with a price that has cents (e.g., €12.99) in quantity 3 — verify the line total is €38.97 (correct floating-point arithmetic, no rounding errors).
- Add a product whose price changes due to a volume discount at quantity threshold — verify the line total reflects the discounted unit price, not the original price.

### Subtotal correct

- Add three different products with various quantities — verify the displayed subtotal equals the arithmetically correct sum of all line item totals.
- Apply a coupon code — verify the subtotal shown is the pre-discount amount, and the discount is shown as a separate line item deduction.
- Remove one item from a multi-item cart — verify the subtotal decreases by exactly that item's line total.
- Change the quantity of an item — verify the subtotal updates in real time or after a defined update action.
- Verify the subtotal is consistent across the mini cart, full cart page, and order summary section.

### VAT calculation and display

- Add a product to the cart — verify VAT is displayed either as "incl. X% VAT" or as a separate line item depending on the shop's configuration, and the amount is mathematically correct.
- Verify that for a German shop (19% standard VAT): a net price of €100 shows gross €119.00 and VAT €19.00.
- Add products with different VAT rates (e.g., standard 19% and reduced 7%) — verify each rate is shown separately in the VAT breakdown.
- Change the delivery country (if country switcher is present) — verify the VAT rate updates accordingly (e.g., Austria 20%).
- Verify that VAT amounts are consistently rounded correctly (standard rounding, not always rounding up or down).

### Shipping cost preview

- View the cart with a subtotal below the free shipping threshold — verify an estimated shipping cost is displayed along with information about how much more is needed for free shipping.
- View the cart with a subtotal at or above the free shipping threshold — verify the shipping cost is shown as €0.00 or "Free".
- Verify the "You are €X away from free shipping" progress bar or message updates dynamically when item quantities change.
- Add a product that is a bulky/oversized item — verify any surcharge is reflected in the shipping cost preview.
- Verify the shipping cost preview matches the actual shipping cost displayed later in checkout (no surprise fee increases).
- If no address has been entered yet, verify a note is shown that the final shipping cost will be confirmed at checkout.

### Enter coupon code

- Enter a valid, active coupon code into the coupon field and click "Apply" — verify the discount is applied, shown as a line item, and the grand total is reduced correctly.
- Enter a valid coupon code with leading/trailing whitespace — verify the code is trimmed and accepted (no rejection due to invisible characters).
- Enter a coupon code in a different letter case (e.g., uppercase vs. lowercase) — verify whether the system handles case-insensitively (document the behavior).
- Verify the coupon code field and apply button are accessible via keyboard (Tab + Enter) without requiring mouse interaction.
- After applying a coupon, verify the coupon code and the discount amount are both clearly displayed in the cart summary.

### Remove coupon code

- After applying a coupon, click the "Remove" or "×" button next to the coupon — verify the discount is reversed, the coupon line disappears, and the grand total returns to the original amount.
- Remove a coupon and verify the subtotal, VAT, and grand total all recalculate correctly.
- After removing a coupon, verify the coupon input field is available again to enter a different code.
- Remove a free-shipping coupon — verify that shipping costs reappear correctly in the cart summary.

### Multiple coupons (if allowed)

- If the shop allows stacking, apply two valid coupon codes sequentially — verify both discounts are applied and shown as separate line items.
- Apply a percentage coupon followed by a fixed-amount coupon — verify the order of application matches the shop's documented stacking rules and the final total is correct.
- If stacking is NOT allowed, attempt to enter a second coupon code — verify a clear error message states that only one coupon can be used per order, and the first coupon remains applied.
- Verify that attempting to use the same coupon code twice is blocked with a relevant error message.

### Coupon validation (expired, minimum order value, already used)

- Enter an expired coupon code — verify a clear error message states the coupon has expired and the cart total is not changed.
- Enter a coupon with a minimum order value of €50 on a cart totaling €30 — verify an error message states the minimum order value is not met, including the required amount.
- Enter a single-use coupon that has already been redeemed by the same account — verify a clear error message states the coupon has already been used.
- Enter a single-use coupon that has already been redeemed by a different account — verify it is rejected as already used.
- Enter a completely invalid/nonexistent coupon code — verify a generic "coupon not found" or "invalid code" error message (not a technical error or stack trace).
- Enter a coupon restricted to a specific product category when the cart only contains items from a different category — verify a clear error message explaining the restriction.
- Apply a valid coupon, then remove items from the cart causing the order total to drop below the minimum — verify the coupon is automatically removed or flagged with a warning.

### Cross-selling in cart

- View the cart page — verify cross-sell product recommendations are displayed (e.g., "Customers also bought", "You might also like").
- Click on a recommended cross-sell product — verify the link leads to the correct product page.
- Add a cross-sell product from the cart page — verify it is added to the cart correctly and the cross-sell section updates (the just-added product is removed from recommendations).
- Verify cross-sell recommendations are relevant to the items in the cart (not random unrelated products).
- On mobile, verify cross-sell widgets in the cart are displayed correctly and do not break the layout.

### Cart persistence (after closing browser)

- Add items to the cart as a logged-in user, close the browser completely, reopen it and navigate back to the shop — verify all cart items, quantities, and variants are still present.
- Add items to the cart as a guest user, close the browser completely, reopen it and navigate back to the shop — verify the cart is restored (cookie-based persistence).
- Add items to the cart, let the session expire (wait beyond session timeout), then revisit the shop — verify the expected behavior: logged-in users should recover the cart; guests may see an empty cart or a partial restoration depending on cookie expiry.
- Verify cart persistence across multiple page navigations within the same session (product page → category page → homepage → back to cart).

### Cart merge (guest → login)

- Add items to the cart as a guest, then log in to an existing account that has a saved cart — verify both the guest cart items and the pre-existing account cart items are merged into a single cart.
- Add items to the cart as a guest, then log in to an account with no existing cart — verify the guest items are transferred to the account cart without duplication.
- Add the same product as a guest that already exists in the logged-in account's saved cart — verify the quantities are combined (not duplicated as two separate line items), or that the shop clearly displays the conflict and resolves it.
- After cart merge, verify the cart page displays the merged cart correctly with accurate totals.
- Add items as a guest, register a new account during checkout — verify the guest cart items are retained and associated with the new account.

### Empty cart display

- Navigate to the cart page with no items in the cart — verify a dedicated empty cart message is shown (e.g., "Your cart is empty") and not a broken/blank page.
- Verify the empty cart page includes a clear call to action (e.g., "Continue Shopping" link, featured products, or search field) to guide the user back to browsing.
- Verify the cart counter in the header shows 0 (or is hidden) when the cart is empty.
- Verify the "Proceed to Checkout" button is absent or disabled when the cart is empty to prevent empty checkout attempts.

### Continue shopping button

- Click the "Continue Shopping" button from the cart page — verify the user is returned to the last visited category or product page, preserving their browsing context.
- Click "Continue Shopping" from an empty cart — verify the user is sent to the homepage or a default category page.
- Verify the "Continue Shopping" button/link is clearly labeled and visually distinct from the "Proceed to Checkout" action.
- On mobile, verify the "Continue Shopping" link is accessible and does not require scrolling past a lengthy cart to find.

### Proceed to checkout button

- Click the "Proceed to Checkout" button with items in the cart — verify the user is directed to the first checkout step (address entry or login prompt for guests).
- Click the "Proceed to Checkout" button without being logged in — verify the user is prompted to log in, register, or continue as a guest, with the cart contents preserved throughout.
- Verify the "Proceed to Checkout" button is prominently placed (visible without scrolling on desktop; sticky or clearly accessible on mobile).
- Verify the button state: it should be active when the cart has valid items and inactive/absent when the cart is empty.
- Click the "Proceed to Checkout" button twice rapidly — verify there is no duplicate checkout session or double-redirect (debounce behavior).

### Weight / dimensions display

- Add a product with defined weight and/or dimensions — verify this information is displayed in the cart if the shop is configured to show it.
- Add multiple products — verify the combined total weight displayed in the cart is the sum of (unit weight × quantity) for all line items.
- Verify displayed weight/dimensions match the values on the product page for each item.
- If weight affects shipping costs, verify changing quantity updates the displayed weight and triggers a shipping cost recalculation.

### Delivery time display per item

- Add a product to the cart that has a defined delivery time (e.g., "In stock — delivery in 2–4 business days") — verify this delivery time is shown per line item in the cart.
- Add multiple products with different delivery times — verify each item's delivery time is shown individually, and if an overall estimated delivery date is shown, it reflects the longest lead time.
- Add a product with a special order or made-to-order delivery time — verify the extended lead time is clearly communicated in the cart.
- Verify delivery time information in the cart is consistent with the delivery time shown on the product page.

### Mark unavailable items in cart

- Add a product to the cart, then simulate the product going out of stock (or test with a known discontinued item) — verify the cart displays the item as unavailable with a visual indicator (e.g., red warning, "Unavailable" label).
- Verify the checkout button is blocked or a warning is shown when the cart contains unavailable items, preventing the user from placing an order for items that cannot be fulfilled.
- Verify a clear message or call-to-action is shown for unavailable items (e.g., "Remove this item to continue" or "Notify me when back in stock").
- Remove the unavailable item from the cart — verify the warning clears and checkout can proceed normally.

### Maximum order quantity per product

- Add a product that has a defined maximum order quantity (e.g., max 5) — verify the quantity selector does not allow selecting more than the maximum.
- Attempt to set the quantity field manually to a number exceeding the maximum — verify it is capped or rejected with an informative error message stating the maximum allowed quantity.
- Add the product to the cart from different pages/sessions and verify the total accumulated quantity across all interactions does not exceed the per-order maximum.
- Verify the maximum quantity restriction is communicated clearly to the user near the quantity selector or as an error message when the limit is hit.

### Minimum order value

- Attempt to proceed to checkout with a cart total below the shop's minimum order value — verify a clear warning message is shown stating the minimum order value and how much more is needed.
- Verify the "Proceed to Checkout" button is disabled or redirects back to the cart with a warning when the minimum order value is not met.
- Add items to reach exactly the minimum order value — verify checkout can proceed.
- Verify the minimum order value check applies to the actual payable amount (i.e., after discounts if applicable, based on the shop's rule documentation).
- If a coupon brings the total below the minimum order value, verify the system detects this and prevents checkout with an appropriate message.
