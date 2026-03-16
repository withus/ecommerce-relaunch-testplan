# 31. Returns / Refunds / Cancellations

### Cancellation during order processing (before shipping)

- Place an order and immediately request cancellation while status is "Received" — verify cancellation is accepted and a confirmation email is sent
- Place an order, wait until status transitions to "Paid", then request cancellation — verify system allows cancellation at this stage and processes it correctly
- Place an order, wait until status transitions to "Processing", then request cancellation — verify system correctly rejects or routes to manual review depending on business rules
- Cancel an order paid via credit card before shipping — verify the payment is voided (not refunded) if not yet captured, or fully refunded if already captured
- Cancel an order paid via PayPal before shipping — verify PayPal payment is fully voided/refunded and the customer receives confirmation
- Cancel an order paid via bank transfer (prepayment) that has not yet been paid — verify order is marked cancelled and no payment is due
- Cancel an order containing multiple items before shipping — verify all items are cancelled, total refund is correct, and all line items appear in the cancellation confirmation email
- Cancel an order that used a coupon code — verify the coupon is restored/re-credited to the customer if it was single-use
- Attempt to cancel an order via customer account self-service — verify the cancel button is only visible/active when cancellation is still possible (before shipping)
- Admin cancels an order on behalf of customer — verify customer receives cancellation email with reason, and refund is triggered automatically
- Cancel an order that has a gift card applied — verify the gift card balance is restored to the original amount
- Cancel an order with a loyalty points discount applied — verify loyalty points used are returned to the customer's account

### Cancellation after shipping / during delivery

- Attempt to cancel an order already marked as "Shipped" via the customer self-service portal — verify the system prevents cancellation and displays an informative message directing the customer to initiate a return instead
- Attempt to cancel an order with an active tracking number via the admin backend — verify admin sees a clear warning that the parcel is in transit and must follow return process
- Customer contacts support to cancel after shipping — verify the support workflow prompts for a return label to be created rather than a direct cancellation
- Order is in "Out for Delivery" status — verify cancellation attempt is blocked with a message explaining the item will need to be refused at the door or returned
- Customer refuses delivery at the door — verify once the parcel is marked as "Returned to Sender" by the carrier, the order status updates and a refund is triggered automatically (or queued for admin review)
- Order is cancelled mid-transit but the parcel is already delivered — verify the system does not auto-refund before the item is physically received back, and the status correctly transitions to awaiting return
- Admin manually marks a shipped order as cancelled — verify a warning appears requiring confirmation that physical goods have been accounted for before the cancellation is finalized
- Shipping carrier loses the parcel during cancellation-in-transit scenario — verify there is a process to handle the claim and refund without requiring item return

### Return request by customer (self-service portal)

- Logged-in customer navigates to order history, selects an eligible order, and submits a return request — verify the request is created, a confirmation email is sent, and the return appears in the admin backend
- Customer attempts to submit a return request for an order outside the return period — verify the portal blocks the request and displays the return deadline clearly
- Customer submits a return request selecting only one item from a multi-item order (partial return) — verify only the selected item(s) are flagged for return and the refund calculation reflects only those items
- Customer submits a return request without a required reason — verify the form validates that a reason is selected or entered before submission
- Customer selects "wrong item received" as reason — verify the portal captures the reason and optionally triggers a different internal workflow (exchange vs. refund)
- Customer submits a return request for a product with non-returnable status (e.g., personalized, hygiene item) — verify the portal blocks the request or routes it to manual review with a clear explanation
- Guest customer attempts to initiate a return using only an order number and email address — verify the system can look up the order and present the return flow without requiring login
- Customer submits a return request with an uploaded photo of the damaged item — verify the image is attached to the return case and visible in the admin backend
- Customer submits multiple return requests for the same order on different dates (phased returns) — verify the system handles overlapping partial returns correctly without duplicating refunds
- Customer submits a return request via mobile device — verify the self-service portal is fully functional on mobile with usable form controls and file upload

### RMA process (Return Merchandise Authorization)

- Submit a return request and verify an RMA number is generated automatically and included in the confirmation email
- Verify the RMA number is unique and not reused across multiple return requests
- Admin reviews a pending return request and approves it — verify the status changes to "Approved" and the customer receives a notification with RMA number and next steps
- Admin reviews a return request and rejects it (outside return window, non-returnable item) — verify the customer receives a rejection email with a clear reason
- Admin marks an RMA as "Item Received" after the goods arrive at the warehouse — verify the return status updates and the refund or exchange workflow is triggered
- Item received in damaged condition beyond what was claimed — verify admin can flag this and trigger a partial refund or dispute flow
- RMA approval automatically generates a return label and sends it to the customer — verify the label is downloadable and contains correct sender/recipient details
- RMA is created for an exchange — verify the exchange order is linked to the original RMA and can be tracked separately
- Admin searches for an RMA by order number, customer email, and RMA number — verify all search methods return correct results
- RMA status history is auditable — verify each status change (Requested → Approved → In Transit → Received → Resolved) is timestamped and visible in the admin view

