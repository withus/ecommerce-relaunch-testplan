# 43. Loyalty Program & Subscriptions

### Points earned on purchase correctly calculated

- Place a standard order and verify that the exact expected number of points is credited (e.g., 1 point per €1 spent) after the order transitions to "paid" or "completed" status — not at placement.
- Place an order using a percentage-off coupon and verify points are calculated on the net amount paid, not the original price.
- Place an order using a fixed-amount coupon that reduces the total to €0 and verify zero points are awarded (or minimum defined threshold applied).
- Add a product with a bonus-points multiplier (e.g., "2x points this week") to cart and confirm the doubled points appear on the post-order points statement.
- Place an order containing multiple items with different points multipliers (e.g., item A = 1x, item B = 3x) and verify each line item's contribution is individually correct and the total is summed accurately.
- Place an order that spans a points-tier bonus threshold (e.g., spend €100 and earn an extra 50 bonus points) and confirm the threshold bonus is added exactly once, not per item.
- Complete checkout with a gift card / store credit payment and verify that points are awarded only on the cash portion, not on the gift-card-covered portion.
- Place an order with a shipping cost and verify whether shipping costs are included or excluded from points calculation per configured rules.
- Complete an order and immediately check the points balance before and after — the delta must match the calculated award with no rounding discrepancy greater than the configured rounding rule (floor/round/ceil).
- Return an item from an order and verify that the awarded points are deducted proportionally and the updated balance is reflected in the account.
- Cancel an entire order that has already been credited with points and confirm all points are reversed, leaving no net change from that order.
- Place an order as a logged-in customer who is simultaneously enrolled in a B2B customer group with a different points rate and confirm the correct rate is applied.
- Place two orders rapidly in close succession and verify both earn points independently without a race condition that credits one order twice or skips the other.
- Verify that tax/VAT portion of the order total is handled consistently (included or excluded) according to configuration, and matches what is displayed to the customer.
- Place an order worth a fractional point amount (e.g., €1.50 at 1 point/€1) and confirm the fractional handling (floor, round, or accumulate) matches the documented rule.

### Points redeemable at checkout

- At checkout, enter a valid points redemption amount and verify the order total decreases by exactly the configured conversion rate (e.g., 100 points = €1.00).
- Attempt to redeem more points than the current balance and verify an appropriate error message is shown and the field is reset or capped.
- Attempt to redeem a points amount that would reduce the order total below the configured minimum order value and confirm an error is returned with a clear message.
- Redeem points on an order that also uses a coupon code and verify both discounts stack correctly, or are blocked per business rules, with no silent calculation error.
- Redeem the exact total points balance and verify the displayed balance reaches zero and the checkout proceeds without error.
- Remove points redemption after adding it and verify the order total reverts to the original amount with no residual discount.
- Attempt to redeem points while logged out / as a guest and confirm the redemption UI is not shown or shows an appropriate "login required" message.
- Apply points redemption, then change the cart (add/remove item), and verify the redemption amount is re-validated and adjusted or cleared if it now exceeds the new balance or minimum.
- Complete checkout with partial points redemption and verify the remaining balance is deducted from the account, not the full balance.
- After a successful points redemption checkout, verify the points ledger in the customer account shows the deduction entry with date and order reference.
- Attempt to redeem points on a product category excluded from redemption (if configured) and verify a clear error is presented.
- Verify that the points redemption input enforces whole-number or configured-increment constraints (e.g., only multiples of 10 points) and rejects invalid values.
- Apply points redemption, proceed to payment, let the session time out, return and log back in — confirm the redemption state is not silently held open against the balance.

### Points balance visible in customer account

