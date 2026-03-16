# 16. Transactional Emails

### Registration confirmation

- Confirm that the registration confirmation email is sent immediately after a new account is created (within 60 seconds).
- Verify the recipient address matches exactly the email entered during registration (no truncation, no lowercasing errors).
- Check that the email subject line contains the shop name and clearly indicates account creation (e.g., "Your account at [Shop] has been created").
- Verify the customer's first and last name are correctly interpolated in the salutation.
- Confirm the email contains a link to the customer account dashboard and that the link resolves correctly.
- Check that the email does not contain any placeholder tokens (e.g., `{{first_name}}`, `%ORDER_ID%`) that were not replaced.
- Verify that registering with a long email address (e.g., 254 characters) does not break the email layout.
- Test that the confirmation email is not sent for admin-created accounts unless explicitly configured.
- Check the unsubscribe link is absent (registration confirmation is a transactional email and must not have a marketing unsubscribe option per GDPR).
- Verify the reply-to address points to a monitored support mailbox, not a no-reply address that silently discards replies.

---

### Double opt-in email

- Confirm the double opt-in email is sent immediately after newsletter opt-in during registration or standalone signup.
- Verify the confirmation link in the email is unique per user and contains a secure, unguessable token.
- Click the confirmation link and verify that the user's newsletter subscription is activated in the database.
- Attempt to click the same confirmation link a second time and verify it returns a "already confirmed" or graceful expiry message rather than an error.
- Verify the confirmation link expires after a configurable period (e.g., 24 or 48 hours) and that clicking an expired link shows an appropriate message with an option to resend.
- Confirm the email clearly states what the user is confirming (newsletter subscription) and which shop it relates to.
- Test that no marketing emails are sent to the user until the double opt-in is completed.
- Verify the opt-in timestamp and IP address are stored server-side for legal proof of consent.
- Check that the email passes spam filter checks (no broken HTML, valid sender domain, text-to-image ratio acceptable).
- Verify the email renders correctly when the confirmation link is very long (some clients wrap links).

---

### Password reset email

- Trigger a password reset request and confirm the email arrives within 60 seconds.
- Verify the reset link contains a cryptographically secure token (minimum 32 random bytes, not predictable).
- Confirm the reset link is single-use: after the password is changed, the same link must no longer work.
- Verify the reset link expires after a defined period (e.g., 1 hour) and that an expired link shows a clear error with a prompt to request a new one.
- Check that the email does not disclose whether the entered email address has an account (security: account enumeration prevention); the response in the UI should be identical for known and unknown addresses.
- Verify the email contains the user's name in the salutation (where available) but does not include the old password, partial password, or any password hint.
- Confirm the email includes the requesting IP address and timestamp so the user can detect unauthorized reset attempts.
- Test that multiple reset requests invalidate all previous tokens (only the most recent link should work).
- Verify the email is sent only to the confirmed primary email address, not to old/unverified addresses.
- Check that the reset flow works end-to-end: email received → link clicked → new password set → login with new password succeeds.

---

### Order confirmation

- Confirm that an order confirmation email is sent within 60 seconds of successful order placement.
- Verify the order number in the email matches the order number shown on the thank-you page and in the customer account.
- Check that all ordered items are listed with: product name, variant (size/color/material), quantity, unit price, and line total.
- Verify the pricing summary is complete and correct: subtotal, shipping costs, applied coupon/discount, VAT breakdown (rate and amount), and grand total.
- Confirm the delivery address and billing address displayed in the email match what was entered at checkout.
- Verify the selected shipping method and estimated delivery time are shown.
- Check that the applied payment method is mentioned (e.g., "Payment via PayPal").
- Confirm links to the order detail page in the customer account are present and functional.
- Test that guest order confirmations include a link to view the order without logging in (using a secure order token), and that this token cannot be guessed.
- Verify the email is sent to the billing email address and not duplicated to additional addresses unless explicitly configured.
- Check that product images in the email are hosted on a reliable CDN and load correctly (not broken image icons).
- Verify the email is not sent if the order was not actually completed (e.g., payment failed mid-flow).