### Return label generation and download

- Approved return triggers automatic generation of a pre-paid return label — verify the label is sent via email and available for download in the customer account
- Customer downloads the return label as a PDF — verify the PDF is valid, correctly formatted, and contains barcode/QR code that the carrier can scan
- Return label contains the correct return address (warehouse, not shop address) — verify no mismatch exists between the displayed and encoded address
- Return label is generated for DHL, DPD, GLS, and Hermes — verify each carrier's label format meets that carrier's specification
- Label generation fails due to a carrier API error — verify the customer receives a fallback message (e.g., contact support) and the failure is logged for admin review
- Customer loses the return label email — verify they can re-download it from the customer account order/return section without needing to contact support
- Return label has an expiry date — verify the label becomes invalid after the specified period and the customer is notified before expiry if the item has not been shipped
- International return: customer is outside Germany — verify correct international return label or carrier instructions are provided and duties/customs implications are communicated
- Return label download is available immediately after RMA approval without page reload — verify no caching issues prevent timely availability
- Return label barcode is scanned successfully (simulate carrier scan via test scan or visual validation tool) — verify the tracking number resolves to the correct return shipment

### Refund to original payment method

- Complete a return for a credit card order — verify refund is issued to the exact same card used for purchase, not to a different card or bank account
- Complete a return for a PayPal order — verify refund appears in the customer's PayPal account within the expected timeframe (typically 3–5 business days) and a refund confirmation email is sent
- Complete a return for a Klarna invoice order — verify the Klarna invoice is updated or cancelled and the customer is no longer required to pay the returned amount
- Complete a return for a Klarna installment order — verify installment plan is adjusted and any overpaid amounts are returned
- Complete a return for a SEPA direct debit order — verify refund is credited back to the originating bank account and the customer is informed of the expected processing time
- Complete a return for a bank transfer (prepayment) order — verify admin is prompted to collect the customer's IBAN for the refund and the refund is processed correctly
- Refund for a credit card order where the original card has been cancelled or expired — verify the payment provider's fallback process is triggered (typically still processes to the card's underlying account) and an error or notice is shown if it fails
- Refund is issued for a payment made with a gift card — verify the gift card balance is restored, not credited to a bank account
- Refund confirmation email is sent to the customer — verify email contains correct refund amount, payment method, and expected processing time
- Refund appears correctly in the payment gateway's dashboard (Stripe, PayPal) — verify amounts match and no duplicate refund was issued
- Full refund is issued for an order including original shipping costs — verify shipping costs are included in the refund amount when legally required (e.g., EU consumer rights)
- Full refund is issued but the customer paid return shipping themselves — verify only the original order amount (minus return shipping if applicable) is refunded

### Partial refund for partial return

- Customer returns 1 of 3 items in an order — verify the refund amount equals exactly the price of the returned item (including proportional VAT), not the full order total
- Customer returns 2 of 3 items where one item had a volume discount applied — verify the discount is recalculated correctly for the remaining item, and the partial refund reflects the actual paid price for the returned items
- Customer returns items from an order where a fixed-amount coupon was applied (e.g., €10 off) — verify the coupon discount is distributed proportionally across the returned items and the refund is not over-issued
- Customer returns items from an order where a percentage coupon was applied — verify the partial refund reflects the discounted price actually paid, not the gross price
- Admin issues a partial refund manually via the admin backend (e.g., for a discount without return) — verify the refund amount input is validated, cannot exceed the remaining refundable amount, and a reason is required
- Partial refund is issued for an item returned in damaged condition — verify admin can enter a reduced refund amount and the customer receives the adjusted amount with an explanation
- Partial return reduces order total below the free shipping threshold — verify system does not retroactively charge shipping for the retained items (or documents the correct handling per business rules)
- Multiple partial refunds are issued for the same order over time — verify the system tracks the cumulative refunded amount and prevents the total from exceeding the original order value
- Partial refund confirmation email lists only the returned items and their individual prices, plus the partial refund total — verify no unrelated line items appear in the email
- Partial refund is reflected correctly in financial reports and accounting exports — verify the transaction record shows both the original charge and the partial credit

### Store credit instead of refund

