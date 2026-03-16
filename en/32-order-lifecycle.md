# 32. Order Status Lifecycle / Order Management

### Complete status workflow: Received > Paid > Processing > Shipped > Delivered

- Place a new order via checkout and verify the initial status is set to **Received** immediately after the order is confirmed.
- Simulate a successful payment webhook/callback from the payment provider and confirm the status transitions from **Received** to **Paid** automatically without manual intervention.
- Verify that a **payment confirmation email** is triggered to the customer exactly when the status changes to **Paid** (not before, not after).
- Manually move an order in the admin backend from **Paid** to **Processing** and confirm the status change is saved and reflected in both the admin view and the customer's order detail page.
- Verify that a **"We are preparing your order"** or equivalent notification email/SMS is sent when the status transitions to **Processing**.
- Enter a valid tracking number in the admin backend, change the order status to **Shipped**, and confirm the status persists correctly in the database.
- Verify that a **shipping confirmation email** is sent to the customer when the status changes to **Shipped**, and that the email contains the correct tracking number and a working tracking link.
- Simulate a delivery confirmation event (e.g., from the shipping provider webhook or manual admin update) and verify the order status changes to **Delivered**.
- Verify that a **delivery confirmation email** (and/or review request) is triggered when status reaches **Delivered**.
- Attempt to move an order status **backwards** (e.g., Shipped → Processing) and verify the system either prevents it or requires an explicit override with an audit log entry.
- Confirm that each status transition is recorded in an **order history/audit log** with timestamp and the user (admin or system) who made the change.
- Verify that invalid status transitions (e.g., Received → Delivered, skipping all intermediate steps) are blocked by the system unless explicitly permitted by business rules.
- Check that the customer-facing **order status page** and account dashboard reflect each status change in real time (or within an acceptable delay) without requiring the customer to log out and back in.
- Verify that the **admin order list** correctly filters and displays orders by each status (Received, Paid, Processing, Shipped, Delivered) and that counts match the actual database records.
- Confirm that a **failed payment** (e.g., declined card) leaves the order in **Received** (or a dedicated **Payment Failed** status) and does not advance it to **Paid**.

---

### Partial shipment (multiple packages for one order)

- Place a multi-item order and ship only a subset of items from the admin backend; verify the system creates a **partial shipment record** linked to the original order.
- Confirm the order status updates to a **Partially Shipped** state (or equivalent) rather than **Shipped** when only some items have been dispatched.
- Verify that the customer receives a **partial shipping notification email** listing only the items included in the current shipment with the correct tracking number.
- Check that items remaining in the order retain their **pending/processing status** and are still displayed as awaiting shipment in the customer's order detail view.
- Create a second shipment for the remaining items and verify the order status transitions to **Fully Shipped** or **Shipped** once all items are dispatched.
- Confirm that a **second shipping confirmation email** is triggered for the second package, distinct from the first, with its own tracking number.
- Verify that tracking numbers for both packages are **individually accessible** in the customer's order detail page and in the customer account area.
- Test the scenario where one package is delivered but the other is still in transit; confirm the order status remains **Partially Delivered** (or equivalent) and does not prematurely advance to **Delivered**.
- Confirm the order only reaches **Delivered** status after all packages have been confirmed as delivered.
- Verify that the **invoice and delivery note** correctly reflect which items are in which shipment.
- Test partial shipment with items that have **different delivery times** (e.g., in-stock item ships immediately, back-ordered item ships later) and confirm each triggers the correct notification.

---

### Backorder / reorder on supply shortage

- Add a product to an order that is marked as out of stock at the time of order placement; verify the system flags the order line as **backordered** and communicates this to the customer during checkout.
- Confirm an **automated notification email** is sent to the customer when an item goes into backorder status, including the expected restocking/shipping date if available.
- When stock is replenished, verify the system automatically or via admin action **releases the backorder**, updates the order status, and sends a **"Your backordered item is now being prepared"** notification to the customer.
- Verify the backordered item's expected delivery date is visible on both the admin order detail page and the customer-facing order detail page.
- Test what happens when a customer places an order for the last unit of an item and a concurrent order consumes that unit first (race condition); confirm only one order succeeds and the other is correctly placed in backorder or an out-of-stock error is returned.
- Confirm that the overall order status correctly distinguishes between items that are **shipped** vs. items that are **backordered** — the order should not show as fully shipped while items are still pending.
- Verify that the customer can **cancel the backordered item** (if permitted) without affecting already-shipped items in the same order.
- Check that a **reorder action** (ordering the same items again when out of stock) creates a new order with the same items rather than modifying the existing one, and that the customer is clearly informed.
- Test admin notifications or internal alerts triggered when an item enters backorder status so the warehouse/purchasing team can act.

---

### Order status page for customers (without login, via link)

