# 26. Admin Backend

### Admin login / authentication

- Verify that the admin login page is accessible only via HTTPS and is not indexed by search engines (check `robots.txt` and meta `noindex`).
- Attempt login with correct credentials — confirm successful authentication and redirect to the admin dashboard.
- Attempt login with an incorrect password — confirm a generic error message is shown (no disclosure of whether email or password was wrong).
- Attempt login with a non-existent email address — confirm the same generic error message is returned (no user enumeration).
- Trigger account lockout by submitting 5 or more consecutive failed login attempts — confirm the account is temporarily locked and an alert email is sent to the account owner.
- Verify that brute-force protection (rate limiting or CAPTCHA) activates after repeated failures.
- Confirm that the password reset flow for admin accounts requires email verification and that reset tokens expire after a configurable period (e.g., 1 hour).
- Verify that a new admin session generates a fresh, unpredictable session token and that the previous session token is invalidated after login (session fixation prevention).
- Confirm that admin sessions expire after a configurable idle timeout (e.g., 30 minutes) and that the user is redirected to the login page with an appropriate message.
- Verify that logout invalidates the server-side session immediately — reusing the old session cookie after logout must return a 401/redirect, not the admin dashboard.
- Confirm that admin login is not accessible via the same URL path used for customer login, and that customer session tokens cannot be escalated to admin access.
- Verify that two-factor authentication (TOTP / email OTP) can be enabled for admin accounts and that login without the second factor is blocked when 2FA is active.
- Confirm that all failed login attempts are written to an audit log (timestamp, IP address, email used).

### Dashboard overview