- Log in and navigate to the loyalty / rewards section of the customer account and confirm the current points balance is prominently displayed.
- Verify the balance shown on the account overview matches the balance shown in the dedicated points/rewards detail page.
- Confirm the points history / transaction log lists each earning event with date, order reference, and points amount.
- Confirm the points history lists each redemption event with date, order reference, and points deducted.
- Confirm cancelled/returned order point reversals appear as separate line items in the history, not as edits to the original entry.
- Log in on a second device simultaneously and verify both sessions display the same balance (no stale cache from one device showing outdated data).
- Complete a purchase and without refreshing the account page, navigate back to the balance — confirm it updates (or shows a refresh prompt) rather than displaying the pre-purchase balance indefinitely.
- Verify that a customer with zero points sees a "0 points" balance and not a blank field or an error.
- Verify that the points balance page loads within acceptable performance thresholds even when the transaction history contains hundreds of entries (pagination or lazy load).
- Check that the points balance and history are not accessible when logged out — direct URL access should redirect to login.
- Verify screen reader / accessibility: the points balance figure has an appropriate label and is announced correctly by assistive technology.

### Points expiry/decay

- Set up an account with points earned more than X months ago (per configuration) and verify those points are marked as expiring soon in the account UI with a specific expiry date.
- Trigger the scheduled points expiry job (or wait for the cron) and verify that expired points are removed from the balance and a corresponding expiry entry appears in the transaction log.
- Verify that a customer receives an email notification (if configured) before their points expire, with the correct expiry date and balance amount.
- Earn new points shortly before an expiry date and verify that only the old batch expires on schedule while the new batch retains its own expiry countdown.
- Redeem points just before expiry and verify that the redemption consumes the oldest/soonest-to-expire points first (FIFO expiry), or per the configured rule.
- Verify that points earned during a promotional period with a different expiry window expire at the correct date, not the default window.
- After points expire, confirm the customer's redeemable balance reflects the deduction, and attempting to check out with a previously valid redemption amount now returns an insufficient-balance error.
- Verify that points earned on a returned/refunded order are not extended or "re-earned" if the refund happens after the expiry warning date.
- Simulate a points decay model (e.g., 10% of balance expires monthly) and verify the decay calculation is applied on the correct schedule and rounds correctly.
- Check that the expiry date displayed in the account UI exactly matches the date the backend job will process the expiry (no off-by-one day due to timezone differences).
- Verify that expired points cannot be reinstated by customer service UI unless an explicit override action is taken (no accidental resurrection via cart re-open).

### Subscription orders (recurring delivery)

- Create a new subscription for a product with a monthly interval and verify the first order is placed immediately (or on the configured start date) with correct items, price, and delivery address.
- Verify the second recurring order is automatically placed at exactly T+30 days (or configured interval) and generates an order confirmation email.
- Attempt to subscribe to a product that is out of stock and verify a clear error or waitlist message is shown rather than silently creating a broken subscription.
- Create a subscription and then update the delivery address — verify the change takes effect on the next scheduled order, not retroactively on already-shipped orders.
- Create a subscription for a product with variants (e.g., size/color) and verify the correct variant is re-ordered on each cycle without reverting to a default.
- Verify that a subscription order respects any active promotions/discounts configured specifically for subscribers (e.g., 10% subscriber discount) on every cycle, not just the first.
- Create a subscription and let a renewal attempt fail due to an expired payment method — verify the customer receives a payment failure email with a link to update payment details.
- After a failed subscription payment, update the payment method and verify the system retries the charge and creates the order successfully without creating a duplicate.
- Verify that failed subscription retries follow the configured retry schedule (e.g., retry after 3 days, then 7 days) and the subscription is cancelled after the maximum retry count is exceeded.
- Verify that a subscription-generated order appears in the customer's order history with a visual indicator (e.g., "Subscription order") distinguishing it from manual orders.
- Create a subscription and verify the customer receives a reminder email a configured number of days before the next renewal (if this feature is enabled).
- Verify that subscription orders correctly inherit the shipping method selected at signup and do not default to the cheapest option on subsequent cycles.

### Subscription management (change interval, pause, cancel)

