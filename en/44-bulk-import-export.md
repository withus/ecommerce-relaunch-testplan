# 44. Bulk Operations & Import/Export

---

### CSV/Excel product import (prices, descriptions, images)

- Upload a well-formed CSV file with 100 products; verify all rows are imported without errors, prices and descriptions match the source data exactly.
- Upload a well-formed XLSX file; verify the system parses the Excel format correctly and imports all data.
- Upload a CSV with UTF-8 BOM (byte order mark at file start `\xEF\xBB\xBF`); verify the BOM is stripped and the first column header is not corrupted (e.g., not `ï»¿name` but `name`).
- Upload a CSV saved in Latin-1 / ISO-8859-1 encoding containing umlauts (ä, ö, ü, ß); verify the system either rejects with a clear encoding error or converts correctly without garbled characters.
- Upload a CSV with Windows-style line endings (CRLF `\r\n`) and verify it is parsed identically to a Unix LF file.
- Upload a CSV with mixed line endings within the same file; verify the parser handles this gracefully without skipping or merging rows.
- Upload a CSV where a description field contains embedded commas wrapped in double quotes (RFC 4180 quoting); verify the field is not split mid-value.
- Upload a CSV where a field contains embedded double quotes escaped as `""` (RFC 4180); verify the value is stored with a literal double quote, not double quotes.
- Upload a CSV where an image column contains absolute URLs; verify images are fetched and associated with the correct products.
- Upload a CSV where an image column contains relative file paths; verify the system resolves paths correctly or returns a clear validation error.
- Upload a CSV with a `price` column containing non-numeric values (e.g., `"N/A"`, `"-"`, empty string); verify the row is flagged as invalid and rejected, while valid rows continue to import.
- Upload a CSV with a negative price (e.g., `-5.00`); verify the system rejects the row or prompts for confirmation.
- Upload a CSV containing a price with thousand-separator dots (e.g., `1.299,99` German format vs. `1,299.99` English format); verify the system handles the configured locale or returns a clear format error.
- Upload a CSV with no header row; verify the system returns a meaningful error rather than importing garbage data.
- Upload a CSV with extra columns not expected by the import schema; verify the extra columns are ignored without error.
- Upload a CSV with missing required columns (e.g., no `sku` column); verify import is blocked with a specific error listing the missing column name.
- Upload an empty CSV file (0 data rows, header only); verify the system returns a clear message ("No records to import") rather than a generic error.
- Upload a completely empty file (0 bytes); verify the system returns a validation error and does not crash.
- Upload a file with the `.csv` extension but containing binary/non-text content (MIME type mismatch); verify the system detects the mismatch and rejects the upload.
- Attempt to import a CSV with an SQL injection payload in a field (e.g., `product_name` = `'; DROP TABLE products;--`); verify the value is stored as plain text and no SQL is executed.
- Attempt to import a CSV with an XSS payload in a description field (e.g., `<script>alert(1)</script>`); verify the value is sanitised or escaped before storage and does not execute on the frontend.
- Upload a very large CSV file (e.g., 50,000 rows, ~50 MB); verify the import runs to completion without PHP/server timeout and provides a progress indicator or queues the job asynchronously.
- Upload a CSV with duplicate SKUs within the same file; verify the system detects the duplicates and either rejects all duplicates or applies a defined conflict resolution (first-wins / last-wins) with a clear report.
- Upload a CSV with a SKU that already exists in the database; verify the system correctly applies the configured conflict strategy (skip, overwrite, or error) and reports which rows were affected.
- After a failed import, verify that no partial data was written to the database (transactional rollback), or that partial imports are clearly flagged in the error report.
- Verify the import UI shows a summary on completion: total rows processed, rows imported successfully, rows skipped, rows failed, and downloadable error log.

---

### Product export for marketplaces (Google Shopping, idealo)

