# 40. Notification System (beyond email)

### Push notifications (browser/web push)

- Navigate to a page that triggers the push permission prompt (e.g., after login or on account settings page) — verify the browser native permission dialog appears
- Click "Allow" on the permission prompt — verify the subscription is stored server-side and a confirmation notification is delivered within 30 seconds
- Click "Block" on the permission prompt — verify no push messages are ever sent to that browser, and the UI reflects the blocked state without error
- Dismiss the permission prompt without choosing — verify the prompt does not reappear on the same page reload but may reappear on a configurable interval (e.g., next session)
- After granting permission, trigger a qualifying event (e.g., order status change, price drop) — verify the push notification arrives with correct title, body text, and icon
- Click the push notification when it arrives — verify it opens the correct deep-link URL in the browser (e.g., the relevant order detail page)
- Revoke permission via browser settings — verify no further pushes are delivered and the system does not log errors on unsubscribe
- Subscribe on two different browsers/devices with the same account — verify push notifications are delivered to both subscriptions independently
- Simulate a lapsed push subscription (expired endpoint) — verify the server detects the 410 Gone response, removes the stale subscription, and does not retry indefinitely
- Open notification preferences in the customer account — verify a "Browser push notifications" toggle is visible, its current state matches the browser permission state, and toggling it off stops future pushes without revoking the browser permission
- Opt out of push via account preferences — re-grant browser permission — verify notifications are still suppressed until the customer actively re-enables them in preferences
- Verify push notification payload does not contain sensitive personal data (PII, order details beyond a generic summary)

### SMS notifications (shipping, delivery)

- Place an order and provide a mobile phone number during checkout — verify an SMS is sent when the order status changes to "Shipped" containing the carrier name and a tracking link
- Verify the SMS tracking link is functional and leads to the correct carrier tracking page
- Place an order without providing a phone number — verify no SMS is attempted and no error is logged
- Add a phone number in account settings after an order has already been placed — verify subsequent status change SMSs use the updated number
- Update the phone number in account settings to a new number — verify the next SMS goes to the new number, not the old one
- Enter an invalid phone number format during checkout — verify inline validation rejects it before order submission
- Opt out of SMS notifications in account preferences — trigger a shipping event — verify no SMS is sent
- Opt back in to SMS after opting out — trigger a delivery event — verify SMS is now sent again
- Verify SMS sender ID or short code is identifiable as the shop (not generic)
- Verify the SMS body text is concise, contains the order number, and includes an unsubscribe instruction ("Reply STOP to unsubscribe") where legally required
- Reply "STOP" to an SMS — verify the phone number is immediately added to the SMS opt-out list and no further SMS messages are sent
- Reply "START" or "YES" after a STOP — verify the number is re-subscribed and SMS notifications resume
- Verify SMS opt-out status persists across sessions and is visible in account notification preferences

### In-app notifications in customer account

- Log in and navigate to the customer account dashboard — verify an unread notification badge or indicator is visible when there are new notifications
- Verify the notification inbox/panel lists notifications in reverse chronological order (newest first)
- Click an individual notification — verify it is marked as read, the unread counter decreases by one, and the notification links to the relevant page
- Click "Mark all as read" — verify all notifications are marked read and the counter resets to zero
- Verify notification persistence: log out, log back in — verify read/unread states are preserved server-side, not just in session
- Trigger multiple notification types (order update, price drop, back-in-stock) — verify each appears in the inbox with a distinct icon or label indicating its type
- Delete an individual notification — verify it is removed from the list and does not reappear on refresh
- Verify the notification panel is accessible via keyboard (Tab to open, Arrow keys to navigate, Enter to activate, Escape to close)
- Verify the notification panel on mobile renders correctly, is scrollable, and individual items have adequate touch targets
- Log in on a second browser tab — trigger an event that generates a notification — verify the unread badge updates in the first tab without a full page reload (real-time or on next page navigation)
- Simulate a large number of notifications (50+) — verify pagination or "Load more" works correctly and performance does not degrade
- Verify that notification content does not render raw HTML or scripts (XSS protection)

### Webhook reliability (retry on failure)

- Configure a webhook endpoint and take it offline — trigger an event — verify the system queues the webhook and logs a failed delivery attempt
- Bring the endpoint back online — verify the system automatically retries the failed webhook and delivers it successfully
- Verify the retry schedule follows exponential backoff (e.g., 1 min, 5 min, 30 min, 2 h, 24 h) and does not hammer a failing endpoint
- Simulate an endpoint that returns HTTP 500 intermittently — verify only genuinely failed deliveries are retried, not successful ones
- Simulate an endpoint that returns HTTP 200 but processes the payload incorrectly — verify the system considers it delivered (acknowledgment-based, not content-based)
- Verify a maximum retry limit exists (e.g., 5 attempts over 24 hours) — after exhausting retries, verify the webhook is marked as permanently failed and an admin alert is triggered
- Verify failed webhook deliveries are visible in the admin backend with status, timestamp, HTTP response code, and response body
- Manually replay a failed webhook from the admin panel — verify it is re-sent immediately and the status updates accordingly
- Verify webhook payloads include a signature header (HMAC) — verify the receiving endpoint can validate authenticity
- Verify webhook delivery order is preserved for sequential events on the same resource (e.g., order created before order shipped)
- Simulate duplicate delivery (network retry causes same event sent twice) — verify the payload includes an idempotency key so the receiver can deduplicate