- Verify that after order completion, the customer receives a **status page URL** (e.g., with a unique token) in the confirmation email that allows them to view their order status without logging in.
- Access the order status link while **not logged in** and confirm the page displays the current order status, order summary (items, total), and estimated delivery — but no sensitive account data.
- Verify the unique token in the URL is **not guessable** (sufficiently random, not based on order ID alone) to prevent unauthorized access to other customers' orders.
- Test that the status page updates correctly when the order status changes (e.g., from Processing to Shipped) and the updated tracking information appears.
- Verify that the **tracking number and carrier link** appear on the status page once the order is shipped.
- Attempt to access an order status page with a **tampered or invalid token** and confirm a proper 404 or access-denied response is returned (no information leakage).
- Check that accessing the order status page while **logged in as a different customer** does not reveal another customer's order details.
- Verify that if the customer later logs in, the **guest order is associated** with their account and accessible in their order history.
- Test the status page on **mobile devices** to confirm it is fully responsive and usable.
- Verify the status page displays correctly in all major browsers (Chrome, Firefox, Safari, Edge).
- Confirm there is a **contact/help link** on the status page (e.g., link to support or contact form) for customers needing assistance.

---

### Address change on pending order

- Place an order and, before it moves to **Processing**, attempt to change the **shipping address** via the customer account; verify the change is accepted and reflected in the order record.
- Verify that an **address change confirmation email** is sent to the customer when the shipping address is successfully updated on a pending order.
- Attempt to change the shipping address once the order status is **Shipped** and confirm the system **blocks** the change with an appropriate error message (the package is already in transit).
- Test changing the address on a **Paid** order (awaiting processing) and confirm the system applies the change correctly without duplicating the order or re-charging the customer.
- Verify that an admin can change the shipping address on any order status and that the change is recorded in the **order audit log**.
- Test changing the address to a **different country** on a pending order and confirm the system recalculates shipping costs and notifies the customer if additional charges apply.
- Confirm that changing the address does **not reset** the order status or trigger unintended re-payment workflows.
- Verify that if the new address falls outside the supported **shipping zones**, an appropriate error is shown and the change is not saved.

---

### Prevention of duplicate orders (double-submit protection)

- Double-click the **"Place Order"** button rapidly and verify only **one order** is created in the database, not two.
- Submit the checkout form via two browser tabs simultaneously with the same session/cart and confirm the system creates only one order.
- Disable JavaScript and attempt a form double-submit (native browser form re-submission) and verify server-side deduplication prevents a second order.
- Verify that after the first submission, the **"Place Order" button is disabled** or replaced with a loading indicator, preventing a second click.
- Confirm that re-loading the order confirmation page (browser refresh) does **not** create a second order (POST/Redirect/GET pattern or equivalent).
- Use browser developer tools to replay the checkout POST request manually and verify the server rejects the duplicate request with an appropriate error.
- Verify that the **cart is cleared** immediately after a successful order placement so that navigating back to the checkout does not allow re-submitting the same cart.
- Check that a **unique idempotency key** or equivalent mechanism is used for payment API calls so that retried payment requests do not result in double charges.
- Simulate a **network timeout** after the order is submitted (server processes the order but the response never reaches the client) and verify the customer is shown an appropriate message and is not charged twice if they retry.
- Verify that the **payment provider's side** reflects only one charge for each order, even in the scenarios above.

---

### Merge multiple orders

- In the admin backend, select two separate orders from the same customer and trigger the **merge** function; verify the merged order contains all items from both original orders.
- Confirm the total price on the merged order is the **sum of both original orders** (or recalculated correctly if shipping is consolidated).
- Verify that after merging, the two original orders are **marked as merged/cancelled** in the system with a reference to the new combined order, and are no longer shown as active.
- Confirm the customer receives a **notification email** informing them that their orders have been merged, with the new combined order number.
- Verify that any **payment** already made for the original orders is correctly reflected on the merged order — no re-charging occurs.
- Test merging orders that have **different shipping addresses** and confirm the admin is prompted to select which address to use for the merged order.
- Verify that **coupons/discounts** applied to the original orders are handled correctly on the merged order (not doubled, not lost).
- Confirm that merging is **blocked** if either order has already been shipped or is in a status that cannot be merged (e.g., Delivered, Cancelled).
- Verify the merge action is recorded in the **audit log** with the admin user, timestamp, and the original order IDs.
- Check that the customer's **order history** in their account shows the merged order correctly and does not show orphaned references to the original orders.

---

### Reorder from order history

- Navigate to a completed order in the customer account and click the **"Reorder"** button; verify all items from the original order are added to the cart correctly (correct variants, quantities).
- Confirm that the reorder action uses **current prices**, not the prices from the original order, and that the customer sees the updated prices in the cart.
- If a product from the original order is **no longer available** (discontinued or out of stock), verify the system skips that item (or shows a warning) rather than throwing an error, and still adds the available items to the cart.
- If a **product variant** (e.g., a specific color/size) from the original order no longer exists, verify the system handles this gracefully (warning message, suggestion of alternative variant).
- Verify that reordering does **not** apply the original coupon code automatically — the customer must enter a valid coupon code manually for the new order.
- Test the reorder functionality while **not logged in** (guest) and confirm the system prompts for login or shows that the feature requires an account.
- After triggering a reorder, verify the customer is **redirected to the cart** (or a cart preview) where they can review and adjust the items before proceeding to checkout.
- Confirm that reordering a large order (e.g., 10+ items) does not cause a **performance issue** or timeout, and all items are added to the cart.
- Verify that the **cart item count** in the header updates correctly after the reorder action adds items to the cart.
- If the customer already has items in the cart when reordering, confirm the reordered items are **added to** the existing cart (not replacing it), and that the customer is made aware of the combined cart contents.
