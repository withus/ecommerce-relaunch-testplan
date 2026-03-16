# 8. Checkout

## 8.1 Address Entry

### Billing address form (all fields)
- Verify that all required fields are present: first name, last name, street, house number, postal code, city, country.
- Submit the form leaving each required field empty one at a time — verify a field-specific validation error appears for each.
- Enter a valid billing address with all fields populated and verify the form submits without errors.
- Enter only whitespace in required fields and verify these are rejected as empty.
- Enter values that exceed maximum character limits (e.g., 300-character street name) and verify the input is either truncated or rejected with an error message.
- Verify that all field labels are visible, correctly worded, and associated with their respective input elements.
- Verify that tab order moves logically through the form fields (first name → last name → street → postal code → city → country).
- Enter valid data and navigate away, then return to checkout — verify form data is preserved.
- Verify that the form displays correctly on mobile devices with no fields hidden or overflowing.
- Submit the form with HTML/script tags in a text field (e.g., `<script>alert(1)</script>`) — verify input is sanitised and not executed.

### Different shipping address
- Verify that a checkbox or toggle exists to enable a separate shipping address.
- By default, verify the shipping address section is hidden or pre-filled with the billing address.
- Enable the "different shipping address" option and verify a separate shipping address form appears.
- Fill in a shipping address that differs from billing and complete the order — verify the confirmation email and order details show the correct shipping address.
- Enable the different shipping address, fill it in, then uncheck the option — verify the shipping address section collapses and the billing address is used for delivery.
- Verify all required fields in the shipping address form have validation equal to the billing address form.
- Verify the shipping cost is calculated based on the shipping address country, not the billing address country.
- Enter a shipping address in a different country than the billing address and verify that applicable international shipping options appear.

### Address validation (postal code format, required fields)
- Enter a postal code in an invalid format for the selected country (e.g., letters for a German PLZ) — verify an error message is displayed.
- Change the country selection and re-enter a postal code — verify that the validation rule updates to match the new country's format.
- Submit the form with the postal code field empty — verify a "required field" error is shown.
- Enter a valid 5-digit German postal code (e.g., 10115) — verify it is accepted without error.
- Enter a postal code with extra spaces (e.g., " 10115 ") — verify it is either trimmed and accepted or flagged for correction.
- Verify that the city field does not auto-clear when a postal code is changed.
- Enter a postal code that does not exist (e.g., 00000 for Germany) — verify behaviour (either accepted or warned, depending on configured validation level).

### Address autocomplete
- Begin typing a street address in the address field and verify autocomplete suggestions appear after 2-3 characters.
- Select an autocomplete suggestion and verify that street, postal code, and city fields are filled automatically.
- Verify that manually overriding an autocomplete-filled field is possible without the form resetting.
- Disable network connection and verify the form is still usable even if autocomplete is unavailable (graceful degradation).
- Verify that autocomplete suggestions are relevant to the selected country.
- Verify that selecting an autocomplete suggestion does not submit the form prematurely.
- Enter a full valid address manually without using autocomplete and verify the form accepts it normally.

### Business address (company name, VAT ID)
- Verify that a "business customer" checkbox or field group exists to toggle company-specific fields.
- Enter a company name and verify it appears on the order confirmation and invoice.
- Enter a valid EU VAT ID (e.g., DE123456789) and verify it is accepted.
- Enter an invalid VAT ID format (e.g., "123ABC") — verify an error message is displayed.
- If VAT validation is enabled, enter a real but non-matching VAT ID — verify the system responds correctly (accepted or flagged).
- Complete a B2B order with VAT ID and verify the invoice shows the VAT ID and applies reverse charge if applicable.
- Verify that the company name field is optional for private customers (no error when left empty in B2C mode).
- Verify that the VAT ID field is not shown when the business address toggle is inactive.

### Parcel locker / post office
- Verify that a parcel locker or post office delivery option is available in the shipping address section.
- Select "parcel locker" and verify the correct address fields are shown (e.g., locker ID, postal code).
- Enter a valid parcel locker ID and verify the order is accepted.
- Attempt to ship a product flagged as non-locker-compatible (e.g., oversized item) to a parcel locker — verify an appropriate error or warning is shown.
- Verify that the billing address can still be a regular home address when the shipping address is a parcel locker.
- Verify that parcel locker addresses appear correctly on the order confirmation and shipping label.
- Select a post office address and verify the form adapts to request the correct fields (e.g., post office number, name).