---

### Payment confirmation

- Confirm a separate payment confirmation email is sent when payment is successfully captured (distinct from the order confirmation where applicable).
- Verify the email contains the transaction ID or payment reference from the payment provider.
- Check that the amount confirmed matches the order total and is displayed in the correct currency with the correct formatting.
- Verify the payment method used is clearly identified (credit card ending in XXXX, PayPal email, etc.) without disclosing full card numbers.
- Confirm that for prepayment/bank transfer orders, the payment confirmation is sent when the payment is received, not when the order is placed.
- Verify the email includes a link to download the invoice PDF or access it in the customer account.
- Test that no payment confirmation is sent for failed or declined transactions.
- Check that the email is not sent twice in cases of duplicate webhook delivery from the payment provider.

---

### Shipping confirmation with tracking link

- Confirm the shipping confirmation email is triggered when the order status is changed to "Shipped" / when a tracking number is assigned.
- Verify the tracking number displayed in the email is correct and matches the shipment booked with the carrier.
- Confirm the tracking link redirects to the correct carrier tracking page and pre-populates the tracking number.
- Test the tracking link for all supported carriers (DHL, DPD, GLS, UPS, Hermes, etc.) and verify each resolves correctly.
- Verify the email lists the items included in the shipment (important for partial shipments).
- For partial shipments, confirm that only the shipped items are listed and that the customer is informed about remaining items.
- Check the estimated delivery date is included and is a realistic future date based on the shipping method.
- Verify the shipping address shown in the email matches the address actually used for the shipment.
- Confirm the carrier name and service level (standard, express) are mentioned.
- Test that the tracking link is accessible without login (deep link directly to carrier site).
- Verify the email is sent to the email address associated with the order, not a cached/stale address.

---

### Delivery confirmation

- Confirm the delivery confirmation email is triggered when the carrier reports delivery (via webhook or polling).
- Verify the email mentions the correct delivery date and time (if available from the carrier API).
- Check that the email includes a prompt for the customer to leave a review (if review request is part of the delivery confirmation rather than a separate email).
- Verify the email contains a link to initiate a return if the customer is not satisfied.
- Confirm that delivery confirmation is not sent if the order was already cancelled or returned.
- Test behavior when the carrier marks an item as delivered but the customer disputes it: verify the email content does not create a false claim of confirmed receipt.
- Check that the email does not fire for every carrier status update (e.g., "out for delivery") but only on final delivery status.

---

### Invoice email (with PDF)

