# 10. Customer Account

## 10.1 Registration

### Registration form

- Navigate to the registration page; verify the form renders with all expected fields (first name, last name, email, password, password confirmation, privacy policy checkbox).
- Verify that the form title and all field labels are visible and clearly readable.
- Verify that the form is accessible via the account icon in the header and via the checkout flow.
- Submit the form with all fields filled in correctly; verify the user account is created and the user is redirected to the expected page (dashboard or confirmation page).
- Verify that no sensitive data (password, token) appears in the page URL after successful registration.

### Required fields validation

- Submit the form with all fields empty; verify that all required fields are highlighted with error messages.
- Submit the form with only the email field filled in; verify the remaining required fields show validation errors.
- Submit the form with only the password fields filled in; verify email and other required fields show errors.
- Verify that optional fields (e.g., phone number, date of birth) do not block form submission when left empty.
- Verify that error messages are descriptive (e.g., "Email is required") rather than generic ("Error").

### Email format validation

- Enter a plain string without `@` (e.g., `notanemail`); verify a validation error is shown.
- Enter an email without a domain (e.g., `user@`); verify a validation error is shown.
- Enter an email without a TLD (e.g., `user@domain`); verify whether the shop accepts or rejects it (document expected behavior).
- Enter an email with consecutive dots (e.g., `user..name@domain.com`); verify validation behavior.
- Enter a valid email with uppercase letters (e.g., `User@Domain.COM`); verify it is accepted and normalized to lowercase during storage.
- Enter an email with a subdomain (e.g., `user@mail.domain.com`); verify it is accepted.
- Verify the email field uses `type="email"` so mobile browsers show the email keyboard.

### Password strength requirements

- Enter a password below the minimum length (e.g., 5 characters); verify the error message states the minimum length.
- Enter a password that meets minimum length but has no uppercase letter (if required); verify the specific requirement is shown.
- Enter a password that meets minimum length but has no number (if required); verify the specific requirement is shown.
- Enter a password that meets minimum length but has no special character (if required); verify the specific requirement is shown.
- Enter a password that meets all requirements; verify no strength error is shown.
- Verify that the strength indicator (weak / medium / strong) reflects the actual password quality in real time.
- Verify that the password field masks characters by default.
- Verify that a "show/hide password" toggle reveals and re-hides the password.

### Password confirmation

- Enter matching passwords; verify no error is shown and the form can be submitted.
- Enter mismatching passwords; verify an error message ("Passwords do not match") is shown before or on submit.
- Clear the password confirmation field after entering it; verify the mismatch error reappears.
- Change the value in the first password field after already matching; verify the confirmation field is re-validated.

### Privacy policy checkbox

- Submit the form without checking the privacy policy checkbox; verify submission is blocked and an error is shown.
- Verify the privacy policy checkbox is not pre-checked (GDPR: no pre-checked consent boxes).
- Verify the privacy policy text contains a clickable link that opens the privacy policy page in a new tab.
- Check the checkbox and verify the error disappears.

### Newsletter opt-in

- Verify the newsletter opt-in checkbox is not pre-checked.
- Register without checking newsletter opt-in; verify the user is NOT added to the mailing list.
- Register with newsletter opt-in checked; verify the user receives a newsletter confirmation email (double opt-in flow if enabled).
- Verify unchecking after checking does not submit a subscription.

### Double opt-in email

- Register a new account; verify a double opt-in confirmation email is sent to the provided address.
- Verify the email arrives within a reasonable time (e.g., under 2 minutes).
- Verify the email contains the correct sender name, subject line, and a functioning confirmation link.
- Click the confirmation link; verify the account is activated and the user is redirected to a confirmation or login page.
- Verify that the account cannot log in before the email is confirmed (if the shop enforces this).
- Let the confirmation link expire (if it has a TTL); verify clicking it shows an appropriate expired-link message.
- Request a new confirmation email after expiry; verify a fresh link is sent and the old link is invalidated.

### Confirmation link works

- Click a valid confirmation link; verify the account status changes to "confirmed" or "active".
- Click the same confirmation link a second time; verify a graceful message is shown ("Account already confirmed") rather than an error.
- Modify the confirmation token in the URL (tamper test); verify the link is rejected with an appropriate error.
- Verify the confirmation link uses HTTPS.

### Already registered email handling