- Trigger a Google Shopping feed export; verify the output is valid XML conforming to the Google Merchant Center specification (verify with an XML schema validator or the GMC feed validator).
- Verify the Google Shopping feed contains all mandatory fields: `g:id`, `g:title`, `g:description`, `g:link`, `g:image_link`, `g:price`, `g:availability`, `g:condition`.
- Verify `g:price` in the feed uses the correct format (e.g., `19.99 EUR`) and not a locale-formatted string with commas.
- Verify `g:availability` correctly maps shop stock status: in-stock products show `in stock`, out-of-stock products show `out of stock`.
- Trigger an idealo product feed export; verify the output format matches the current idealo CSV/XML specification (required columns present, correct delimiters).
- Verify that products marked as "disabled" or "draft" are excluded from all marketplace exports.
- Verify that products without a valid price or missing mandatory fields are either excluded from the feed or trigger a feed validation warning in the admin.
- Export the feed for a shop with 10,000+ products; verify the export completes within an acceptable time limit and the resulting file is not truncated.
- Verify the feed URL (if auto-generated and publicly accessible) is not guessable or protected, or intentionally public per platform requirements.
- Re-export after updating a product price; verify the new price appears in the next feed generation and the old price is gone.
- Verify that product images referenced in the feed return HTTP 200 and are publicly accessible (not behind auth).
- Export the feed and verify special characters in product titles (umlauts, ampersands `&`) are properly encoded (XML entities or UTF-8) and do not break feed parsing.
- Verify that products with variant-specific pricing list the correct price range or the lowest variant price according to the platform's specification.

---

### Bulk product editing (price changes, status)

- Select 10 products via checkbox in the admin product list and apply a bulk price update (e.g., set price to 29.99); verify all 10 products show the new price in the admin and on the frontend.
- Select 10 products and apply a bulk percentage price increase (+10%); verify each product's new price is correctly calculated and rounded according to the shop's rounding rules.
- Select 10 products and apply a bulk percentage price decrease; verify no product ends up with a negative price and an error is shown if the calculation would result in 0 or below.
- Select products and bulk-change status from "active" to "inactive"; verify the products immediately disappear from the frontend category pages and search results.
- Select products and bulk-change status from "inactive" to "active"; verify the products appear on the frontend.
- Apply a bulk action to 0 selected products (no checkbox ticked); verify the system shows a validation message ("Please select at least one product") rather than performing an empty operation.
- Select all products on the current page (e.g., 50) and apply a bulk category assignment; verify all selected products are reassigned to the new category.
- Attempt to apply a bulk price update that would set a price below the minimum allowed value; verify the system rejects the operation with a clear error.
- After a bulk edit, verify the change history / audit log records which admin user performed the bulk action, what was changed, and when.
- Perform a bulk status change to "deleted"; verify products are soft-deleted (not permanently removed) and existing orders referencing those products remain intact.
- Verify that bulk operations on large selections (500+ products) are processed via a background job with a progress indicator, rather than synchronously blocking the request.

---

### Data validation on import (report faulty rows)

- Import a CSV where row 15 has a malformed price and row 23 has a duplicate SKU; verify the error report identifies errors by row number, column name, and a human-readable description (e.g., "Row 15, column 'price': value 'abc' is not a valid number").
- Verify that valid rows in the same file are still imported when a subset of rows contains errors (partial success with error reporting), unless the system is configured for all-or-nothing transactional imports.
- Import a CSV with a `stock_quantity` column containing a decimal value (e.g., `5.5`); verify the system flags this as invalid (stock must be integer) rather than silently truncating.
- Import a CSV with a date field (e.g., `sale_end_date`) in an unrecognised format (e.g., `31/12/2025` when `YYYY-MM-DD` is expected); verify the row is flagged with a clear date-format error.
- Import a CSV with a URL field containing a malformed URL (e.g., missing scheme); verify the row is rejected with a URL-validation error.
- Import a CSV containing emoji characters in text fields; verify either correct UTF-8 storage or a clear rejection message — no silent data corruption.
- Verify that the error report is downloadable (e.g., as a CSV) listing all failed rows with their original data and the corresponding error reason, so the user can correct and re-import only the failed rows.
- Import a CSV where all rows are invalid; verify the system shows a comprehensive error report and imports zero rows, rather than silently completing.
- Verify the error report does not expose sensitive system internals (file paths, SQL errors, stack traces) in the user-facing output.
- Import a file containing 1 valid row and 999 invalid rows; verify performance is acceptable and the single valid row is imported (or held, depending on import mode), with 999 errors listed.

