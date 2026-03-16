# 37. Session Management & Cart Persistence

### Cart persists after closing browser (logged in)

- Log in, add 3 products (different quantities) to the cart, then close the browser tab completely.
- Reopen the browser and navigate directly to the shop URL — verify all cart items, quantities, and configured options (e.g., selected variants) are restored exactly as left.
- Log in on a fresh browser session (cleared history, but not cookies) — verify the cart is populated from the server-side session, not from local storage alone.
- Add products to cart, log in, close the browser, then open it again after 24 hours — verify cart is still present and no items have silently dropped.
- Add a product to the cart, close the browser, reopen, and verify that the cart total and any applied coupon codes are also restored correctly.
- Add a product to the cart while logged in, then use the browser's "Restore previous session" feature — verify the cart page reflects the correct state without duplicate entries.
- Log in on a private/incognito window, add products, close the window — reopen a new incognito window and log in again; verify the cart persists (server-side, not cookie-based storage).
- Log out, close the browser, log back in — verify the cart still contains the previously added items (cart is tied to the user account, not just the session cookie).
- Add 10+ items to the cart, close the browser, and reopen — verify all items are present and no quantity caps or truncation occurred during persistence.
- Verify the browser console shows no errors during cart restoration on page load.

### Cart merging: guest cart + login cart combined

- Add 2 products to the cart as a guest, then log in to an account that already has 1 different product in its saved cart — verify all 3 products appear in the merged cart.
- Add the same product to both the guest cart and the logged-in account cart (same SKU, same variant) — verify the quantities are summed correctly (e.g., guest: qty 2 + account: qty 1 = qty 3), not duplicated as two line items.
- Add the same product with different variants as a guest and in the account cart (e.g., guest: size M, account: size L) — verify both appear as separate line items in the merged cart.
- Add a product as a guest, then log in during checkout — verify the merge happens seamlessly without redirecting the user away from checkout.
- Add products as a guest, then register a new account during checkout — verify the guest cart is transferred to the new account cart, not lost.
- Add a product as a guest, log in, then immediately log out — verify the merged cart is no longer accessible in the guest session (cart belongs to the account).
- Add a product as a guest using a coupon code, then log in — verify the coupon code also carries over into the merged cart.
- Add a product as a guest that was deleted/removed from the catalog, then log in — verify the system gracefully handles the unavailable item (shows an error or removes it) without crashing the merge.
- Add products as a guest on mobile, then log in on the same device — verify the cart merge works correctly in the mobile view.
- Add products as a guest, log in, and verify analytics/tracking events (add_to_cart, view_cart) are not double-fired due to the merge.

### Session timeout behavior (checkout form data loss?)

- Begin filling out the checkout address form, then leave the browser idle for the duration of the session timeout (check the configured timeout value, typically 15–30 minutes). After timeout, attempt to submit the form — verify the user is shown a meaningful error or redirected to login without losing all entered data.
- Fill out the full checkout form (address, shipping, payment selection), then let the session expire, then click "Place Order" — verify the system does not create a ghost/empty order, and the user is informed of the session expiry.
- Verify the session expiry time is set to a sensible value for a checkout flow (should not be shorter than a realistic checkout duration, e.g., at least 30 minutes).
- Let the session expire on the cart page — verify the cart page still displays the cart contents (ideally recovered from cookie or DB) and prompts for re-login rather than showing an empty cart.
- During checkout, open a new tab and deliberately trigger session invalidation (e.g., log out in the new tab), then return to the checkout tab and attempt to proceed — verify the checkout tab detects the invalid session and handles it gracefully.
- After session timeout during checkout, log back in — verify the user is redirected back to the checkout page with as much data pre-filled as possible, not sent to the homepage.
- Verify no sensitive payment data (card numbers, CVV) is ever stored in form recovery/pre-fill after a session timeout.
- Test session timeout on mobile with the browser backgrounded for an extended period — verify behavior is consistent with the desktop scenario.
- Verify the session timeout warning (if implemented) appears with enough time for the user to act (e.g., a "Your session expires in 5 minutes" notification).
- After session timeout, verify that clicking the browser back button does not reveal any previously entered personal or payment data from cache.

### Simultaneous sessions on multiple devices

- Log in on a desktop browser and add 2 products to the cart. Then log in on a mobile browser (same account) and add 1 different product — verify that both sessions reflect the same merged cart state after a page refresh on each device.
- Log in on Device A and add Product X to the cart. Without refreshing, log in on Device B and add Product Y. Refresh Device A — verify Product Y is now visible, and no item has been lost.
- Log in on Device A and remove a product from the cart. On Device B (already logged in, same session), verify the product disappears from the cart after a refresh (no stale data).
- Log in on 3 devices simultaneously. Each device adds a unique product. Refresh all devices — verify all 3 products are present on all 3 devices (consistent server-side cart state).
- Log in on Device A (desktop) and log in on Device B (mobile). On Device B, add a high-demand product that has only 1 unit in stock. On Device A, add the same product — verify only one succeeds and the other receives an out-of-stock error.
- Log in on Device A and proceed to checkout. While Device A is on the payment step, log in on Device B and clear the cart — verify Device A's checkout either fails gracefully or completes based on the order reservation logic (not silently place an empty order).
- Verify that a session opened in a private/incognito tab is treated as a separate session from a regular tab, even when logged into the same account in both.
- Test cart behavior when one device has a very slow connection: Device A adds a product but the server request is still in-flight when Device B reads the cart — verify no race condition or data loss occurs.
- Log in on Device A, add products, then change the account password on Device B — verify Device A's session is either terminated or the user is prompted to re-authenticate, and the cart is not lost.
- After logging out on one device, verify the session and cart are no longer accessible from that device, even if the browser back button is used.