### Country selection
- Open the country dropdown and verify it contains all expected countries for the shop's target markets.
- Select a country outside the delivery zone — verify an error message or that the checkout blocks progression.
- Change the country selection after entering a postal code — verify the postal code field clears or re-validates.
- Verify that the default selected country matches the shop's primary market.
- Select a non-EU country and verify that VAT is removed or adjusted in the order summary.
- Verify that the country selection persists if the user navigates back from the next checkout step.
- Verify the country dropdown is accessible by keyboard and screen readers (correct ARIA labels).

### Saved addresses (logged-in customer)
- Log in with an account that has saved addresses — verify existing addresses are displayed for selection in the billing and shipping address steps.
- Select a saved address and verify all fields are pre-filled correctly.
- Select a saved address, modify one field, and complete checkout — verify the modified address is used for the order but the original saved address is unchanged.
- Verify that the customer can add a new address during checkout without losing cart contents.
- Delete a saved address from the account area and return to checkout — verify the deleted address no longer appears.
- Verify that the default address is pre-selected when the checkout address step loads.
- Log in mid-checkout (guest converting to account) — verify saved addresses are offered after login.

### Guest checkout (without account)
- Verify that a "Continue as guest" option is clearly displayed on the checkout login/registration step.
- Complete a full checkout as a guest (no account) and verify the order is placed successfully.
- Verify that a guest checkout order confirmation email is sent with a working order status link.
- Verify that a guest cannot access the order details page through the customer account area.
- Verify that after a guest order, the customer is offered the option to create an account (post-order registration).
- Attempt to proceed to checkout with an empty cart as a guest — verify the user is redirected to the cart page.
- Verify that guest checkout does not create a duplicate account if the email address already exists; instead show a message prompting login.

---

## 8.2 Shipping Options

### Shipping methods display (standard, express, pickup)
- Verify that all available shipping methods are listed with name, description, delivery time estimate, and cost.
- Verify that at least one shipping method is pre-selected by default.
- Select each available shipping method and verify the order summary updates the shipping cost accordingly.
- Verify that shipping methods not applicable to the customer's address (e.g., pickup for customers in another city) are hidden or disabled.
- Verify that shipping method labels are accurate (e.g., "Standard Delivery 3-5 days" does not show under Express).
- Verify that changing the shipping method does not clear address fields already entered.
- Verify that the selected shipping method is shown in the order summary on the final confirmation step.

### Shipping costs correctly calculated
- Add a single item to the cart and proceed to checkout — verify the displayed shipping cost matches the configured rate for the selected method.
- Add multiple items and verify that the shipping cost is calculated correctly (not multiplied per item unless configured that way).
- Change the shipping address to a different country and verify the shipping cost updates to reflect international rates.
- Select express shipping and verify the correct express surcharge is applied.
- Verify that the shipping cost shown in the cart matches the shipping cost on the checkout page.
- Complete an order and verify the shipping cost on the order confirmation matches what was displayed during checkout.
- Verify that VAT on shipping costs (if applicable) is correctly calculated and displayed.

### Free shipping above amount X
- Add items totalling just below the free shipping threshold — verify standard shipping costs are applied.
- Add items totalling exactly the free shipping threshold — verify free shipping is applied.
- Add items totalling above the free shipping threshold — verify free shipping is applied.
- Apply a coupon that reduces the order total below the free shipping threshold — verify standard shipping is reinstated.
- Remove an item from the cart so the total drops below the free shipping threshold — verify shipping cost reappears.
- Verify that the free shipping threshold is clearly communicated to the customer (e.g., "Add €X more for free shipping" banner).
- Verify free shipping applies only to eligible products if the shop has product-exclusion rules.

### Delivery time display per shipping method
- Verify each shipping method displays an estimated delivery time (e.g., "2-4 business days").
- Verify that the estimated delivery date accounts for weekends and public holidays if applicable.
- Select express shipping and verify a faster estimated delivery time is shown compared to standard shipping.
- Verify that delivery times update if the shipping address country changes.
- Place an order and verify the delivery time shown in checkout matches the time shown in the confirmation email.
- Verify that delivery time is shown per item if items have different lead times (e.g., made-to-order vs. in-stock).