- Confirm the invoice email is sent at the correct trigger point (after payment capture, or when invoice is issued in the backend).
- Verify the PDF attachment is present in the email and is not corrupted (opens correctly in PDF readers).
- Check the PDF invoice contains all legally required fields: seller name and address, customer name and address, VAT ID, sequential invoice number, invoice date, line items with net price, VAT rate and amount per line, total net, total VAT, total gross.
- Verify the invoice number in the PDF matches the invoice number referenced in the email body.
- Confirm the invoice total in the PDF matches the order total the customer paid.
- Test that the invoice PDF is accessible via the download link in the customer account independently of the email.
- Verify the PDF is generated in the correct language (matches the customer's locale/language setting).
- Check that the PDF file size is reasonable (not exceeding email attachment limits, typically under 1 MB for a standard invoice).
- Verify the invoice email is not sent for cancelled orders or orders that were fully refunded before invoicing.
- Test that the invoice can be re-sent from the admin backend and that the resent email contains the same PDF.

---

### Review request (after delivery)

- Confirm the review request email is triggered only after a configurable delay post-delivery (e.g., 7 days), not immediately.
- Verify the email links directly to the review submission form, pre-authenticated where possible (so the customer does not need to log in separately).
- Check that only purchased products are listed in the review request (no products from cancelled or returned orders).
- Verify clicking the review link from a mobile device opens the review form in a mobile-optimised view.
- Confirm that customers who have already submitted a review for a product do not receive a second review request for that product.
- Verify the email contains an opt-out link allowing the customer to stop receiving review request emails.
- Test that submitting a review via the email link correctly associates the review with the verified purchase.
- Check that the email subject line and body copy do not promise rewards for reviews unless a reward program is actually in place (legal requirement in some jurisdictions).
- Confirm that the delay timer resets if the delivery date is updated (e.g., re-delivery after failed first attempt).

---

### Cart reminder (abandoned cart)

- Confirm the abandoned cart email is sent only after the configured abandonment delay (e.g., 1 hour after last cart activity) and only to customers who have provided their email address.
- Verify the email lists the exact products left in the cart with correct names, images, quantities, and prices.
- Check that prices in the abandoned cart email reflect current prices (not prices at time of abandonment if they have since changed), or clearly indicate any price change.
- Confirm the "Complete your order" CTA link restores the cart with the same items and redirects the customer directly to checkout.
- Verify out-of-stock items from the abandoned cart are either excluded from the email or flagged as unavailable.
- Test that guests who abandoned a cart only receive the email if they explicitly entered their email address during a checkout step.
- Confirm the abandoned cart email is not sent if the customer subsequently completed the purchase in a different session.
- Verify a maximum of one reminder email is sent per abandonment event (no repeated harassment unless a multi-step sequence is intentionally configured and disclosed).
- Check that an unsubscribe link is present and functional (GDPR/CAN-SPAM compliance for commercial communication).
- Test that clicking the cart restoration link from a different device or browser correctly loads the cart.

---

### Wishlist reminder

- Confirm wishlist reminder emails are only sent to customers who have opted in to such notifications.
- Verify the email lists the products on the wishlist with correct images, names, and current prices.
- Check that a price drop notification is triggered when a wishlist item's price decreases, and that the old and new prices are both shown.
- Verify the email flags any wishlist items that have gone out of stock.
- Confirm the CTA links go directly to the individual product pages, with the correct variant pre-selected where applicable.
- Test that items removed from the wishlist before the email is sent are not included in the reminder.
- Verify the unsubscribe link opts the user out of wishlist reminders specifically, not all transactional emails.
- Check that the reminder frequency respects the configured interval and does not send more frequently than intended.

---

### Newsletter welcome email

- Confirm the newsletter welcome email is sent immediately after double opt-in confirmation (not before).
- Verify the email contains a warm welcome message, a brief description of what the subscriber can expect, and potentially a welcome discount code.
- Check that any discount code included in the welcome email is valid, has the correct discount value, and has not already expired.
- Verify the discount code works at checkout without errors.
- Confirm the email contains a clearly visible unsubscribe link.
- Test that the welcome email is not sent a second time if the user re-subscribes after having previously unsubscribed.
- Verify the sender name and "from" address match the configured newsletter sender identity.
- Check that the email is sent in the correct language based on the subscriber's locale.
- Confirm the email is visually on-brand (correct logo, colors, fonts) and renders correctly across major email clients.

---

### Newsletter unsubscribe confirmation

- Confirm an unsubscribe confirmation email is sent within 60 seconds of a user clicking the unsubscribe link.
- Verify the email confirms the email address that was unsubscribed (to help users who manage multiple addresses).
- Check that the email does not contain any marketing content or CTAs to re-subscribe that violate the user's expressed opt-out intent.
- Confirm the user is actually removed from the active newsletter list in the backend immediately upon clicking unsubscribe (not just queued).
- Test that clicking a newsletter unsubscribe link does not unsubscribe the user from transactional emails (order confirmations, shipping notifications, etc.).
- Verify the unsubscribe is reflected in the newsletter settings section of the customer account.
- Confirm the unsubscribe link in the confirmation email itself does not trigger a second unsubscribe flow.
- Test that re-subscribing after unsubscribing requires a fresh double opt-in.

---

### Coupon email

- Confirm the coupon email is sent to the correct audience segment (e.g., birthday coupon sent on the correct date, loyalty coupon after qualifying purchase).
- Verify the coupon code displayed in the email is valid and correctly configured in the system (correct discount type, value, expiry date).
- Check that the coupon code's expiry date mentioned in the email matches the actual expiry date stored in the system.
- Test applying the coupon code from the email directly at checkout and confirm the discount is applied correctly.
- Verify the minimum order value (if applicable) is clearly stated in the email.
- Confirm that product or category restrictions for the coupon are clearly communicated in the email.
- Test that clicking a "Shop now" or "Redeem" CTA link in the email pre-populates the coupon code at checkout (if this feature is implemented).
- Verify the coupon email contains an unsubscribe link for marketing communications.
- Check that the coupon email is not resent if the code has already been redeemed.

---

### Return confirmation

- Confirm the return confirmation email is sent when a return request is submitted by the customer.
- Verify the email contains the RMA (Return Merchandise Authorization) number or return ID.
- Check that the items being returned are listed with their names, quantities, and the reason for return as submitted.
- Verify the return instructions are clearly written: shipping address, packaging requirements, whether a return label is included or needs to be generated.
- If a pre-paid return label is provided as an attachment or link, confirm the label is accessible and scannable.
- Verify the estimated processing time for the return and refund is mentioned.
- Confirm the email links to a return status tracking page in the customer account.
- Test that the return confirmation is triggered both for customer self-service returns and admin-initiated returns.

---

### Refund confirmation

- Confirm the refund confirmation email is sent when a refund is issued (full or partial).
- Verify the refunded amount displayed in the email matches the actual amount refunded in the payment system.
- Check that the refund destination is clearly stated (e.g., "refunded to your Visa card ending in 1234", "PayPal account", "store credit").
- Verify the estimated time until the refund appears is mentioned and is accurate for the payment method used.
- For partial refunds, confirm the email lists which specific items or amounts were refunded and any remaining balance.
- Test that the refund confirmation email is not sent before the refund is actually processed in the payment gateway.
- Verify the email references the original order number for easy cross-referencing.
- Confirm a credit note PDF is attached or linked if required by the accounting process.

---

### Account deletion confirmation

- Confirm the account deletion confirmation email is sent after a customer successfully deletes their account (GDPR right to erasure).
- Verify the email confirms that personal data has been deleted or scheduled for deletion in accordance with the privacy policy and legal retention requirements.
- Check that the email states which data is retained for legal reasons (e.g., invoices, tax records) and for how long.
- Verify the email is the last communication sent to this address (no further marketing or transactional emails should be sent after this point).
- Confirm the email is sent to the address being deleted (not to a different address on the account).
- Test that if re-registration with the same email address occurs after deletion, the user is treated as a completely new customer with no history carried over.
- Verify the deletion confirmation email does not contain links to the now-deleted account area (these would result in errors).

---

### Email rendering (desktop, mobile, dark mode)

- Test each transactional email in at least 3 desktop clients: Gmail (web), Outlook 2019/2021, Apple Mail.
- Test each transactional email in at least 3 mobile clients: Gmail (iOS), Gmail (Android), Apple Mail (iOS).
- Verify all emails render without broken layout when viewed in dark mode on iOS Mail, Gmail Android dark mode, and Outlook dark mode.
- Check that text remains legible in dark mode (no white text on white background, no dark text disappearing on dark backgrounds).
- Confirm all images have a defined background color or are designed to be visible on both light and dark backgrounds.
- Verify the email width is responsive: full width on mobile (below 600px), fixed-width centered layout on desktop.
- Check that font sizes are legible on mobile (minimum 14px for body text, 16px recommended for CTAs).
- Verify CTA buttons are large enough to tap on mobile (minimum 44x44px touch target).
- Confirm images that fail to load show meaningful alt text rather than blank spaces.
- Test email rendering in Outlook 2016/2019 specifically for any VML/table layout issues.
- Verify emails do not trigger "images blocked by default" warnings that render the email meaningless without images (important text must not be image-only).
- Use a tool such as Litmus or Email on Acid to capture rendering screenshots across 20+ client/OS combinations before go-live.

---

### Email links working

- Systematically click every link in every transactional email type and verify each resolves to the correct destination with a 200 HTTP status.
- Verify that order-specific links (order detail, invoice download) work for both logged-in users and for guest order tokens.
- Confirm that links using URL parameters (e.g., tracking tokens, pre-populated coupon codes) are correctly URL-encoded and do not break on special characters.
- Test that links wrapped by email tracking services (click tracking) redirect correctly to the final destination without intermediate error pages.
- Verify that links to the shop work correctly in both production and do not accidentally point to staging/development environments.
- Check that deep links to specific sections (e.g., "Go to your order" linking directly to a specific order) do not land on generic pages due to authentication redirects that lose the intended destination.
- Confirm that the unsubscribe link in every marketing-category email is a direct one-click unsubscribe (not a form requiring further input) to comply with RFC 8058 and Google/Yahoo requirements.
- Test link behavior when the user is already logged in vs. not logged in, ensuring appropriate redirects.

---

### Email sender / reply-to correct

- Verify the "From" name displayed in email clients is the shop name (not a technical internal identifier or ESP subdomain).
- Confirm the "From" email address uses the shop's own domain (e.g., `info@shop.com`, not `no-reply@mailprovider.com`).
- Verify the reply-to address is set to a monitored mailbox and is different from the "From" address only where intentional.
- Test that replying to each transactional email type routes to the appropriate team (e.g., reply to an order confirmation goes to customer service).
- Confirm that no-reply addresses, if used, display a clear message in the email body directing customers to the correct contact channel.
- Check the "Return-Path" / bounce address is configured to a valid mailbox so that bounces are captured and processed.
- Verify the sender name and address are consistent across all transactional email types (no mixture of different brands or subdomains).

---

### SPF / DKIM / DMARC correct

- Verify the SPF record for the sending domain is published in DNS and includes all sending IP ranges and ESP sources used.
- Confirm DKIM signing is configured: send a test email and check the DKIM signature header is present and valid (use MXToolbox or mail-tester.com).
- Verify the DMARC policy is published for the sending domain (`v=DMARC1; p=reject` or at minimum `p=quarantine` for production).
- Confirm DMARC alignment: the "From" domain matches the DKIM signing domain and/or the SPF-passing domain (strict or relaxed alignment as configured).
- Test sending a transactional email and check it passes Gmail's and Outlook's authentication checks (look for green lock / "via" indicators in the email client).
- Verify DMARC reports are being received and reviewed (check the `rua` tag in the DMARC record points to a monitored address or DMARC reporting service).
- Check the BIMI record if implemented (brand logo display in supporting email clients) is correctly configured and the VMC certificate is valid.
- Confirm no SPF includes exceed the 10 DNS lookup limit (which causes SPF permerror).

---

### Spam score

- Run each transactional email type through a spam scoring tool (mail-tester.com, GlockApps, or equivalent) and verify the score is 9/10 or higher.
- Check that the email's text-to-image ratio meets best practices (at minimum 60% text, not image-heavy emails with little text).
- Verify no spam trigger words are present in subject lines or body copy (e.g., "FREE", "WINNER", excessive exclamation marks).
- Confirm that HTML email code is clean, valid, and does not contain hidden text, misleading links, or mismatched URLs (link text shows one URL, href points to another).
- Verify the sending IP or shared ESP pool has a good reputation (not blacklisted); use MXToolbox Blacklist Check.
- Test that transactional emails are sent via a dedicated sending stream/IP separate from bulk marketing emails, to protect deliverability.
- Confirm the email headers do not contain unusual or non-standard fields that could trigger spam filters.
- Verify the unsubscribe header (`List-Unsubscribe`) is present for emails where applicable (required by Gmail/Yahoo for bulk senders from 2024).
- Test delivery to Gmail, Outlook, and a self-hosted mail server and verify the emails land in the inbox, not spam/junk folders.
- Confirm that the plain-text alternative (multipart/alternative) is present in every HTML email and contains a readable version of the email content.