- Customer selects "store credit" as their preferred refund method during the return request — verify a gift card or account credit of the correct amount is issued after the return is processed
- Store credit is added to the customer's account balance — verify it appears in the customer account dashboard with the correct amount and expiry date (if applicable)
- Store credit is applied at checkout on the next order — verify the credit reduces the order total correctly and cannot be applied beyond the available balance
- Store credit covers full order total on next purchase — verify checkout allows €0.00 payment and the order is placed successfully
- Store credit is issued for a partial return — verify only the value of the returned items is credited, not the full order amount
- Store credit expires after a defined period (e.g., 12 months) — verify expired credits cannot be applied at checkout and the customer receives a reminder email before expiry
- Store credit is not transferable between accounts — verify that store credit issued to account A cannot be used by account B
- Admin manually issues store credit as goodwill compensation — verify the credit is added to the customer account, is visible in the admin backend, and is not associated with a specific return order
- Customer has both a coupon code and store credit — verify both can be applied in the same checkout and the combined discount is handled correctly
- Store credit is refunded when a store-credit-funded order is itself cancelled — verify the credit is returned to the account, not to a payment method

### Exchange workflow (different product/variant)

- Customer requests an exchange for a different size of the same product — verify an exchange order is created, linked to the original RMA, and the correct variant is reserved in inventory
- Customer requests an exchange for a completely different product — verify the price difference (if any) is calculated and either charged or refunded accordingly
- Exchange item has a higher price than the returned item — verify the customer is prompted to pay the difference and the payment is processed via their original payment method or a new transaction
- Exchange item has a lower price than the returned item — verify the price difference is refunded to the original payment method or issued as store credit per the customer's preference
- Exchange item is out of stock — verify the customer is notified and offered alternatives (backorder, different variant, full refund)
- Exchange order is shipped before the return item is received (advance exchange) — verify this is only possible if the business rules allow it and a hold or pre-authorization is placed on the customer's payment method
- Exchange order tracking is visible in the customer's account linked to the original order — verify no confusion between original order, return, and exchange shipment
- Customer cancels the exchange request before the replacement is shipped — verify the exchange order is cancelled, the original return is still processed, and a refund is issued instead
- Exchange product is a different category with a different VAT rate — verify the invoice for the exchange order reflects the correct VAT and the refund for the original item also uses the correct rate
- Admin processes an exchange manually in the backend — verify stock is deducted for the replacement product and the customer account reflects the complete exchange history

### Return period check (automatic rejection after deadline)

- Customer submits a return request exactly on the last valid day of the return period (e.g., day 14) — verify the request is accepted
- Customer submits a return request one day after the return period expires — verify the request is automatically rejected and the customer receives a clear message with the deadline that was missed
- Return period is calculated from the delivery date, not the order date — verify the system uses the confirmed delivery date (from carrier tracking) as the start of the return period
- Order has no confirmed delivery date (carrier tracking missing) — verify the system falls back to estimated delivery or order date plus a buffer, and documents this clearly
- Business extends the return period for a specific product category (e.g., 30 days for electronics) — verify the extended deadline is applied correctly and the standard 14-day check does not reject valid requests
- Admin manually overrides the deadline for a specific customer (goodwill exception) — verify the override is logged, the request is accepted, and the exception is visible in the return record
- Return period check accounts for legal holidays or weekends in different countries — verify the deadline extension is applied correctly for cross-border orders where consumer law may vary
- Automatic rejection email for out-of-period requests contains the correct deadline date and a customer service contact option — verify no incorrect dates are shown
- System sends a "return deadline reminder" email X days before the return window closes — verify the email is triggered at the correct time and contains the correct deadline and a direct link to initiate the return
- Return period for a subscription order — verify each delivery in a subscription has its own independent return window, not a shared window from the subscription start date

### Status updates for return via email/SMS

- Return request submitted — verify customer receives an immediate confirmation email with RMA number, summary of returned items, and next steps
- Return request approved by admin — verify customer receives an approval email with return label attachment (or download link), instructions, and expected processing time
- Return request rejected by admin — verify customer receives a rejection email with a clear reason and information about how to appeal or contact support
- Return label is downloaded by the customer — verify no additional email is triggered (avoid confirmation email spam), unless the business rule requires it
- Carrier scan confirms the return shipment is in transit — verify customer receives an "We received your return shipment" or "Return in transit" notification
- Return item is received at the warehouse — verify customer receives a "Your return has been received" email with the date
- Refund is issued — verify customer receives a refund confirmation email with the exact amount, refund method, and expected processing time
- Exchange order is shipped — verify customer receives a shipping confirmation email with tracking number for the replacement item
- Return is closed/resolved — verify customer receives a final status update and the return case is marked as closed in their account
- SMS notifications are sent for key status changes (if SMS is enabled) — verify SMS messages contain the correct order/RMA number, are concise, and include a link to the customer account for details
- All return-related emails render correctly on mobile and desktop — verify no broken layouts, missing images, or unformatted text in email clients (Gmail, Outlook, Apple Mail)
- Email sender address and reply-to are correctly configured for return emails — verify replies go to the customer service inbox, not a no-reply address