### Notification preferences (opt-in/opt-out per channel)

- Navigate to notification preferences in the customer account — verify each channel (push, SMS, in-app, WhatsApp) has its own independent toggle
- Disable push notifications via preferences toggle — verify push messages stop immediately without requiring browser permission revocation
- Disable SMS notifications — verify the opt-out is stored in the database and persists after logout/login
- Enable a previously disabled channel — verify notifications for that channel resume with the next qualifying event
- Verify that notification type granularity is available (e.g., "Order updates", "Price alerts", "Back-in-stock", "Marketing") — toggling "Order updates" off should not suppress price drop alerts
- Opt out of all channels — trigger multiple events — verify zero notifications are delivered across all channels
- Verify that transactional-critical notifications (e.g., order confirmation, payment failure) cannot be fully opted out of (they are mandatory by design) and the UI explains this clearly
- Create a new account — verify default preference states are correctly set (e.g., transactional on, marketing off) in compliance with GDPR opt-in requirements
- Verify preference changes take effect immediately, not just at the next scheduled sync
- Verify preference state is consistent across all surfaces: account settings page, preference center link in emails, and in-app notification settings
- Attempt to unsubscribe via a preference-center link in a notification message — verify it targets only that channel/type without unsubscribing the user from everything else
- Verify the preference page is accessible without login via a one-click unsubscribe link (as required by email/SMS regulations) — verify the token in the link is single-use or time-limited

### Price change notification (wishlist)

- Add a product to the wishlist and opt in to price change notifications — reduce the product price in the admin — verify a notification is sent within the defined interval
- Verify the notification clearly states the old price, the new price, and includes a direct link to the product page
- Reduce the price and then raise it back before the notification job runs — verify no spurious "price dropped" notification is sent for a net-zero change
- Verify notifications are only sent for price decreases, not increases (unless the feature explicitly covers increases)
- Add a product variant to the wishlist — change the price on that specific variant only — verify the notification is sent for the correct variant and references it specifically
- Remove a product from the wishlist before the notification is dispatched — verify no notification is sent
- Opt out of price change notifications for a specific wishlist item — change the price — verify no notification is sent for that item while other items still receive notifications
- Verify duplicate notifications are suppressed: if a price drops multiple times in one day, only one notification is sent per day (or per configured frequency)
- Verify price change notifications respect the customer's preferred channel (in-app, push, email) as set in notification preferences
- Simulate a product being discontinued (deleted from catalog) while on a wishlist — verify no broken-link notifications are sent

### Back-in-stock notification

- Navigate to a sold-out product page — verify a "Notify me when available" button or email input field is present
- Submit the back-in-stock request without being logged in — verify the system accepts an email address, sends a confirmation email, and stores the request
- Submit the back-in-stock request while logged in — verify the stored email matches the account email and the customer does not need to re-enter it
- Attempt to submit the same email for the same product twice — verify a duplicate message is shown and no duplicate subscription is created
- Restock the product in the admin — verify back-in-stock notifications are sent to all subscribed customers within the defined processing window
- Verify the notification contains the product name, the link to the product page, and a clear call to action ("Shop now")
- Verify that once a back-in-stock notification has been sent to a customer for a product, they are automatically unsubscribed from that specific alert (one-time trigger)
- If stock runs out again after restocking, verify customers can re-subscribe via the same flow
- Restock only one variant (e.g., size M) while other variants remain out of stock — verify only customers who subscribed to size M are notified, not all subscribers for the product
- Verify there is an unsubscribe mechanism in the notification itself so customers can cancel the alert without needing to log in
- Verify the opt-in for back-in-stock notifications is GDPR-compliant: purpose-limited consent, no use of the email for marketing without separate consent
- Simulate a large restock event with hundreds of subscribers — verify notifications are sent via a queue/batch job and do not cause a synchronous timeout or email service rate-limit breach

### Order status updates via WhatsApp/Telegram

- Connect a WhatsApp or Telegram account in notification preferences — verify an opt-in confirmation message is sent to the connected account
- Place an order — verify an order confirmation message is delivered via the connected channel with order number and summary
- Change the order status to "Shipped" in the admin — verify a shipping notification with tracking link is delivered via WhatsApp/Telegram
- Change the order status to "Delivered" — verify a delivery confirmation message is sent
- Disconnect the WhatsApp/Telegram integration in account settings — verify no further messages are sent on subsequent status changes
- Verify the opt-out flow via the messaging platform itself: send "STOP" or use the channel's built-in block feature — verify the system detects the opt-out (via webhook from the messaging platform) and marks the customer as unsubscribed
- Attempt to re-enable WhatsApp/Telegram notifications after opting out — verify the customer must explicitly confirm re-opt-in via the messaging platform (e.g., send "START") before messages resume
- Verify message templates comply with WhatsApp Business Policy (pre-approved templates for transactional messages, no unsolicited marketing content)
- Verify messages are sent in the customer's language as set in their account preferences
- Simulate a failed delivery (invalid/deregistered number) — verify the error is logged, no infinite retry loop occurs, and the customer is notified via a fallback channel (e.g., in-app notification or email)
- Verify message content does not expose sensitive data beyond what is necessary (e.g., no full payment details, only partial order information)
- Test concurrent order status updates for multiple orders from the same customer — verify all messages are delivered correctly without merging or dropping any