---

### Import of customer data / order data (migration)

- Import a customer data CSV with valid records; verify customers are created in the system with correct email addresses, names, and addresses.
- Attempt to import a customer CSV containing an email address that already exists in the database; verify the system applies the configured conflict strategy (skip/update/error) without creating duplicate accounts.
- Import customer data and verify passwords are never imported as plain text; the system should either force password reset on next login or accept only pre-hashed values in the documented format.
- Import a customer CSV containing a field with personally identifiable information (PII) in a column that should not hold PII (e.g., phone number in the `notes` field); verify access to the imported data is restricted per GDPR role permissions.
- Import an order data CSV for migrating historical orders; verify order totals, line items, statuses, and customer references are correctly associated.
- Import order data referencing customer IDs that do not exist in the system; verify the system either creates stub customers, skips the rows, or flags them with a clear "unknown customer" error.
- Import order data with order dates in the past; verify the dates are stored as provided and do not default to the import timestamp.
- Import a customer CSV with 100,000 rows; verify the import is queued, runs to completion, and does not exhaust server memory or timeout.
- After importing customer data, verify no welcome emails or password reset emails are unintentionally triggered during the migration import.
- Verify the import process logs the total number of records imported, the admin user who triggered the import, and the timestamp — for audit compliance.
- Import order data containing special characters in customer names (e.g., Chinese, Arabic, Cyrillic); verify correct UTF-8 storage and display in the admin order view.
- Attempt a customer import with a CSV where the `email` column contains invalid email formats; verify each invalid row is rejected with a specific validation error.

---

### Export of orders for accounting/ERP

- Export all orders from the last 30 days as CSV; verify the file contains all expected columns (order ID, date, customer name, billing address, line items, totals, VAT, payment method, status).
- Export orders and verify the total amounts in the export match the totals shown in the admin order list (no rounding discrepancies).
- Export orders and verify VAT amounts are broken down correctly per tax rate (e.g., 19% and 7% in separate columns if applicable).
- Apply a date range filter (e.g., Q1 2025) and export; verify only orders within that range appear in the file and the count matches the admin UI count.
- Filter by order status (e.g., "paid" only) and export; verify no orders with other statuses appear in the file.
- Export orders for accounting and verify coupon discount amounts are shown as a separate line item or column, not silently subtracted from product prices.
- Export orders where one order contains a partial refund; verify the export shows the original amount, the refund amount, and the net payable — not just the original amount.
- Export a large dataset (e.g., 50,000 orders); verify the export is offered as a download link (asynchronous generation) rather than a synchronous response that would time out.
- Open the exported CSV in Microsoft Excel (locale: German/Windows); verify the file does not display garbled characters for umlauts, and decimal separators are handled correctly.
- Open the exported CSV in LibreOffice Calc; verify the same data integrity as in Excel.
- Export orders and import the resulting file into a test instance of a common ERP (e.g., DATEV, Lexoffice); verify the import succeeds without format errors.
- Verify the export file does not contain full payment card numbers, CVV codes, or any raw payment credentials — only masked card info or payment method labels.
- Export orders and verify the exported file is generated fresh at the time of the request, not a cached copy from a previous export (to ensure the latest order status is included).
- Verify that the export function is restricted to admin users with the appropriate role/permission and is not accessible via a guessable URL without authentication.
