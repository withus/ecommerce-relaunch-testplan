# 11. Wishlist

### Add product to wishlist

- Navigate to a product page while logged out; click the wishlist button (heart icon) and verify the product is added to the cookie-based guest wishlist without requiring login.
- While logged in, click the wishlist button on a product page and verify the product appears in the persistent wishlist in the customer account.
- Click the wishlist button on a product that is already on the wishlist and verify the UI reflects the toggled state (e.g., filled heart vs. outline) and the product is not duplicated.
- Add a product to the wishlist directly from a category page (quick-add button) and verify it is stored correctly.
- Add a product with multiple variants (e.g., size/color) to the wishlist and verify the specific selected variant is saved, not just the base product.
- Add 20+ products to the wishlist in succession and verify all are stored and retrievable without data loss.
- Add an out-of-stock product to the wishlist and verify it is accepted and clearly marked as unavailable on the wishlist page.
- Attempt to add the same product to the wishlist twice in rapid succession (double-click scenario) and verify no duplicate entry is created.
- Verify the wishlist icon in the header updates its counter immediately after adding a product (no page reload required).
- Verify that a non-logged-in user cannot view another user's wishlist by navigating directly to a wishlist URL (e.g., `/wishlist/123`); the response must be a 403 or redirect to login.

---

### Remove product from wishlist

- Open the wishlist page while logged in, click the remove button on a product, and verify the product disappears from the list immediately without a full page reload.
- Remove a product from the wishlist using the wishlist toggle on the product page itself (heart icon) and verify it is no longer listed on the wishlist page.
- Remove the last remaining product from the wishlist and verify the wishlist page displays an appropriate empty-state message (e.g., "Your wishlist is empty") with a call-to-action link.
- Remove a product from a guest wishlist (cookie-based) and verify the cookie is updated correctly and the product does not reappear on page refresh.
- Attempt to remove a product from the wishlist while offline (network disconnected) and verify a user-friendly error message is shown; the list must remain unchanged.
- Verify that after removing a product from the wishlist, the header wishlist counter decrements correctly.
- Confirm that removing a product from the wishlist does not affect the shopping cart (same product in cart must remain untouched).

---

### View wishlist

- Navigate to the wishlist page as a logged-in user and verify all previously added products are displayed with correct name, image, price, and availability status.
- Open the wishlist page as a guest and verify products stored in the cookie-based wishlist are listed correctly.
- Verify that a logged-in user can only see their own wishlist and not other users' wishlists (attempt direct URL access with another user's wishlist ID).
- Verify that an unauthenticated request to a user-specific wishlist URL (e.g., `/account/wishlist`) redirects to the login page.
- Verify that out-of-stock items are clearly flagged (e.g., "Currently unavailable") on the wishlist page.
- Verify that price changes since the product was added to the wishlist are reflected with the current price on the wishlist page (and optionally a "price changed" notice).
- Verify that deleted products (removed from the catalog) are either removed from the wishlist or shown with a "no longer available" notice rather than a broken entry.
- Verify that the wishlist page loads correctly with 50+ items and does not time out or render incorrectly.
- Verify that product images on the wishlist page load correctly, are appropriately sized, and have descriptive alt text.
- Verify that the wishlist page is responsive and usable on mobile devices (touch targets, readable layout, no horizontal scroll).

---

### Move from wishlist to cart

- Click the "Add to cart" button on a wishlist item and verify the product is added to the cart with quantity 1.
- After moving a product to the cart, verify the product remains on the wishlist (move = copy to cart, not transfer) unless the shop explicitly removes it — verify the intended behavior is implemented consistently.
- Attempt to move an out-of-stock product from the wishlist to the cart and verify an appropriate error message is shown (e.g., "This product is currently unavailable") and the cart is not modified.
- Move a product with multiple variants from the wishlist where a variant was saved and verify the correct variant is added to the cart.
- Move a product from the wishlist to the cart when the cart already contains the same product and verify the quantity is incremented correctly (no duplicate line item).
- Verify the cart counter in the header updates immediately after adding a wishlist item to the cart.
- Move multiple wishlist items to the cart in quick succession and verify all are added correctly without race conditions.
- If a "Move all to cart" (or "Add all to cart") feature exists, test it with a mix of in-stock and out-of-stock items; verify only available items are added and a summary error is shown for unavailable ones.

---

### Share wishlist (link, email)