- Verify that the dashboard loads within an acceptable time (under 3 seconds) even with a large dataset (e.g., 100,000 orders).
- Confirm that all key metrics displayed (e.g., today's revenue, new orders, new customers) match the actual database values — cross-check at least 3 figures against raw queries.
- Verify that a read-only admin role can view the dashboard but cannot click through to perform any write operations from it.
- Confirm that each dashboard widget refreshes correctly (either on page load or via AJAX polling) and does not cache stale data beyond its defined TTL.
- Verify that the dashboard does not expose any personally identifiable information (PII) on the overview screen that is not necessary (e.g., full customer names or email addresses should not appear in aggregate charts).
- Confirm that dashboard access attempts by unauthenticated users (direct URL access) result in a redirect to the admin login page, not a 403 or 500 error.
- Verify that a shop manager role can access the dashboard but is denied access to system configuration sections linked from the dashboard.

### Order management

- Verify that all orders appear in the order list, sorted by date descending by default, with correct order numbers, customer names, totals, and statuses.
- Confirm that searching and filtering orders (by order number, customer email, status, date range, amount) returns accurate and complete results.
- Open an individual order and verify that all line items, prices, discounts, shipping costs, VAT breakdown, and total match the original order confirmation.
- Verify that a customer service role can view and search orders but cannot access financial reports or system settings.
- Confirm that bulk actions (e.g., mark multiple orders as processed, export selected orders) work correctly and apply only to the selected records.
- Verify that order data is read-only for roles that should not edit orders (e.g., warehouse staff) — edit/delete buttons must not be rendered or must return a 403 if called directly.
- Confirm that pagination in the order list works correctly — navigating to page 2+ does not repeat or skip records.
- Verify that the order list does not expose sensitive payment data (full card number, CVV) at any point in the admin UI.
- Confirm that every admin action on an order (view, status change, note added) is recorded in an audit log with user ID, timestamp, and action taken.

### Change order status

- Verify that all valid status transitions (e.g., Received → Paid → Processing → Shipped → Delivered) can be performed by an authorized admin user.
- Attempt an invalid status transition (e.g., Delivered → Processing) — confirm the system rejects it with a clear error message.
- Verify that changing order status to "Shipped" triggers a shipping confirmation email to the customer with the correct tracking information.
- Confirm that changing status to "Cancelled" triggers a cancellation email and, where applicable, initiates a refund or stock release.
- Verify that a warehouse role can move an order to "Shipped" but cannot cancel orders or process refunds.
- Confirm that each status change is recorded in the order's history log (new status, admin user, timestamp) and is visible in the order detail view.
- Verify that concurrent status updates to the same order (e.g., two admin users acting simultaneously) are handled safely — last-write-wins with a conflict warning, or optimistic locking prevents silent data loss.
- Confirm that status changes via the admin UI and via API/webhook (e.g., payment provider callback) are both reflected consistently and neither silently overwrites the other.

### Create invoice

- Verify that an invoice can be generated for any completed/paid order and that the resulting PDF contains all legally required information (seller address, buyer address, invoice number, invoice date, line items, VAT breakdown, total, tax ID/VAT ID).
- Confirm that invoice numbers are sequential, unique, and never reused — including after deletion or voiding.
- Verify that the invoice PDF is downloadable by the admin and that the same PDF is stored and accessible in the customer account area.
- Confirm that generating an invoice for an already-invoiced order either displays the existing invoice or prompts for confirmation before creating a duplicate.
- Verify that invoice generation is restricted to roles with finance or order-management permissions — a warehouse role must not be able to generate invoices.
- Confirm that the invoice is automatically emailed to the customer when generated (or that there is an explicit "send to customer" option).
- Verify that a voided or corrected invoice creates a corresponding credit note with a proper reference to the original invoice number.
- Confirm that the invoice generation action is written to the audit log (admin user, timestamp, order ID, invoice number).

### Create delivery note

- Verify that a delivery note (packing slip) can be generated for any order in "Processing" or later status, and that it lists all items, quantities, and the delivery address correctly.
- Confirm that the delivery note does not include pricing information (or that a configurable option controls this) to prevent exposing prices to warehouse staff.
- Verify that partial delivery notes can be created when only a subset of items in an order are being shipped, and that the remaining items are flagged as pending.
- Confirm that creating a delivery note is accessible to warehouse staff roles but not to customer-facing roles.
- Verify that each delivery note has a unique identifier and that generating a new one for an already-fulfilled order requires confirmation.
- Confirm that the delivery note creation action is recorded in the audit log.

### Manage returns

- Verify that a return request submitted by a customer appears in the admin returns queue with all relevant details (order ID, items, reason, date, status).
- Confirm that admins can approve or reject a return request and that each action triggers an appropriate status email to the customer.
- Verify that approving a return automatically generates a return label (or prompts the admin to upload/generate one) and that the label is accessible to the customer.
- Confirm that the return management interface is restricted to customer service and order management roles — roles without those permissions must receive a 403 on direct URL access.
- Verify that partial returns (only some items from an order) are handled correctly — only the returned items affect inventory and refund calculations.
- Confirm that the return deadline is enforced: attempting to approve a return submitted after the configured return period displays a warning to the admin.
- Verify that each return action (approve, reject, label generated, item received) is logged in the audit trail with admin user ID and timestamp.

### Process refunds

- Verify that a full refund can be initiated for a paid order and that the refund is correctly submitted to the payment provider (Stripe, PayPal, etc.) and confirmed.
- Confirm that a partial refund can be issued for individual line items or a custom amount, and that the refunded amount never exceeds the original charged amount.
- Verify that refund processing is restricted to roles with finance permissions — a warehouse or customer service role without finance access must be blocked.
- Confirm that attempting to refund an order that was already fully refunded results in a clear error, not a duplicate refund submission.
- Verify that after a refund is processed, the order status is updated accordingly and the customer receives a refund confirmation email.
- Confirm that the refund amount is correctly reflected in financial reports and dashboard revenue figures.
- Verify that refund actions are written to the audit log (admin user, order ID, refund amount, payment provider response, timestamp).
- Confirm that if the payment provider returns an error during refund processing, the admin sees a meaningful error message and the order status is not incorrectly updated to "Refunded".

### Product management (CRUD)

- Verify that a new product can be created with all required fields (name, SKU, price, category, description, images, tax class, stock) and appears correctly on the storefront after publication.
- Confirm that all field validations work: negative prices are rejected, mandatory fields enforce submission, SKU uniqueness is enforced, and overly long strings are truncated or rejected with an error.
- Verify that editing an existing product saves changes correctly and that the updated data is immediately reflected on the storefront (cache invalidated).
- Confirm that deleting a product that has active orders does not delete historical order data — the product is deactivated/archived, not hard-deleted, or the deletion is blocked with a warning.
- Verify that duplicate SKU creation is blocked with a clear error message.
- Confirm that product management is restricted to roles with catalog-management permissions — a customer service role must not see or access product CRUD screens.
- Verify that bulk product status changes (activate/deactivate multiple products) work correctly and that only the selected products are affected.
- Confirm that every create, update, and delete action on a product is written to the audit log (admin user, product ID, fields changed, timestamp).
- Verify that unpublished (draft) products are not visible on the storefront or in search results.
- Confirm that image uploads for products enforce file type (JPEG, PNG, WebP only) and file size limits, and reject executable files.

### Category management

- Verify that new categories can be created with a name, slug, parent category, description, and image, and that they appear in the storefront navigation after publication.
- Confirm that the category slug is unique across all categories and that attempts to create a duplicate slug are rejected with an error message.
- Verify that renaming or moving a category (changing its parent) automatically creates a 301 redirect from the old URL to the new URL.
- Confirm that deleting a category prompts the admin to reassign products before deletion, and that no products are left in an orphaned state.
- Verify that nested category hierarchies (at least 3 levels deep) are handled correctly in both the admin tree view and the storefront navigation.
- Confirm that category management access is restricted to roles with catalog-management permissions.
- Verify that changes to category structure are recorded in the audit log.

### Customer management

- Verify that the customer list displays all registered customers with email, name, registration date, order count, and total spend, and that the data is accurate.
- Confirm that searching and filtering customers (by email, name, registration date, order history) returns correct results.
- Verify that an admin can view a customer's full profile, address book, order history, and account status — but that full payment card data is never displayed.
- Confirm that editing a customer's data (name, email, address) is restricted to roles with CRM or customer service permissions.
- Verify that resetting a customer's password from the admin panel sends a reset email to the customer and does not display the new password in the admin UI.
- Confirm that deactivating a customer account prevents that customer from logging in and displays an appropriate message on login attempt.
- Verify that permanently deleting a customer account (GDPR erasure) anonymizes all PII in order records rather than deleting historical order data entirely.
- Confirm that any admin action on a customer account (edit, deactivate, delete, password reset) is recorded in the audit log.
- Verify that customer management screens are not accessible to roles without CRM access — direct URL attempts must return 403.

### Coupon management

- Verify that a new coupon can be created with all available options (type: percentage/fixed/free shipping, value, minimum order, usage limit, per-customer limit, expiry date, applicable products/categories) and that it is saved correctly.
- Confirm that the coupon code field enforces uniqueness — duplicate codes are rejected.
- Verify that setting an expiry date on a coupon causes it to be automatically rejected at checkout after the expiry, without requiring manual deactivation.
- Confirm that single-use coupons cannot be applied more than once across different orders, even by different customers.
- Verify that a coupon with a per-customer usage limit of 1 is blocked on the second use by the same customer.
- Confirm that deactivating a coupon in the admin immediately prevents it from being applied at checkout.
- Verify that bulk coupon generation (for campaign use) creates the specified number of unique codes.
- Confirm that coupon usage statistics (number of uses, total discount given) are accurately tracked and displayed in the admin.
- Verify that coupon management is restricted to roles with marketing or promotion permissions.
- Confirm that coupon create/edit/delete/deactivate actions are written to the audit log.

### CMS page management

- Verify that a new CMS page can be created with a title, slug, meta title, meta description, content body, and publication status, and that it appears on the storefront when published.
- Confirm that the slug field enforces uniqueness and that conflicts with existing routes (e.g., `/checkout`, `/products`) are blocked.
- Verify that unpublished (draft) CMS pages are not accessible on the storefront — direct URL access must return 404 or redirect.
- Confirm that editing a published CMS page and saving it immediately updates the storefront content (cache invalidated).
- Verify that deleting a CMS page that is linked in the navigation menu removes or deactivates the navigation link, rather than leaving a dead link.
- Confirm that CMS page management is restricted to roles with content-management permissions.
- Verify that the content editor does not allow saving raw `<script>` tags or other XSS vectors in page content (input is sanitized on save and on render).
- Confirm that all CMS page changes are recorded in the audit log with the admin user and a diff of changed fields where possible.

### Media management (images, files)

- Verify that images can be uploaded in supported formats (JPEG, PNG, WebP, GIF) and are rejected for unsupported or potentially dangerous formats (SVG with scripts, PHP files, EXE).
- Confirm that the maximum file size limit is enforced and that exceeding it produces a clear error message.
- Verify that uploaded filenames are sanitized (no path traversal characters, no executable extensions) before being stored.
- Confirm that media files are stored outside the web root or behind access control so that direct URL guessing cannot expose files that have been "deleted" in the admin.
- Verify that deleting a media file that is still referenced by a product or CMS page either blocks the deletion with a warning or shows a list of pages that will be affected.
- Confirm that media folders/libraries can be browsed, and that pagination works correctly with large media libraries (1,000+ files).
- Verify that media management access is restricted to roles with content or catalog permissions.
- Confirm that file uploads and deletions are recorded in the audit log.

### Settings (shop, shipping, payment, taxes)

- Verify that shop-wide settings (store name, address, currency, default language, email sender) can be saved and that changes take effect immediately.
- Confirm that shipping zone configurations (countries, weight thresholds, flat rates, free-shipping thresholds) are saved correctly and that checkout shipping calculations reflect the updated rules.
- Verify that adding, editing, or removing a payment method in settings correctly enables or disables it at checkout.
- Confirm that tax rate configurations (rate, applicable country/region, product tax class) are saved correctly and that checkout tax calculations reflect the updated rates.
- Verify that settings changes are validated before saving — invalid values (e.g., negative tax rates, non-numeric thresholds) are rejected with clear error messages.
- Confirm that access to the settings area is restricted to the superadmin or a dedicated settings role — all other roles must be denied access.
- Verify that each settings save action is written to the audit log, including the field changed, the old value, the new value, the admin user, and the timestamp.
- Confirm that reverting a setting to its previous value takes effect immediately and that the audit log reflects the revert.

### User / role management

- Verify that a new admin user can be created with a name, email, password, and role assignment, and that the account receives a welcome/activation email.
- Confirm that each predefined role (superadmin, shop manager, customer service, warehouse, finance, content manager) grants access only to the sections defined for that role — test each role by logging in as a user with only that role.
- Verify that a user assigned to a restricted role cannot access forbidden sections by direct URL — the response must be a 403 (or redirect to dashboard with error), never a 200 with partial data.
- Confirm that a superadmin can change another user's role and that the change takes effect on the affected user's next request (or forces a session refresh).
- Verify that deactivating an admin user immediately invalidates any active sessions for that user.
- Confirm that deleting an admin user does not delete associated audit log entries — historical actions must remain attributed to the deleted user's identifier.
- Verify that the password policy is enforced for new admin users (minimum length, complexity) and that weak passwords are rejected.
- Confirm that role management (create role, assign permissions to role) is restricted to the superadmin role only.
- Verify that all user and role management actions are written to the audit log (created by, affected user, role assigned/changed, timestamp).
- Confirm that there is always at least one active superadmin account — the system must block deletion or deactivation of the last superadmin.

### Export functions (CSV, PDF)

- Verify that order exports (CSV) contain all expected columns (order ID, date, customer name, email, items, quantities, prices, shipping, total, status, payment method) and that values match the database.
- Confirm that customer exports contain all PII fields that are expected and are restricted to roles with explicit export permissions.
- Verify that exporting a filtered result set (e.g., orders in a specific date range) includes only the filtered records, not the full dataset.
- Confirm that PDF exports (e.g., packing slips, invoice batches) render correctly with accurate data and no layout overflow.
- Verify that large exports (e.g., 50,000 orders) are handled via background processing with a download link, rather than timing out the HTTP request.
- Confirm that exported files do not include sensitive fields that are not necessary for the export purpose (e.g., hashed passwords, payment tokens).
- Verify that export access is logged — each export action is recorded in the audit log with admin user, export type, filters applied, and timestamp.
- Confirm that concurrent export requests from multiple admin users do not interfere with each other and each receives the correct file.

### Import functions

- Verify that a valid CSV product import processes all rows correctly (correct field mapping, no duplicates) and that imported products appear on the storefront.
- Confirm that a CSV with validation errors (missing required fields, invalid prices, duplicate SKUs) produces a row-level error report without silently importing partial data.
- Verify that importing a file with an unsupported format (e.g., an executable disguised as CSV) is rejected before processing.
- Confirm that very large import files are processed in the background (queue) and that the admin is notified on completion, rather than the request timing out.
- Verify that importing customer or order data is restricted to superadmin or specifically authorized roles.
- Confirm that each import action is recorded in the audit log with admin user, file name, number of rows processed, number of errors, and timestamp.
- Verify that a failed import can be retried without creating duplicate records from partially successful prior runs (idempotency or conflict handling).

### Statistics / reports

- Verify that revenue reports display correct totals for a selected date range — cross-check at least one period against raw order data.
- Confirm that all charts and aggregations (top products, top categories, conversion rate, average order value) are mathematically consistent with underlying order records.
- Verify that reports respect role-based access — a warehouse role must not be able to access revenue or customer reports.
- Confirm that reports can be exported (CSV or PDF) and that the exported data matches what is displayed in the UI.
- Verify that the report date-range filter behaves correctly for timezone edge cases (e.g., orders placed at midnight are attributed to the correct day in the shop's configured timezone).
- Confirm that statistical data is not cached for longer than its defined TTL, so that reports always reflect reasonably current data.
- Verify that reports involving PII (e.g., customer spend reports) are subject to the same access controls as direct customer management screens.

### Log viewer

- Verify that the log viewer displays system events (errors, warnings, info), including timestamp, log level, message, and context, in reverse-chronological order.
- Confirm that filtering logs by level (error, warning, info, debug), by date range, or by search term returns only matching entries.
- Verify that the log viewer does not expose sensitive data in log messages — credentials, API keys, full payment details, or session tokens must be masked or excluded from logs.
- Confirm that log viewer access is restricted to superadmin or explicitly authorized roles — direct URL access by unauthorized roles must return 403.
- Verify that audit log entries (admin user actions) are separate from system error logs and are tamper-evident — no admin user should be able to delete or edit audit log entries.
- Confirm that the audit log captures at minimum: user ID, username, action type, affected resource (type + ID), old value / new value where applicable, IP address, and timestamp.
- Verify that log rotation and retention policies are applied — old logs are archived or deleted according to the configured schedule and do not grow unbounded.
- Confirm that attempting to download or export logs is itself recorded as an audit event.
- Verify that injecting special characters into log messages (e.g., via user-supplied input that ends up in logs) does not cause log injection, log forging, or XSS when the log viewer renders the messages.
