# 53. B2B-Specific Features (if applicable)

### Customer groups with different prices

- Verify that a B2B customer account assigned to a specific customer group sees prices different from the default retail price.
- Log in as a B2B customer from Group A (e.g., 15% off) and a B2B customer from Group B (e.g., 25% off) separately; confirm each sees their group-specific price on the same product detail page.
- Confirm that a guest (unauthenticated) user sees only the standard retail (gross) price and cannot access B2B pricing without authentication.
- Switch a customer's group assignment in the admin backend and verify the price they see updates immediately upon next login or page reload (no caching issue).
- Verify that B2B group prices are consistently displayed on category listing pages, product detail pages, cart, checkout summary, order confirmation page, and confirmation email.
- Place an order as a B2B customer and confirm the invoiced price matches the group-specific price shown during the checkout, not the retail price.
- Verify that a customer who belongs to no group explicitly defaults to standard retail pricing (no accidental B2B discount applied).
- Test that group pricing is applied correctly when a B2B customer uses the Quick Order feature or adds items via API/bulk import.
- Confirm that strikethrough/RRP display correctly shows the original retail price alongside the discounted B2B group price to communicate the saving.
- Attempt to manipulate the group price client-side (e.g., via browser dev tools modifying a hidden field) and verify the server-side revalidates the correct price at order placement.

### Tiered pricing / volume discounts

- Verify that the tiered pricing table is displayed on the product detail page, showing price breakpoints (e.g., 1–9 units: €10.00, 10–49 units: €8.50, 50+ units: €7.00).
- Set quantity to 9 and confirm the unit price matches tier 1; change to 10 and confirm it switches to tier 2 in real time without page reload.
- Verify that the cart subtotal is recalculated correctly when the quantity crosses a tier boundary (e.g., increasing from 9 to 10 units reduces the per-unit price and the total reflects the new tier).
- Test behavior at exact boundary values: enter the exact minimum quantity for each tier and confirm the correct tier price applies.
- Test one unit below a boundary (e.g., 49 units) and confirm the lower tier price is applied, not the higher-tier price prematurely.
- Add two separate line items of the same product (e.g., via different cart-add actions) and verify whether the system combines quantities for tier calculation or treats them independently (document expected behavior).
- Apply a coupon code on top of tiered pricing and confirm the discount stacks correctly (discount applied to the already-tiered price, not the original list price, or vice versa per business rule).
- Verify that tier prices are correctly recalculated if a customer changes their shipping country or customer group mid-session.
- Confirm that the order confirmation email and PDF invoice display the correct tier price per unit and the total, not the standard list price.
- Test tiered pricing with configurable products: changing a product option (e.g., size) that has its own price surcharge should combine correctly with the active tier discount.
- Verify that admin reports and order exports reflect the tiered price actually charged, not the base list price.

### Request for quote

- Navigate to a product page as a B2B customer and verify the "Request for Quote" (RFQ) button is visible and accessible; confirm it is not shown to retail (B2C) customers.
- Submit an RFQ for a single product with a specified quantity and a custom message; verify the submission succeeds and a confirmation message or page is shown.
- Submit an RFQ for multiple products simultaneously (multi-product quote basket) and confirm all items appear in the submitted quote.
- Verify that a confirmation email is sent to the customer immediately after RFQ submission, containing all requested items, quantities, and contact details.
- Verify that the admin/sales team receives a notification email or the RFQ appears in the admin backend quote management area.
- As admin, create a quote response with custom pricing and send it to the customer; verify the customer receives the quote offer email with a link to review it.
- As customer, open the quote offer link, review the proposed prices, and accept the quote; confirm the accepted quote converts into a regular order in the system.
- As customer, reject the quote or request a counter-offer and verify the system handles this state transition without errors.
- Attempt to submit an RFQ without being logged in; verify the system either prompts login or shows an appropriate access-denied message (no silent failure).
- Submit an RFQ with the quantity field left empty or set to zero; verify client-side and server-side validation prevents submission and shows a clear error.
- Verify that an accepted quote locks in the agreed price even if the product's regular price changes before the order is placed.
- Confirm that RFQ items respect inventory status — if a product is out of stock, the system notifies the customer or admin accordingly within the quote workflow.