### Preferred delivery date (if available)
- Verify a date picker or input field for preferred delivery date is present if this feature is enabled.
- Select a date in the past — verify it is rejected with an error message.
- Select a date that is within the standard delivery window — verify it is accepted.
- Select a date that is beyond the maximum configurable delivery window — verify it is rejected or flagged.
- Leave the preferred delivery date blank — verify the order proceeds without it (field is optional).
- Verify the preferred delivery date is included in the order confirmation email and visible in the admin backend.
- Select a weekend or public holiday as a preferred delivery date — verify the system warns or disables those dates.

### Bulky goods surcharge
- Add a product flagged as bulky goods to the cart and verify a surcharge is added to the shipping cost.
- Add a mix of regular and bulky products — verify only the bulky goods surcharge applies (not doubled).
- Verify the bulky goods surcharge is itemised separately in the order summary or clearly labelled.
- Remove the bulky item from the cart — verify the surcharge disappears from the shipping cost.
- Select a shipping address in a region with a different bulky goods rate — verify the correct regional rate is applied.
- Verify the bulky goods surcharge appears on the invoice.
- Verify that the bulky goods surcharge is not waived even when the order qualifies for free shipping.

### Island surcharge
- Enter a shipping address for a German island (e.g., Sylt, Rügen, Helgoland) — verify an island surcharge is added.
- Verify the island surcharge amount matches the configured rate.
- Enter a mainland address — verify no island surcharge is applied.
- Verify the island surcharge is clearly labelled in the order summary (not lumped in with standard shipping).
- Complete the order and verify the island surcharge is reflected on the invoice.
- Verify that the island surcharge is applied for all shipping methods, not only standard shipping.

### International shipping costs
- Select a shipping address in an EU country (e.g., Austria, Netherlands) — verify the correct EU shipping rate is displayed.
- Select a shipping address outside the EU (e.g., Switzerland, UK) — verify the correct non-EU rate is displayed.
- Verify that international shipping costs are listed in the same currency as the rest of the checkout.
- Add a bulky item and select an international address — verify the bulky surcharge and international shipping rate stack correctly.
- Verify that delivery time estimates update for international addresses.
- Select a country the shop does not ship to — verify an appropriate message is shown and checkout is blocked.
- Verify that customs/import duty notices are shown for non-EU destinations if required.

### Click & collect / in-store pickup
- Select "Click & Collect" as the shipping option and verify a list of available pickup locations is displayed.
- Select a pickup location and verify the address, opening hours, and contact information are displayed.
- Complete an order with Click & Collect selected — verify the confirmation email includes the pickup location details and collection instructions.
- Verify that shipping cost is zero (or a configured amount) for Click & Collect orders.
- Verify that the customer is notified when the order is ready for pickup.
- Verify that normal shipping address fields are optional or hidden when Click & Collect is selected.
- Attempt Click & Collect for an item only available in one location — verify only that location is shown.

---

## 8.3 Payment Methods

### Credit card (Visa, Mastercard, Amex)
- Select credit card as payment method and verify the card input form (card number, expiry date, CVV) is displayed.
- Enter a valid Visa test card number and verify successful payment processing.
- Enter a valid Mastercard test card number and verify successful payment processing.
- Enter a valid Amex test card number (4-digit CVV) and verify successful payment processing.
- Enter an expired card — verify a clear error message is shown and the order is not placed.
- Enter an invalid CVV — verify validation rejects it.
- Enter a card number with incorrect length — verify real-time input validation rejects it.
- Verify the card number field formats input with spaces (e.g., 4242 4242 4242 4242) for readability.
- Verify that card data is never stored in plain text and is handled exclusively via the payment provider's secure iframe or hosted fields.

### PayPal
- Select PayPal as payment method and verify a "Pay with PayPal" button or redirect is presented.
- Click the PayPal button and verify the PayPal login/payment window opens.
- Complete a PayPal sandbox payment and verify the order is placed and confirmation page is shown.
- Cancel the PayPal flow mid-process — verify the user is returned to the checkout with their cart intact.
- Verify that the order total shown in the PayPal payment window matches the cart total.
- Complete a PayPal payment and verify the order status updates correctly in the backend.
- Verify that PayPal payments capture the correct currency.