### Cart abandonment trigger (timing of "left" detection)

- Add products to the cart and close the browser tab without checking out — verify that the abandoned cart email is triggered after the correct delay (e.g., 1 hour, check the configured threshold), not immediately.
- Add products, navigate to the checkout page, then close the tab — verify the abandonment is detected at the cart level, not the checkout level, or confirm which step triggers the event per the business logic.
- Add products to the cart, then return to the shop within the abandonment detection window and continue browsing — verify no abandoned cart email is sent, as the session is still active.
- Add products, leave, receive an abandoned cart email, click the recovery link — verify the link restores the exact cart contents (same products, quantities, variants, and any applied coupons).
- Add products, leave, receive an abandoned cart email, click the recovery link on a different device than the one used originally — verify the cart is accessible regardless of device.
- Add products while logged in, abandon, then complete a purchase — verify the abandoned cart email series is stopped (no further reminders are sent after the purchase).
- Add products as a guest (with email entered during checkout initiation), abandon the checkout — verify the guest abandonment flow works if an email address was captured.
- Trigger multiple abandonment sessions within a short time — verify duplicate abandonment emails are not sent for the same session.
- Verify the abandoned cart recovery link is not reusable after the cart has been completed (security: link should expire after successful purchase or a time limit).
- Add products, abandon, receive a recovery email, use the link but do not purchase — verify a second reminder (if configured) is sent correctly and the link in the second email also works.

### Cart contents after password reset

- Add products to the cart while logged in. Initiate a password reset (via "Forgot password"), complete the reset, and log in with the new password — verify the cart contents are still present after the password change.
- Add products to the cart as a guest. Navigate to the login page, request a password reset, complete the reset flow, and then log in — verify the guest cart is merged with the account cart after login (even when the login path was via password reset).
- Initiate a password reset but do not complete it — verify the cart is unaffected and accessible when logging in normally with the old password.
- Complete a password reset on Device A. Log in on Device B — verify the cart is accessible from Device B (cart tied to account, not invalidated by password change).
- Verify that completing a password reset does not log out active sessions on other devices AND preserves the cart on those devices (or, if it invalidates sessions per security policy, the cart is recovered on re-login).
- Add products to the cart, reset the password, and log in with the new password — verify no duplicate cart items appear (the session state was not preserved alongside the new session unexpectedly).
- After a password reset, verify the cart restoration does not re-fire analytics add_to_cart events, preventing false conversion data.
- Verify that the password reset link cannot be reused to gain access to the cart after it has been consumed.

### Cookie expiry and session recovery

- Set the browser date/time to a future date past the session cookie expiry, then attempt to load the shop — verify the cart is restored from the server-side persistent cart (if implemented) or the user is prompted to log in again without data loss.
- Manually delete the session cookie via browser DevTools while on the cart page, then refresh — verify the page handles the missing session gracefully (redirect to login, not a blank cart or 500 error).
- Delete all shop-related cookies and local storage entries while items are in the cart (guest session) — verify the expected behavior: either the guest cart is lost (acceptable) with a clear empty-cart message, or it is recovered via fingerprint/local fallback.
- Log in, add products to the cart, then clear all cookies — log in again and verify the server-side persistent cart is restored correctly.
- Verify the session cookie is set with the correct flags: `Secure` (HTTPS only), `HttpOnly` (not accessible via JavaScript), and `SameSite=Lax` or `SameSite=Strict` (CSRF protection).
- Verify the session cookie expiry duration matches the documented/configured value (e.g., 30-day persistent login if "Stay logged in" is checked, vs. session-only cookie if not checked).
- Verify the cart cookie (if separate from the session cookie) has a defined and documented expiry and is not set as a session cookie unintentionally.
- Enable "Stay logged in" / "Remember me" at login, add products, close the browser completely, reopen after 24 hours — verify the persistent cookie restores the session and the cart.
- Do NOT enable "Stay logged in", add products, close the browser completely, reopen — verify the session cookie (non-persistent) has expired and the user is prompted to log in, and the cart is recovered from the server-side DB after login.
- Log in with "Stay logged in" enabled. When the persistent cookie expires (test by manipulating the expiry date in DevTools), verify the user is redirected to login and the cart is preserved on the account for recovery after re-authentication.
- Test cart and session behavior with third-party cookies blocked (browser privacy settings) — verify the shop's own first-party cookies still function correctly and the cart is not affected.
- Verify that `document.cookie` in the browser console does not expose any session token value (confirming the `HttpOnly` flag is correctly set).