### Purchase on invoice (payment terms 30/60/90 days)

- Log in as a B2B customer with net payment terms enabled; verify the "Purchase on Invoice" payment method appears in checkout and displays the configured payment term (e.g., "30 days net").
- Log in as a retail (B2C) customer and verify "Purchase on Invoice" with extended terms is not offered as a payment option.
- Complete a purchase using 30-day invoice payment; verify the order confirmation email and PDF invoice clearly state the payment due date (order date + 30 days).
- Verify that the invoice PDF includes all mandatory legal information: company name, VAT ID, invoice number, line items, net total, VAT amount, gross total, and due date.
- Test with a customer whose invoice limit has been reached (e.g., outstanding balance exceeds configured credit limit); verify the payment method is blocked and an appropriate message is displayed.
- Confirm that placing an order on invoice does not immediately deduct from a payment gateway — no PayPal or Stripe charge should occur for this payment method.
- Mark an invoice as paid in the admin backend and verify the order status updates accordingly and the customer's credit limit is restored.
- Test the 60-day and 90-day term variants: verify due dates on invoices are calculated correctly for each term.
- Simulate an overdue invoice scenario in staging: verify that the system blocks new invoice-payment orders for the customer until the overdue balance is resolved.
- Verify that partial payments against an invoice are tracked correctly in the admin area and the remaining balance is displayed accurately.
- Confirm that order confirmation emails for invoice-payment orders do not contain any instructions for card payment or PayPal, only bank transfer details and the due date.

### Approval workflows (order approval by supervisor)

- Log in as a B2B sub-user (employee with spending limit) and place an order exceeding their individual spending limit; verify the order enters an "Awaiting Approval" state instead of being placed immediately.
- Confirm the sub-user sees a clear status message indicating the order is pending approval, and receives a notification email.
- Verify the designated approver (supervisor account) receives an email notification with a link to review the pending order.
- Log in as the approver, navigate to the approval queue, and approve the pending order; verify the order status changes to the normal "Received/Processing" state and inventory is reserved.
- Log in as the approver and reject a pending order with a rejection reason; verify the sub-user is notified by email with the reason and the order is cancelled without charging or reserving inventory.
- Test the case where an order is below the sub-user's spending limit: verify it bypasses the approval queue and is placed directly.
- Test the edge case where an order is exactly at the spending limit boundary: verify it either passes or requires approval consistently per the configured rule.
- Verify that an approved order cannot be approved a second time (idempotency) — the approve action should be disabled or hidden after first approval.
- Verify that a rejected order cannot be re-approved — the workflow should require the sub-user to re-submit a new order.
- Test approval timeout: if the approver takes no action for the configured timeout period, verify the system either auto-cancels the order or escalates it, per business rule, and notifies relevant parties.
- Confirm that the full approval audit trail (submitted by, submitted at, reviewed by, reviewed at, decision, reason) is visible in the admin order detail view.
- Test multi-level approval: if the shop supports a chain (e.g., team lead approves first, then finance), verify each level receives notification only after the previous level approves, and the order does not proceed until all levels are complete.
- Verify that approvers cannot modify order contents (prices, quantities, products) during the approval action — they may only approve or reject.

### Customer numbers / reference numbers on orders

- Verify that a B2B customer can enter their internal purchase order (PO) number or customer reference number during checkout via a dedicated field.
- Confirm the reference number field accepts alphanumeric characters, hyphens, and slashes (common PO number formats) and rejects excessively long strings with a clear error message.
- Verify the entered reference number appears on: the order confirmation page, the order confirmation email, the PDF invoice, and the admin order detail view.
- Test that the reference number field is optional (leaving it blank does not block order completion) unless the customer group is configured to require it.
- In the admin backend, search or filter orders by customer reference number; confirm the search returns the correct matching order(s).
- Export orders to CSV/Excel and verify the customer reference number column is included in the export.
- Verify that the reference number is stored against the correct order and does not bleed over into other orders placed by the same customer in the same session.
- Test with special characters in the reference number (e.g., slashes, parentheses); verify they are stored and displayed without corruption (no HTML encoding issues on display).
- Confirm that the customer can view their own reference number in their order history in the account area.
- If the shop integrates with an ERP system, verify the customer reference number is passed correctly through the API or export so the ERP can match inbound orders to customer POs.