- Generate a shareable wishlist link as a logged-in user and verify the link is a unique, non-guessable URL (not sequential IDs).
- Open the shared wishlist link in a private/incognito browser window (logged-out state) and verify only the shared list is visible — no access to the account or other data.
- Verify that a shared wishlist link grants read-only access; the viewer must not be able to modify or delete items from the original owner's wishlist.
- Share the wishlist via email using the built-in share feature; verify the email is sent, contains the correct link, and renders properly.
- Verify that disabling/revoking a shared wishlist link (if this feature exists) immediately prevents access via that URL.
- Verify that the shared wishlist link correctly displays current prices and availability at the time of viewing, not cached data from when the list was shared.
- Attempt to access a shared wishlist URL that does not exist or has been revoked and verify a 404 or appropriate error page is shown.
- Verify that sharing a wishlist does not expose any personal account data (name, email, address, order history) to the recipient.
- Test the wishlist share link across different browsers and devices to ensure it opens correctly without requiring app installation or login.
- Verify that the "Share" button generates the link via HTTPS only (no plain HTTP links in the generated URL).

---

### Wishlist as guest (cookie-based)

- Add a product to the wishlist without being logged in and verify it persists after navigating to other pages within the same session.
- Close the browser entirely, reopen it, and navigate back to the shop; verify the guest wishlist is still intact (cookie persistence after browser close).
- Add products to the guest wishlist and verify the cookie is set with appropriate flags: `Secure`, `HttpOnly` (or only Secure if JS needs access), and `SameSite` attribute.
- Verify the guest wishlist cookie has a reasonable expiry time (e.g., 30–90 days) and not a session-only cookie that disappears on browser close.
- Add a product to the wishlist in incognito/private browsing mode and verify the wishlist is session-scoped (disappears when the private window closes).
- Test the guest wishlist when cookies are disabled in the browser; verify the user sees a clear notice (e.g., "Enable cookies to save your wishlist") rather than a silent failure.
- Verify that the guest wishlist cannot be read or tampered with via JavaScript console (i.e., the cookie is HttpOnly if security requires it, or the stored data cannot lead to XSS).
- Add 15+ products to the guest wishlist and verify the cookie size does not exceed browser limits and all items are retained (alternative: verify server-side session storage is used instead).

---

### Wishlist as logged-in user (persistent)

- Log in, add products to the wishlist, then log out and back in; verify the wishlist contents are fully persisted in the database and restored on login.
- Verify that the wishlist is tied to the user account and not to the browser/device; log in from a second device and confirm the same wishlist is visible.
- Verify that a logged-in user's wishlist is not accessible to other logged-in users (attempt to access via direct URL with another user's account).
- Add 50+ products to a logged-in user's wishlist and verify performance: the wishlist page must load within an acceptable time (e.g., under 3 seconds).
- Verify that wishlist data is retained after a password change.
- Verify that wishlist data is retained after the user updates their email address.
- Attempt to access the wishlist page after the session has timed out; verify the user is redirected to the login page and returned to the wishlist after logging in.
- Verify that if an admin deletes a product from the catalog, the wishlist entry is either removed or flagged as unavailable — no orphaned or broken entries.
- Verify that the wishlist page is included in GDPR data export (Art. 20): wishlist items should appear in the user's data download.
- Verify that deleting the account also deletes all associated wishlist data (no orphaned records in the database).

---

### Transfer wishlist after login

- Add products to the guest wishlist (cookie-based), then log in to an existing account; verify the guest wishlist items are merged into the account wishlist without duplicates.
- Add products to the guest wishlist, then register a new account; verify the guest wishlist is transferred to the newly created account.
- Add Product A to the guest wishlist; log in to an account that already has Product B on its wishlist; verify the resulting wishlist contains both A and B.
- Add Product A to the guest wishlist; log in to an account that already has Product A on its wishlist; verify no duplicate entry is created after the merge.
- Verify that after a successful merge and transfer, the guest cookie-based wishlist is cleared (no leftover cookie data).
- After transferring the guest wishlist to an account, log out and verify the cookie-based wishlist is now empty (items live in the account, not in the cookie).
- Add products to the guest wishlist; click "Create account" or "Log in" from the wishlist page; verify the redirect after login/registration brings the user back to the wishlist (not the homepage) and the merge has occurred.
- Test the transfer flow when the guest has an empty wishlist and logs in to an account with items; verify the account wishlist is unchanged and no data is lost.
- Test the transfer flow when both the guest wishlist and the account wishlist are empty; verify no errors are thrown and the empty wishlist page is displayed correctly.
- Verify that the wishlist transfer is atomic: if an error occurs mid-merge, neither the guest nor the account wishlist is left in a corrupted or partial state.