- Navigate to the subscription management page and verify all active subscriptions are listed with their product, interval, next order date, and status.
- Change a subscription interval from monthly to bi-monthly and verify the next scheduled order date updates accordingly and the change is reflected in the confirmation email.
- Change a subscription interval to a shorter cycle (e.g., weekly) and verify the next order date recalculates from the current date at the new interval, not from the original anchor date.
- Pause a subscription and verify no order is generated on the next scheduled date while the subscription remains paused.
- Resume a paused subscription and verify the next order date is correctly recalculated from the resume date (not the original paused date), and an order is created on the next cycle.
- Attempt to pause a subscription that has a renewal due within 24 hours and verify whether the pause takes effect immediately (cancelling the imminent charge) or from the cycle after — behavior should match documented rules.
- Cancel a subscription and verify no further orders are generated, no further charges are attempted, and the subscription status updates to "Cancelled" in the account UI.
- Cancel a subscription after an order has already been placed for the current cycle and verify the cancellation does not affect the already-placed order (it remains valid).
- Attempt to reactivate a cancelled subscription and verify either a clear "create new subscription" flow is triggered, or reactivation restores the original product and settings.
- Change the quantity within a subscription (e.g., from 1 unit to 3 units) and verify the updated quantity and updated price are reflected on the next recurring order.
- Change the delivery address within an active subscription and verify the change confirmation email is sent and the new address appears on the next order.
- Log in as a customer with no active subscriptions and verify the subscription management page shows an appropriate empty state rather than an error.
- Verify that all subscription management actions (pause, cancel, change interval) are reflected in an admin backend view for customer service agents.

### Recurring payment for subscriptions

- Verify that the payment method used for the initial subscription is tokenized / stored for future recurring charges without storing raw card data.
- Trigger a recurring charge and verify the charge amount exactly matches the current product price plus shipping for the subscription — no rounding discrepancy.
- Trigger a recurring charge on a subscription for a product whose price has changed since signup and verify whether the new price or the locked-in signup price is charged per the configured rule.
- Verify that the recurring charge uses Strong Customer Authentication (SCA) exemption (MIT — Merchant Initiated Transaction) correctly and does not require customer interaction for each cycle.
- Simulate a card decline (insufficient funds) on a recurring charge and verify the retry logic fires on the correct schedule without charging the customer multiple times on the same attempt.
- Simulate a card being reported stolen/blocked between subscription cycles and verify the system processes the decline gracefully, notifies the customer, and does not create a fulfilled order without payment.
- Verify that a successful recurring charge generates a receipt/invoice emailed to the customer with the correct line items, subscription reference, and billing date.
- Verify that a recurring payment appears on the customer's bank statement with a recognizable merchant descriptor (not a generic or blank descriptor).
- Verify that refunding a subscription order (partial or full) does not cancel the subscription itself unless that is the explicit customer action.
- Test the scenario where a customer updates their payment method between retry attempts — verify the next retry uses the new payment method.
- Verify that the payment gateway webhook for a successful subscription charge is processed idempotently: receiving the same webhook twice does not create two orders.
- Check PCI-DSS compliance: confirm that no raw card numbers are logged in application logs, error logs, or database tables for subscription payments.

### Referral program (invitation link tracking)

- Generate a referral link from the account area and verify the link contains a unique, non-guessable referral code tied to the referrer's account.
- Share the referral link and complete a registration via that link — verify the new account is correctly attributed to the referrer in the admin backend.
- Complete a first purchase as the referred customer and verify the referral reward (points, discount, or credit) is credited to the referrer's account.
- Verify the referral reward is only triggered on the referred customer's first qualifying purchase, not on subsequent orders.
- Attempt to use your own referral link to create a second account and verify the system detects self-referral (same email domain, same IP, same device fingerprint) and blocks or flags the reward.
- Attempt to use an expired or deactivated referral code and verify an appropriate error message is shown rather than silently attributing the referral with no reward.
- Verify that the referred customer also receives their new-customer incentive (if configured) and that both rewards (referrer + referee) are granted without duplication errors.
- Share the referral link with multiple users who all register and purchase — verify the referrer receives one reward credit per valid referred purchase, not a single credit regardless of count.
- Verify there is a maximum cap on referral rewards per account (if configured) and that no rewards are issued beyond the cap.
- Check that referral attribution is tracked via the link even if the referred user clicks the link, leaves, returns to the site directly later, and then registers (cookie-window attribution).
- Verify the referral program UI in the customer account shows: the referral link, number of successful referrals, pending referrals, and total rewards earned.
- Attempt to manipulate the referral code in the URL (e.g., substituting another user's code) and verify that attribution correctly follows the code, and no unintended account is credited due to parameter injection.