### Net price display

- Log in as a B2B customer with net price display enabled; verify all prices on category pages, product detail pages, search results, and the cart show net (excl. VAT) prices.
- Verify that on the same pages, the VAT amount and gross total are also clearly indicated (e.g., "+ 19% VAT" footnote or a separate line), meeting legal disclosure requirements.
- Log in as a B2C (retail) customer and verify all prices display as gross (incl. VAT) by default.
- Toggle between net and gross display in account settings (if the feature is user-switchable) and verify the display updates across all pages without requiring a full logout/login cycle.
- Verify that the cart summary page shows a clear breakdown: net subtotal, VAT line(s) per applicable tax rate, and gross total.
- Confirm the checkout order summary, confirmation page, confirmation email, and PDF invoice all present the same net/gross breakdown consistently.
- Test with a product that has multiple VAT rates applied (e.g., different rates for different product categories in a mixed cart); verify each rate is broken out separately in the summary.
- Verify that net price display does not affect the actual amount charged — the customer is always charged the gross amount inclusive of VAT.
- Switch a customer's account type from B2B (net) to B2C (gross) in the admin backend and verify the price display mode changes on their next login.
- Confirm that tiered pricing / volume discounts still display correctly in net mode (the tier threshold labels and per-unit net prices are accurate).

### Customer-specific catalogs / assortments

- Log in as a B2B customer assigned to a restricted catalog; verify that products not in their catalog are not visible in search results, category pages, or direct URL access.
- Attempt to directly access the URL of a product excluded from the customer's catalog; verify the shop returns a 404 or access-denied page, not the product details.
- Log in as a different B2B customer with a different catalog assignment; verify they see their specific assortment and not the first customer's catalog.
- Log out and browse as a guest; verify the guest sees only the default public catalog (or is prompted to log in to view the full range, depending on shop configuration).
- Add a new product to a specific customer's catalog via the admin backend; verify the product becomes visible for that customer after the change (cache-aware: test with cleared cache).
- Remove a product from a customer's catalog via admin; verify it disappears from that customer's visible assortment on next page load.
- Verify that the customer cannot add a catalog-excluded product to their cart via API call or direct cart manipulation (server-side validation enforced).
- Confirm that search results are filtered to the customer's catalog — searching for the name or SKU of an excluded product returns no results for that customer.
- Test the "Quick Order by article number" feature with a SKU from outside the customer's catalog; verify it is rejected with a clear error, not silently added.
- Verify that the number of visible products in a category matches the expected count for the customer's catalog, including pagination being accurate to their filtered assortment.

### Quick order by article number

- Navigate to the Quick Order page as a logged-in B2B customer and verify the form is accessible and clearly labeled.
- Enter a valid article number (SKU) and a valid quantity; verify the product name, price, and availability are looked up and displayed inline before adding to cart.
- Enter an invalid or non-existent article number; verify a clear inline error message is shown ("Article not found") without disrupting other rows.
- Enter an article number for a product not in the customer's catalog (if catalog restrictions apply); verify it is rejected with an appropriate message.
- Enter a quantity of zero or a negative number; verify client-side and server-side validation prevents this and shows a clear error.
- Enter a quantity exceeding the product's maximum order quantity; verify the system caps or rejects the quantity with an explanatory message.
- Add 10+ rows simultaneously in the quick order form (bulk entry); verify all rows are processed correctly and all valid items are added to the cart in a single action.
- Upload a CSV file containing article numbers and quantities (if CSV upload is supported in the quick order feature); verify all valid rows are parsed and added to cart, and a report is shown for invalid rows.
- Verify that the quick order process correctly applies customer group pricing and tiered pricing based on the entered quantity, not the default list price.
- After adding items via quick order, navigate to the cart and verify all items, quantities, prices, and totals are exactly as expected.
- Test with a partial row (article number entered but quantity field blank, or vice versa); verify the system prevents submission and highlights the incomplete field.
- Verify performance: submitting a quick order with 50 rows completes within an acceptable time (e.g., under 5 seconds) without timeout or UI freeze.