### PayPal Express
- Verify the PayPal Express button is visible on the cart page and/or checkout page.
- Click PayPal Express — verify it opens the PayPal flow and pre-fills shipping address from the PayPal account.
- Verify the shipping cost is recalculated based on the address received from PayPal.
- Complete an express checkout order via PayPal Express — verify the full order confirmation flow works.
- Cancel PayPal Express — verify the user is returned to the previous page with cart intact.
- Verify that address information transferred from PayPal Express passes the same validation as manually entered addresses.

### Klarna (invoice, installments, instant)
- Select Klarna Invoice — verify the Klarna widget loads and shows the invoice option.
- Select Klarna Installments — verify the available payment plans are shown with monthly amounts and total cost.
- Select Klarna Instant (Pay Now) — verify the option is presented correctly.
- Complete a Klarna Invoice order with valid test credentials — verify the order is placed and confirmation shown.
- Verify that Klarna eligibility decisions (approved/rejected) are handled gracefully with a clear message if rejected.
- Verify that the Klarna widget does not interfere with other page elements on mobile.
- Verify the Klarna payment method is hidden for customers in countries where Klarna is not available.
- Verify that the correct Klarna locale/language is used based on the customer's country.

### Amazon Pay
- Verify the Amazon Pay button is displayed when this payment method is enabled.
- Click Amazon Pay — verify the Amazon sign-in flow opens correctly.
- Complete a test Amazon Pay order and verify the confirmation page is shown.
- Cancel the Amazon Pay flow — verify the user is returned to checkout with cart intact.
- Verify that the shipping address transferred from Amazon Pay is correctly applied to the order.

### Apple Pay
- Open checkout in Safari on an Apple device (or a browser with Apple Pay test mode) — verify the Apple Pay button is displayed.
- Verify the Apple Pay button is NOT displayed in non-Safari browsers or on non-Apple devices.
- Tap the Apple Pay button — verify the Apple Pay sheet appears with the correct order total and merchant name.
- Complete an Apple Pay payment — verify the order is placed and the confirmation page is shown.
- Cancel Apple Pay — verify the user is returned to checkout with cart intact.
- Verify Apple Pay is not shown when the device has no payment cards enrolled.

### Google Pay
- Open checkout in a Chrome browser on Android or a desktop with a saved Google Pay card — verify the Google Pay button is displayed.
- Click the Google Pay button — verify the Google Pay payment sheet opens with the correct total.
- Complete a Google Pay test payment — verify the order is placed and confirmation page is shown.
- Cancel Google Pay — verify the user is returned to checkout with cart intact.
- Verify the total amount shown in the Google Pay sheet matches the cart total including shipping and taxes.

### SEPA direct debit
- Select SEPA Direct Debit as payment method — verify an IBAN input field is shown.
- Enter a valid German IBAN (e.g., DE89370400440532013000) — verify it is accepted.
- Enter an IBAN with an incorrect format — verify a validation error is shown.
- Enter an IBAN for a non-SEPA country — verify it is rejected or flagged.
- Verify the SEPA mandate text and legal notice are displayed before order placement.
- Verify that the IBAN is masked/hidden after entry (only last 4 digits visible).
- Complete a SEPA order and verify the confirmation email includes the mandate reference number.

### Bank transfer / prepayment
- Select bank transfer/prepayment as payment method — verify the option is displayed.
- Place an order with bank transfer — verify the confirmation page and email include the shop's bank account details and the payment reference (order number).
- Verify that the order status is set to "awaiting payment" (not "paid") after a bank transfer order.
- Verify that the payment reference is unique per order (e.g., the order number).
- Verify that the bank transfer instructions are clearly formatted and include IBAN, BIC, bank name, account holder, and reference.

### Cash on delivery
- Select cash on delivery as payment method — verify the option is displayed with the applicable surcharge.
- Verify the cash on delivery surcharge is added to the order total in the summary.
- Complete an order with cash on delivery — verify the order is placed and the confirmation email mentions the COD amount.
- Verify cash on delivery is only available for supported delivery zones (not international if configured that way).
- Remove the cash on delivery surcharge from the total if the option is switched to another method — verify it disappears from the summary.