- Attempt to register with an email address that already has an account; verify an error message is shown (e.g., "An account with this email already exists").
- Verify the error message does NOT reveal whether a password exists or whether the account was created via social login (no user enumeration beyond what is necessary).
- Verify the form retains all other entered data (only the email field is flagged), so the user does not have to retype everything.

### Social login (Google, Facebook, Apple)

- Click "Continue with Google"; verify the OAuth flow redirects to the Google consent screen.
- Complete the Google OAuth flow with a valid account; verify a new shop account is created and the user is logged in.
- Complete the Google OAuth flow with the same Google account a second time; verify the user is logged in to the existing account (no duplicate account created).
- Deny consent on the Google OAuth screen; verify the user is returned to the registration page without an error.
- Repeat the above three scenarios for Facebook and Apple (if implemented).
- Verify that social login accounts can set a password later for email/password login.
- Verify that no sensitive OAuth tokens are exposed in the URL or page source after the flow.

### Registration during checkout

- Start checkout as a guest and reach the address step; verify a "Create account" option or checkbox is available.
- Check "Create account" during checkout, enter a password; complete the order and verify a new account is created.
- Verify the order is associated with the newly created account and appears in order history.
- Verify that omitting the "Create account" option allows guest checkout to proceed without creating an account.

---

## 10.2 Login

### Login with email + password

- Enter valid credentials; verify the user is logged in and redirected to the dashboard or the page they were on before.
- Verify that the session cookie is set with the `Secure`, `HttpOnly`, and `SameSite` flags.
- Verify that the password field value is never echoed back in the page HTML or in any response.
- Log in and verify that the account icon in the header changes to show the logged-in state (name or avatar).

### Login with username (if supported)

- If the shop supports username-based login, enter a valid username and password; verify login succeeds.
- Enter a valid email instead of username (if both are supported); verify login succeeds.
- Enter a valid username with wrong password; verify the error message is shown.

### Wrong credentials error message

- Enter a valid email with a wrong password; verify a generic error message is shown (e.g., "Invalid email or password") — NOT "Password is incorrect" (which confirms the email exists).
- Enter a non-existent email with any password; verify the same generic error message is shown (no user enumeration).
- Enter correct credentials with an extra trailing space; verify login fails or the input is trimmed gracefully (document expected behavior).
- Verify no stack trace or internal error details are shown on failed login.

### Account locked after X failed attempts

- Attempt login with wrong credentials X times in a row (X = configured lockout threshold); verify the account is locked.
- Verify a clear message is shown (e.g., "Account locked. Try again in 15 minutes or reset your password.").
- Verify a lockout notification email is sent to the account owner.
- Wait for the lockout period to expire; verify login is possible again with correct credentials.
- Verify that successful login resets the failed-attempt counter.
- Verify that lockout is per-account, not per-IP only (so an attacker using a different IP still triggers lockout).

### "Forgot password" flow

- Click "Forgot password" on the login page; verify a password reset page or modal is shown with an email field.
- Enter a registered email and submit; verify a success message is shown (e.g., "If an account exists, a reset email has been sent") — without confirming whether the email is registered.
- Enter a non-existent email; verify the same neutral success message is shown (no user enumeration).
- Verify the reset email is sent within a reasonable time.

### Password reset email

- Verify the reset email arrives from the correct sender address.
- Verify the email subject is descriptive (e.g., "Reset your password").
- Verify the email body contains the user's name and a clear call-to-action reset link.
- Verify the reset link uses HTTPS and does not contain the password in plaintext.

### Password reset link expiry

- Click a password reset link within the validity period; verify it opens a "Set new password" form.
- Let the link expire (e.g., after 1 hour); click it and verify an appropriate "Link expired" message is shown.
- Verify that an expired link cannot be reused to set a new password.
- Use a reset link after the password has already been reset; verify the link is invalidated and shows an appropriate message.

### Set new password

- Enter a valid new password and confirmation; submit and verify the password is updated and the user is either logged in or redirected to the login page.
- Enter a new password that is the same as the old one; verify whether this is allowed or blocked (document expected behavior).
- Enter a mismatching confirmation; verify a validation error is shown.
- Enter a new password below the minimum strength; verify validation error is shown.
- Verify that after a password reset, all existing sessions for that account are invalidated (forced logout on all devices).

### "Stay logged in" / remember me

