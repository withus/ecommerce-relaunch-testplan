# 9. Express Checkout

### Apple Pay button on product page

- Verify Apple Pay button renders on the product page only when accessed from a Safari browser on a supported Apple device (macOS, iOS, iPadOS).
- Confirm Apple Pay button does NOT render on non-Apple browsers (Chrome, Firefox, Edge) or non-Apple devices.
- Verify the button is displayed in the correct position (above or below the standard "Add to Cart" button) and does not overlap other elements.
- Confirm the button uses the official Apple Pay branding (black or white button as per Apple's guidelines) — no custom styling that violates Apple Pay HIG.
- Select a product variant (e.g., size L, color blue) and verify the Apple Pay button reflects the correct variant and price in the payment sheet.
- On a product with required options not yet selected, click the Apple Pay button and confirm the user is prompted to complete required selections before the payment sheet opens.
- Verify the Apple Pay button is not shown when the product is out of stock.
- Check Apple Pay button visibility and tap target size on mobile (iOS Safari) — minimum 44×44px.
- Confirm the Apple Pay button is accessible via keyboard navigation and has an appropriate ARIA label for screen readers.
- Verify `PaymentRequest` / `ApplePaySession` is only initiated after the user taps the button (no premature session creation on page load).

### Google Pay button on product page

- Verify Google Pay button renders on the product page in Chrome and other Google Pay-supported browsers on Android and desktop.
- Confirm Google Pay button does NOT render when the user's device/browser combination does not support Google Pay.
- Verify the button uses the official Google Pay branding and complies with Google Pay's brand guidelines (color, size, rounded corners).
- Confirm the button is positioned consistently with the Apple Pay button and does not push other critical elements (price, variant selectors, Add to Cart) out of view.
- Select a product with multiple variants and verify the Google Pay payment sheet reflects the currently selected variant's price.
- Attempt to trigger Google Pay on a product with unselected required options and confirm an appropriate validation error is shown.
- Verify Google Pay is not displayed when the product is out of stock.
- Check that clicking the Google Pay button on desktop opens the Google Pay overlay without triggering a page reload or navigation.
- On Android Chrome, tap the Google Pay button and verify the native Google Pay sheet appears with the correct merchant name, item description, and total.
- Verify that a `google.payments.api.PaymentsClient` `isReadyToPay` check is performed before rendering the button (no button shown if Google Pay is not available for the user).

### PayPal Express button on product page

- Verify the PayPal Express button renders on the product page for all browsers and devices (it should have broader availability than Apple Pay / Google Pay).
- Confirm the button uses the official PayPal SDK-rendered button (not a manually styled HTML button) and displays the correct color/shape variant.
- Click the PayPal Express button on a simple product and confirm the PayPal popup/lightbox opens with the correct item name, quantity, and price pre-filled.
- Select a product variant before clicking PayPal Express and verify the popup reflects the selected variant's price.
- Click PayPal Express on a product with unselected required options and confirm validation fires before the popup opens.
- Verify PayPal Express is not shown (or is disabled) when the product is out of stock.
- Test PayPal Express with a PayPal sandbox account: complete the payment flow end-to-end and verify the order is created in the backend.
- Verify that dismissing the PayPal popup (closing the window) returns the user to the product page without any error state or broken UI.
- Confirm the PayPal button respects the page's content security policy (CSP) — no console errors related to blocked PayPal SDK scripts.
- Check that the PayPal button renders correctly on mobile (proper size, no overflow) and is tappable without zooming.

### Express checkout in cart

- Verify Apple Pay, Google Pay, and PayPal Express buttons are all displayed in the cart (where applicable per device/browser).
- Confirm the buttons appear above the "Proceed to Checkout" button and are clearly labeled as express options (e.g., a "— OR —" separator).
- Add multiple products to the cart and click any express checkout button — verify the payment sheet lists all cart items with correct individual prices and a correct total.
- Change the quantity of a cart item and verify the express checkout total updates before the payment sheet is opened.
- Remove a product from the cart so the cart becomes empty and verify all express checkout buttons disappear or are disabled.
- Apply a coupon code in the cart and open the express checkout — verify the discount is reflected in the payment sheet total.
- Verify the express checkout buttons in the cart are shown after a page reload (cart is persistent).
- On mobile, confirm all three express buttons (where available) are visible without horizontal scrolling and each has a sufficient tap target.
- Test that clicking an express checkout button from the cart does not add a duplicate entry to the order (the cart contents should be used as-is, not re-added).
- Verify that the cart mini-drawer/sidebar also shows express checkout options if the full cart page does.

### Express checkout on checkout page

- Navigate to the standard multi-step checkout page and verify express checkout buttons are displayed at the top (before address entry steps).
- Confirm the express buttons on the checkout page show the same cart contents and totals as the cart page.
- If the user has already partially filled in address fields, verify that initiating express checkout does not discard or conflict with the partially entered data.
- Click an express checkout button on the checkout page and complete the payment — verify the user is not forced back through the standard checkout steps.
- Verify that the express checkout buttons are not shown on the final "order review/confirmation" step of the standard checkout (to avoid duplicate submissions).
- Test that after successfully completing express checkout from the checkout page, the user lands on the order confirmation page with the correct order number.
- Confirm the express buttons are displayed for guest users (not logged in) on the checkout page.
- Verify the express buttons are also displayed for logged-in users and that their stored addresses do not interfere with the address provided via the wallet.
- On a checkout page with a session that has timed out, verify the express checkout buttons do not appear or clicking them shows a meaningful "session expired" message rather than a silent failure.

### Address transfer from wallet

- Complete Apple Pay express checkout and verify that the shipping address returned from the Apple Pay sheet is correctly populated in the order record (name, street, city, postal code, country).
- Complete Google Pay express checkout and verify the shipping address from the Google Pay sheet is stored correctly in the order.
- Complete PayPal Express checkout and verify the PayPal-provided shipping address is used for the order, including any address2/apartment field.
- Use an Apple Pay wallet address with non-ASCII characters (e.g., German umlauts: Müller, Straße) and verify the address is stored without character corruption.
- Use a wallet address with a very long street name (50+ characters) and confirm it is saved completely without truncation.
- Use a wallet address from a non-German country (e.g., Austria, Switzerland) and verify the country code is correctly mapped and stored.
- Verify the billing address is also transferred from the wallet (or that the shipping address is used as billing address when no separate billing address is provided).
- Confirm no personal wallet data is logged in plain text in server logs or error trackers (Sentry, etc.).
- Test with a wallet that has an incomplete address (e.g., missing phone number) and verify the system either prompts the user for the missing data or handles the missing field gracefully.
- Verify the transferred address appears on the order confirmation page and in the confirmation email without any display artefacts.

### Shipping cost calculation in express flow

- After providing a delivery address in the Apple Pay sheet, verify that available shipping methods and their costs are shown in the payment sheet before the user confirms payment.
- Change the delivery address within the Apple Pay sheet to a different country and verify the shipping options and costs update accordingly.
- Verify that free shipping (when applicable above a cart threshold) is correctly reflected in the shipping options shown in the express payment sheet.
- Select express shipping in the Apple Pay sheet and verify the updated total (base price + express shipping surcharge) is displayed before confirmation.
- Use a delivery address for a special destination (e.g., German island, remote area) and verify the applicable surcharge appears in the shipping options.
- Test with a Google Pay sheet: change the delivery address and verify shipping options dynamically update via the `onShippingAddressChange` callback without closing the sheet.
- Test with PayPal Express: verify shipping options update when the user selects a different address in the PayPal popup.
- Verify that if no shipping method is available for the selected address (e.g., a country not served), the user sees a meaningful error in the payment sheet and cannot proceed.
- Confirm that the shipping cost shown in the express payment sheet exactly matches what is later shown on the order confirmation page — no discrepancy.
- Verify that digital-only orders (no physical shipping) either show a "no shipping required" option or skip the shipping selection step in the express flow.

### Cancel express checkout → return to normal flow

- Click the Apple Pay button, then dismiss the Apple Pay payment sheet (press Cancel / close the sheet) — verify the user is returned to the exact same page (product page or cart) with no error messages and all previously selected options intact.
- Click the Google Pay button, then close the Google Pay overlay — verify the page state is unchanged (cart quantities, variant selections, coupon codes all preserved).
- Click the PayPal Express button, then close the PayPal popup — verify the user is back on the product/cart page and no partial order has been created in the backend.
- Verify that cancelling express checkout does NOT decrement any inventory reservation or create a pending order record.
- After cancelling express checkout, verify the user can still proceed via the standard "Add to Cart" → "Proceed to Checkout" flow without issues.
- After cancelling express checkout, verify the user can immediately attempt express checkout again without errors (no stuck state).
- Verify that multiple consecutive cancel + retry cycles (e.g., 5 times in a row) do not degrade performance or produce console errors.
- On mobile, dismiss the express checkout sheet using the native gesture (swipe down on iOS) and verify the same clean return to the page.
- Verify that a payment sheet timeout (user leaves it open for several minutes, then the session expires) shows a user-friendly message rather than a raw error when the user returns to the page.
- Confirm that cancelling at the address-selection step (before payment authorisation) does not trigger any payment intent creation on the payment provider side.

### Payment intent creation

- Click an express checkout button (any provider) and verify a Payment Intent is created on the backend via the payment provider's API (check server logs or Stripe Dashboard in test mode).
- Verify the Payment Intent is created with the correct amount (in the smallest currency unit, e.g., cents) matching the cart total including shipping.
- Verify the Payment Intent currency matches the shop's configured currency.
- Confirm the Payment Intent is created only after the user initiates the express checkout (button click) — not on product page load.
- Verify the Payment Intent metadata includes identifiable order reference information (e.g., cart ID, session ID) to allow reconciliation.
- For Stripe: verify the Payment Intent is created with `capture_method: automatic` (or `manual`, depending on shop config) as intended.
- Verify that if the Payment Intent creation API call fails (simulate 500 from Stripe/PayPal), the user sees a user-friendly error message in the payment sheet and is not left in a broken state.
- Confirm that a Payment Intent created during express checkout is not a duplicate of any intent created during a prior standard checkout attempt for the same cart.
- Verify the Payment Intent ID is stored in the order record after the payment is confirmed, enabling post-payment lookups and refunds.
- Check that Payment Intent creation does not reveal sensitive data (API keys, raw error messages from the payment provider) in the browser's network tab responses.

### Payment intent update on changes

- Initiate express checkout (payment sheet open), then in a separate browser tab change the cart quantity — verify the Payment Intent amount is updated to match the new total when the next network call is made.
- In the express checkout payment sheet, change the delivery address to trigger a different shipping cost — verify an `updateWith` call (or equivalent) updates the Payment Intent amount before the user confirms.
- In the express checkout payment sheet, switch the selected shipping method — verify the Payment Intent amount is updated to reflect the new shipping cost before payment confirmation.
- Verify that updating the Payment Intent does not create a new Payment Intent — it must reuse the existing ID via an update API call.
- Simulate a scenario where the cart price changes server-side between Payment Intent creation and user confirmation (e.g., a flash sale activates) — verify the payment fails or re-validates rather than charging the old amount.
- Verify that updating the Payment Intent in the express sheet does not cause the sheet to flicker, close, or reset the user's entered address.
- After a Payment Intent update, confirm the final charged amount in the payment provider dashboard matches the total shown on the order confirmation page.
- Verify that a failed Payment Intent update (e.g., the backend returns an error when recalculating) shows a user-friendly error in the payment sheet and offers the user a way to proceed via standard checkout.
- Test rapid consecutive changes (e.g., switching shipping methods quickly 3–4 times) and verify only the final updated amount is used, with no race condition charging a stale amount.
- Verify Payment Intent updates are idempotent: re-submitting the same update (same address, same shipping) does not create duplicate API calls or change the intent state unexpectedly.