### Purchase on invoice
- Select "Purchase on Invoice" — verify the payment option is displayed (shop's own invoicing, not Klarna).
- Verify the payment terms (e.g., "Payment due within 14 days") are displayed alongside the option.
- Complete a purchase on invoice order — verify the order is placed and an invoice PDF is sent with the confirmation email.
- Verify that purchase on invoice is only available for logged-in customers or customers who meet configured eligibility criteria.
- Verify that first-time customers or high-value orders trigger additional verification if configured.

### Installment payments
- Select installment payment — verify available payment plans with number of installments, monthly amounts, and total cost (including any interest) are shown.
- Verify the APR/interest rate is displayed where legally required.
- Complete an installment payment order with test credentials — verify the order is placed successfully.
- Verify that installment payments are only available above a configured minimum order value.
- Verify that cancelling an installment plan (if the provider supports it) mid-flow returns the user to checkout.

### Payment method selection UI
- Verify all enabled payment methods are listed with their logos and names.
- Verify that selecting one payment method deselects the previous one (radio button behaviour).
- Verify that the payment method form (e.g., card fields) only appears after the method is selected.
- Verify that payment method logos and labels are displayed correctly on both desktop and mobile.
- Verify that if only one payment method is available, it is pre-selected automatically.
- Verify that payment methods are sorted in a logical, consistent order.

### Payment method validation
- Attempt to proceed to the next checkout step without selecting a payment method — verify an error message prompts the user to select one.
- Select a payment method that requires additional input (e.g., credit card) and attempt to proceed without entering details — verify field-specific errors are shown.
- Enter invalid card details and verify real-time validation highlights the error before form submission.
- Verify that payment method validation errors are shown inline (next to the field) and not as a generic page error.

### 3D Secure / SCA
- Pay with a test card that triggers 3D Secure — verify the 3DS authentication challenge is presented.
- Complete the 3DS challenge successfully — verify the order is placed and the confirmation page is shown.
- Fail the 3DS challenge (e.g., wrong OTP) — verify a clear error message is shown and the order is not placed.
- Cancel the 3DS flow — verify the user is returned to checkout with cart intact.
- Pay with a card that does not require 3DS — verify the payment completes without an authentication challenge.
- Verify that 3DS2 (frictionless flow) is used where applicable and only triggers a challenge when required by the issuing bank.

### Failed payment (handling + error message)
- Use a test card configured to decline — verify a user-friendly error message is displayed (e.g., "Your payment was declined. Please try a different card or contact your bank.").
- Verify the error message does not expose technical details or raw gateway error codes.
- Verify the user can retry payment with a different method after a failure without losing cart or address data.
- Verify that a failed payment does not create a duplicate order in the backend.
- Test a card that fails with "insufficient funds" — verify an appropriate message is shown.
- Test a card that fails with "card expired" — verify an appropriate message is shown.
- Verify that after 3 consecutive payment failures, the user is advised to contact support.

### Payment timeout
- Simulate a payment that times out at the gateway — verify the user receives an appropriate error message.
- Verify the user can retry payment after a timeout.
- Verify that the order is not marked as "paid" if the payment timed out.
- Verify that any payment intent or reservation is cancelled after a timeout.
- Verify that the session and cart are preserved so the user can re-attempt checkout.

### Prevent double payment
- Click the "Place Order" button rapidly multiple times — verify only one payment attempt is made and only one order is created.
- Disable the "Place Order" button after the first click — verify it becomes unclickable immediately.
- Navigate back after a successful payment and attempt to resubmit the order form — verify the system rejects the duplicate submission.
- Verify that the payment intent ID is unique per checkout session and cannot be reused for a second payment.
- Verify that if the user opens checkout in two browser tabs and completes payment in one, the second tab shows an appropriate message.

---

## 8.4 Order Summary

### All items correctly listed
- Add multiple different products to the cart and proceed to checkout — verify all items appear in the order summary with correct names, quantities, and variants.
- Add a configurable product with options — verify the selected options (e.g., size, colour) are displayed in the summary.
- Add the same product in two different variants — verify both variants are shown as separate line items.
- Verify that product images appear next to each line item in the order summary.
- Verify that clicking a product name in the order summary links back to the product page (or is non-clickable, consistently).
- Update the quantity of an item in the cart — verify the order summary on the checkout page reflects the change.

### Prices correct
- Verify that each line item displays the correct unit price matching the product page.
- Verify that the line item total (unit price × quantity) is correctly calculated.
- Verify that any variant-based price surcharges are reflected in the line item price.
- Verify that sale prices (not regular prices) are shown if a product is on sale.
- Add a product with a tiered/volume discount — verify the discounted price is shown in the summary.
- Verify that the sum of all line item totals equals the displayed subtotal.

### Shipping costs displayed
- Verify that the shipping cost line is displayed separately in the order summary with the selected shipping method name.
- Change the shipping method on the shipping step — verify the shipping cost in the summary updates.
- Qualify for free shipping — verify the shipping line shows "Free" or "€0.00" rather than disappearing entirely.
- Verify the shipping cost is shown inclusive of VAT where applicable (or exclusive with VAT note).
- Verify that express shipping costs are correctly labelled and distinguishable from standard shipping.

### VAT breakdown
- Verify that a VAT breakdown is shown in the order summary (e.g., "Incl. 19% VAT: €X.XX").
- Verify the VAT amount is mathematically correct relative to the net total.
- For orders with products at different VAT rates, verify each rate is listed separately.
- Select a delivery country with a different VAT rate — verify the VAT amount updates accordingly.
- For B2B customers with a valid VAT ID in another EU country, verify reverse charge is applied and VAT is shown as €0.00.
- Verify that the "Total" shown is the gross amount (incl. VAT) in B2C mode.

### Coupon discount displayed
- Apply a valid percentage coupon — verify the discount amount is shown as a separate line in the order summary.
- Apply a fixed-amount coupon — verify the exact discount amount is shown.
- Apply a free shipping coupon — verify shipping is reduced to €0.00 and the coupon is listed in the summary.
- Verify the coupon code itself is shown next to the discount amount (e.g., "Coupon SAVE10: -€5.00").
- Verify that removing the coupon removes the discount line from the summary and the total updates immediately.
- Apply a coupon that covers the entire order value — verify the total shows €0.00 and the payment step is handled gracefully.

### Total amount correct
- Verify the displayed total equals: subtotal + shipping − coupon discount + any surcharges (including VAT).
- Change quantity, shipping method, and apply a coupon — verify the total updates correctly after each change.
- Verify the total shown at checkout matches the amount charged by the payment provider (check payment intent amount).
- Verify the total on the confirmation page matches the total shown during checkout.
- Verify the total on the invoice matches the total shown at checkout.
- Verify correct rounding (to 2 decimal places) when totals include fractions of cents.

### Terms & conditions checkbox
- Verify a checkbox labelled with a link to the Terms & Conditions is present before the "Place Order" button.
- Attempt to place an order without checking the T&C checkbox — verify an error message blocks submission.
- Verify the T&C link opens the full terms document (in a new tab or modal).
- Verify the checkbox is unchecked by default (no pre-checked opt-in).
- Verify the checkbox and its label are visible and accessible on mobile.

### Cancellation policy checkbox
- Verify a checkbox acknowledging the cancellation policy is present (required by EU law for digital/made-to-order goods where applicable).
- Attempt to place an order without checking the cancellation policy checkbox — verify an error blocks submission.
- Verify the cancellation policy link opens the correct policy document.
- Verify the checkbox is unchecked by default.
- Verify the cancellation policy checkbox label is clearly worded and legally compliant (e.g., "I have read and acknowledge the cancellation policy").

### Privacy policy checkbox
- Verify a reference to the privacy policy is present in the checkout (checkbox or inline text with link).
- If a checkbox is required, attempt to place an order without checking it — verify an error blocks submission.
- Verify the privacy policy link opens the correct privacy policy document.
- Verify the checkbox is unchecked by default.
- Verify that the privacy policy is up to date and includes all required GDPR information.

### Place order button
- Verify the "Place Order" button is clearly visible and labelled in accordance with the "button solution" legal requirement (e.g., "Order with obligation to pay" / "Zahlungspflichtig bestellen").
- Verify the button is disabled until all required checkboxes are checked.
- Click the button once — verify a loading state is shown and the button is disabled immediately to prevent double submission.
- Verify a successful click leads to the order confirmation page.
- Verify the button text complies with applicable consumer protection law in the target market.

### Double-submit protection
- Click "Place Order" once and immediately click again — verify only one order is created in the backend.
- After a successful order, use the browser back button and attempt to resubmit — verify the system detects the duplicate and shows an appropriate message.
- Verify the order submission uses a unique token (nonce/CSRF) that is invalidated after first use.
- Simulate a slow network by throttling bandwidth — click "Place Order" once and verify the button remains disabled until a response is received.
- Check the backend order list after a rapid double-click test and confirm no duplicate orders exist.

---

## 8.5 Order Confirmation

### Thank-you page display
- Complete a successful order and verify the thank-you/confirmation page loads immediately after payment.
- Verify the page displays a clear success message (e.g., "Thank you for your order!").
- Verify the page is fully responsive and readable on mobile devices.
- Verify the page includes a link to continue shopping or return to the homepage.
- Verify the page does not reload or change state on browser refresh after the order is placed.
- Verify that the page is accessible without login for guest orders (via the unique URL).

### Order number displayed
- Verify the order number is prominently displayed on the confirmation page.
- Verify the order number format matches the format in the admin backend.
- Verify the order number matches the order number in the confirmation email.
- Verify the order number is unique (place two orders and confirm they receive different numbers).
- For logged-in customers, verify the order number matches the one visible in the order history.

### Order summary
- Verify the confirmation page shows a complete summary: all ordered items with names, quantities, variants, and prices.
- Verify the shipping address and billing address are shown.
- Verify the selected shipping method and delivery time estimate are shown.
- Verify the payment method used is shown (e.g., "Paid with Visa ending in 4242").
- Verify the total paid (including shipping and VAT) is shown.
- Verify that coupon discounts are reflected in the summary if a coupon was applied.

### Confirmation email sent
- Complete an order and verify a confirmation email arrives at the provided email address within 2 minutes.
- Verify the email is sent to the correct address (the one entered during checkout).
- Complete a guest order and verify the confirmation email is sent without requiring account creation.
- Place a test order and check spam/junk folders — verify the email is not misclassified as spam.
- Verify the sender name and email address are correctly configured (shop name, valid reply-to address).
- Verify the confirmation email is not sent twice for the same order.

### Confirmation email content correct
- Verify the email subject line contains the order number and/or shop name.
- Verify the email body contains the order summary: items, quantities, prices, total.
- Verify billing and shipping addresses are shown in the email.
- Verify the shipping method and estimated delivery date are mentioned.
- Verify the payment method is mentioned.
- Verify the email contains a link to view the order status online.
- Verify the email contains contact details for customer support.
- Verify the email contains legal information: link to T&C, cancellation policy, and privacy policy.
- Verify the email renders correctly in Gmail, Outlook, and Apple Mail on both desktop and mobile.
- Verify the email renders correctly in dark mode.

### Redirect after payment (redirect URLs)
- After a successful PayPal payment, verify the redirect URL leads back to the shop's confirmation page (not a generic PayPal page).
- After a successful Klarna payment, verify the redirect lands on the correct confirmation page.
- After a successful 3DS payment, verify the redirect URL from the card issuer returns correctly to the confirmation page.
- After cancelling a PayPal payment, verify the redirect returns to the checkout page (not a 404 or error page).
- After a failed payment, verify the redirect returns to the checkout page with an appropriate error message.
- Verify that redirect URLs are validated server-side and do not allow open redirects to external domains.
- Verify that all redirect URLs are HTTPS.

### Tracking pixel / conversion tracking
- Complete a test order and verify the purchase event fires in Google Analytics 4 (check via GA4 DebugView or GTM Preview Mode).
- Verify the GA4 purchase event contains: transaction_id (order number), value (revenue), currency, items array with correct products.
- Verify the Google Ads conversion tag fires after successful order completion.
- Verify the Meta/Facebook Pixel Purchase event fires with the correct value and currency.
- Verify that no tracking events fire if the user has rejected consent in the cookie banner.
- Verify tracking events do not fire if the user reloads the confirmation page (duplicate conversion prevention).
- Verify the revenue tracked equals the actual order total (no discounts missing, correct currency).
- Verify the Google Tag Manager dataLayer contains the correct purchase event data on the confirmation page.