- Log in with "Remember me" checked; close the browser completely and reopen it; verify the user is still logged in.
- Log in without "Remember me"; close the browser; reopen and verify the session is expired.
- Verify that the "remember me" cookie has a long expiry (e.g., 30 days) and is set with `Secure` and `HttpOnly` flags.
- Verify that clicking "Logout" removes the persistent cookie and requires re-login.

### Session timeout

- Log in and leave the session inactive for the configured timeout period; attempt to access an authenticated page and verify the user is redirected to login.
- Verify the session timeout message is user-friendly (e.g., "Your session has expired. Please log in again.").
- Verify that after timeout, the user is redirected back to the page they were trying to access after re-login.
- Verify that session tokens are rotated after re-login (no fixation).

### Login redirect (back to previous page)

- While not logged in, attempt to access a protected page (e.g., `/account/orders`); verify the user is redirected to login.
- After successful login, verify the user is redirected back to the originally requested page, not the generic dashboard.
- Verify that the redirect URL stored in the session cannot be manipulated to redirect to an external site (open redirect protection).

### Social login

- Use a social login provider to log in to an existing account; verify session is established correctly.
- Verify that social login session cookies carry the same security flags as email/password sessions.
- Verify that a user who registered via email/password cannot be taken over via social login with the same email (or that the linking is explicit and confirmed).

### Two-factor authentication (if available)

- Enable 2FA on an account; attempt login with correct password; verify a 2FA prompt (TOTP or email code) is shown.
- Enter the correct 2FA code; verify login succeeds.
- Enter an incorrect 2FA code; verify login is blocked with an appropriate error.
- Enter a valid 2FA code after it has expired (TOTP window passed); verify it is rejected.
- Verify that the "Remember this device" option (if present) works for the configured period.
- Verify recovery codes can be used when the 2FA device is unavailable.
- Disable 2FA and verify login reverts to single-factor.

---

## 10.3 Customer Account Area

### Dashboard / overview

- Log in and navigate to the account dashboard; verify it loads without errors and shows a summary of recent orders, saved addresses, and quick links.
- Verify that the dashboard does not expose any other customer's data.
- Verify the dashboard is not accessible without authentication (direct URL access redirects to login).

### Edit personal data

- Navigate to "Personal data" or "Profile"; verify the current data (name, email, phone) is pre-filled correctly.
- Change the first name and save; verify the change is reflected immediately and persists after page reload.
- Attempt to save with the first name field empty; verify a validation error is shown.
- Enter special characters and umlauts (e.g., `Müller-Schmidt`) in the name fields; verify they are saved and displayed correctly.
- **Security:** Attempt to inject HTML/script into the name field (`<script>alert(1)</script>`); verify it is sanitized and not executed.
- **Isolation:** Log in as User A, note User A's account URL/ID; log in as User B in another browser and attempt to access User A's edit URL; verify access is denied (403 or redirect).

### Change email address

- Navigate to email change; enter a new, valid email address and current password for confirmation; verify a confirmation email is sent to the new address.
- Verify the old email address remains active until the new email is confirmed.
- Click the confirmation link in the new email; verify the email address is updated.
- Attempt to change to an email address already used by another account; verify an appropriate error is shown.
- Verify that after email change, a notification is sent to the old email address informing the user of the change.
- Attempt to change email without providing the current password (if required); verify it is blocked.

### Change password

- Navigate to password change; enter the current password and a valid new password (with confirmation); verify the password is updated.
- Verify a success confirmation is shown and a notification email is sent.
- Enter an incorrect current password; verify the change is rejected with an appropriate error.
- Enter a new password below the minimum strength; verify a validation error is shown.
- Enter mismatching new password and confirmation; verify a validation error is shown.
- **Session invalidation:** After changing the password, verify that all other active sessions (other browsers/devices) are terminated and require re-login.
- Verify the current session either remains valid (with a fresh token) or also requires re-login (document expected behavior).

### Address book (add, edit, delete addresses)

- Add a new shipping address with all required fields; verify it is saved and appears in the address list.
- Add an address with only required fields; verify it is accepted.
- Attempt to add an address with an invalid postal code format; verify validation catches it.
- Edit an existing address; change the street and save; verify the change persists.
- Delete an address that is not the default; verify it is removed from the list.
- Attempt to delete the only address; verify whether deletion is blocked or the default address protection message is shown.
- **Isolation:** Log in as User A and note an address ID; log in as User B and attempt to edit/delete User A's address via direct URL manipulation; verify access is denied.

### Set default address

- Mark a non-default address as default; verify the previous default loses its default status.
- Proceed to checkout; verify the default address is pre-selected in the shipping address step.
- Delete the current default address; verify a different address is automatically promoted to default or the user is prompted to set a new one.

### Order history / order overview

- Navigate to order history; verify all past orders are listed with order number, date, status, and total.
- Verify that only orders belonging to the logged-in user are shown.
- **Isolation:** Note an order number from User A; log in as User B and attempt to access User A's order detail URL directly; verify access is denied (403 or redirect).
- Verify that orders placed as a guest (before account creation with the same email) are either shown or not shown per the shop's data policy (document expected behavior).
- Verify that pagination or "load more" works correctly for customers with many orders.

### View individual order (details)

- Click on an order in the history; verify the detail page shows all items, quantities, prices, shipping address, payment method, and order status.
- Verify that price totals on the order detail match the original order confirmation.
- Verify that the order detail page for another user's order ID returns 403 or redirects (IDOR protection test).

### Reorder

- Click "Reorder" on a completed order; verify all items are added to the cart.
- Verify that out-of-stock items from the original order show a notice instead of being silently skipped.
- Verify that current prices are used for the reorder, not the historical order prices.
- Verify that the reorder does not accidentally duplicate items already in the cart (or that the behavior is clear and expected).

### Download invoice (PDF)

- Navigate to a completed order and click "Download Invoice"; verify a valid PDF is downloaded.
- Verify the PDF contains the correct mandatory information (invoice number, date, address, line items, VAT, total).
- **Isolation:** Attempt to download an invoice for another user's order by manipulating the URL or invoice ID; verify access is denied.
- Verify the invoice download link requires an active authenticated session (direct link without session returns 403 or redirects to login).

### Shipment tracking

- Navigate to a shipped order; verify a tracking number and/or tracking link is displayed.
- Click the tracking link; verify it opens the carrier's tracking page in a new tab with the correct tracking number pre-filled.
- For an order not yet shipped, verify no tracking information is shown (or a "not yet shipped" notice is displayed).

### Initiate return

- Navigate to a delivered order within the return period; verify a "Return" or "Initiate return" button is available.
- Click the button; verify a return form is shown allowing the user to select items and reason.
- Submit the return request; verify a confirmation is shown and a confirmation email is sent.
- Verify the order status updates to reflect the return in progress.
- Attempt to initiate a return for an order outside the return period; verify the button is absent or the attempt is rejected with a clear message.
- **Isolation:** Attempt to initiate a return on another user's order by manipulating the URL; verify access is denied.

### View wishlist

- Navigate to the wishlist in the account area; verify all saved products are listed with image, name, and price.
- Verify the wishlist is not accessible without authentication.
- **Isolation:** Attempt to access another user's wishlist via direct URL; verify access is denied.
- Verify that products marked as out-of-stock show an appropriate notice in the wishlist.

### Newsletter settings

- Navigate to newsletter settings; verify the current subscription status (subscribed / unsubscribed) is correctly shown.
- Toggle newsletter subscription off; save; verify the setting persists and the user is removed from the mailing list.
- Toggle newsletter subscription back on; verify a new double opt-in email is sent (if required by shop policy).
- Verify the unsubscribe link in newsletter emails also updates the preference in the account settings.

### Delete account (GDPR)

- Navigate to account deletion; verify a clear warning is shown explaining what will be deleted.
- Confirm deletion; verify the account and all personal data are removed (or anonymized per GDPR policy).
- Verify a confirmation email is sent to the account's email address.
- Attempt to log in with the deleted account's credentials; verify login fails.
- Verify that the email address of a deleted account can be used to register a new account after deletion.
- Verify that orders associated with the deleted account are retained for legal/accounting purposes but anonymized (document expected behavior per GDPR policy).
- Attempt to access the account's previous order URLs after deletion; verify access is denied.

### Logout

- Click "Logout"; verify the user is redirected to the homepage or login page and the session cookie is invalidated.
- After logout, attempt to navigate back to the dashboard via browser history or direct URL; verify the user is redirected to login (session is not restored from cache).
- After logout, attempt to use the session token (from a copied cookie) in a request; verify the server rejects it (session server-side invalidation).
- Verify that "Remember me" cookies are also cleared on explicit logout.
- Verify that logging out on one device does not automatically log out other active sessions on other devices (unless the shop explicitly implements global logout — document expected behavior).
