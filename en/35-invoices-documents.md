# 35. Invoices & Documents

### PDF invoice generation and download

- Place a completed order and verify that a PDF invoice is generated automatically without manual admin intervention.
- Navigate to the customer account order history, select a completed order, and confirm a "Download Invoice" button or link is present.
- Click the download link and verify the browser either downloads a `.pdf` file or opens a PDF viewer — not a blank page, 404, or HTML error.
- Verify the downloaded file is a valid, non-corrupt PDF (opens in Adobe Reader, browser PDF viewer, and third-party tools without errors).
- Verify the PDF renders all text, tables, and layout elements correctly — no truncated rows, overlapping text, or missing columns.
- Verify that the PDF filename follows a consistent, meaningful convention (e.g., `invoice-2024-00123.pdf`) rather than a random hash or generic name.
- Test PDF generation for an order with a single line item — verify layout does not have excessive whitespace or misaligned elements.
- Test PDF generation for an order with 20+ line items — verify content flows across pages correctly with repeated table headers on each page.
- Test PDF generation for an order containing a product with a very long name (100+ characters) — verify text wraps correctly and does not overflow outside the table cell or page boundary.
- Test PDF generation for an order where a coupon or discount was applied — verify the discount is clearly shown as a separate line item with the correct negative amount.
- Verify the PDF is generated in the correct language when the shop supports multiple languages (e.g., German invoice for a German customer, English for English).
- Verify that re-downloading the same invoice multiple times always produces an identical file (same content, same invoice number — no re-generation with changed data).
- Verify that the PDF generation process completes within an acceptable time limit (e.g., under 3 seconds) even for large orders.
- Test PDF download via the admin backend — verify admins can also download the invoice from the order detail view.
- Verify that PDFs are not publicly accessible via a guessable URL without authentication (e.g., `/invoices/123.pdf` should require login or a signed/token-protected URL).

### Delivery note generation

- Place an order and advance it to "shipped" status — verify a delivery note PDF is generated and accessible in the admin backend.
- Verify the delivery note contains order number, customer name, shipping address, list of items with quantities, and the shipping date.
- Verify the delivery note does NOT contain prices or payment information (delivery notes are not financial documents).
- Verify delivery note is accessible to the customer in their account if the shop exposes it, or confirm it is intentionally admin-only and document that decision.
- Test delivery note generation for an order with partial shipment — verify separate delivery notes are created per shipment, each referencing the correct subset of items.
- Verify the delivery note PDF renders correctly across all standard paper sizes (A4 at minimum) with no content cut off at margins.
- Verify the delivery note references the correct carrier and tracking number when available.
- Test delivery note for an order with a gift message — verify the gift message is either included or intentionally excluded based on business requirements.
- Verify the delivery note PDF filename is distinguishable from the invoice filename (e.g., `delivery-note-2024-00123.pdf` vs. `invoice-2024-00123.pdf`).
- Verify that generating a delivery note for a cancelled order is either blocked with an appropriate error or produces a clearly marked "cancelled" document.

### Correct mandatory information on invoice (tax number, VAT ID, address)

- Verify the invoice contains the seller's full legal company name (not just a trade name or domain).
- Verify the invoice contains the seller's complete street address including house number, postal code, city, and country.
- Verify the seller's tax number (Steuernummer) or VAT identification number (USt-IdNr.) is printed on the invoice — at least one is required under German tax law (§14 UStG).
- Verify the invoice contains the buyer's full name and billing address as entered during checkout.
- Verify the invoice contains a unique invoice number (see dedicated section below).
- Verify the invoice contains the invoice date (date of issue).
- Verify the invoice contains the delivery date or service date (Lieferdatum / Leistungsdatum) — this is a separate mandatory field from the invoice date.
- Verify each line item shows: item description, quantity, unit price (net), VAT rate applied, and line total.
- Verify the invoice shows the net total (subtotal before VAT), the VAT amount broken down by rate (e.g., 7% and 19% as separate lines if mixed rates apply), and the gross total.
- Verify invoices for B2B customers include the customer's VAT ID if provided during registration/checkout.
- Test an invoice where the customer provided a VAT ID for intra-EU delivery — verify the invoice correctly states "Reverse Charge" or "steuerfreie innergemeinschaftliche Lieferung" and shows 0% VAT.
- Test an invoice for a customer outside the EU — verify the invoice correctly states the applicable VAT treatment (export delivery, 0% VAT with correct legal reference).
- Verify the payment terms or payment method used are mentioned on the invoice (e.g., "Payment received via PayPal on 2024-03-15").
- Verify all monetary amounts use the correct currency symbol and decimal formatting for the invoice locale (e.g., `€ 19,00` in German, `€19.00` in English).
- Verify the invoice mentions the seller's bank details or indicates that payment has already been received, as appropriate.

### Invoice accessible in customer account

- Register an account, place an order, and verify the invoice appears in the order history within the customer account once the order status reaches the configured trigger point (e.g., "paid" or "shipped").
- Verify the invoice does NOT appear in the customer account before payment is confirmed — no invoice for pending or failed orders.
- Verify that a customer can only see their own invoices — logging in as Customer A and navigating to Customer B's order URL must return a 403/404, not the invoice.
- Verify the invoice list in the customer account is sorted by date descending (most recent first) by default.
- Test accessing the customer account on mobile — verify the "Download Invoice" button/link is accessible without horizontal scrolling and is touch-friendly (min. 44×44px tap target).
- Verify that deleting an account (GDPR deletion request) either removes the invoice download access or retains it for the legally required retention period (10 years under German commercial law) and clearly communicates this to the customer.
- Verify invoices remain accessible for all orders — not just the last 12 months — unless a documented retention policy limits earlier access.
- Verify that the invoice download link does not expire after a short time (e.g., a signed URL with a 5-minute expiry would break saved bookmarks).
- Verify the invoice is listed under the correct order when a customer has multiple orders (verify order number cross-reference).
- Verify that a guest order (no account) does not expose the invoice via a publicly guessable URL, and that the link sent via email is either token-protected or requires authentication.

### Send invoice via email

- Place an order and complete payment — verify an invoice PDF is automatically attached to the order confirmation email or a separate invoice email, per the shop's configured behaviour.
- Verify the invoice email is sent to the billing email address, not a potentially different shipping contact.
- Verify the email subject line clearly identifies it as an invoice (e.g., "Your invoice for order #2024-00123").
- Verify the attached PDF is not corrupt and opens correctly in standard email clients (Gmail, Outlook desktop, Outlook web, Apple Mail, Thunderbird).
- Verify the attached PDF file size is reasonable (typically under 500 KB for a standard invoice) — an excessively large attachment may indicate uncompressed images or rendering issues.
- Verify that resending an invoice from the admin backend sends the same invoice number, not a new one — the document must be identical to the original.
- Verify the invoice email is not flagged as spam by major email providers — check SPF, DKIM, and DMARC alignment for the sending domain.
- Test invoice email delivery for addresses with plus-sign aliases (e.g., `customer+shop@gmail.com`) — verify it is not rejected.
- Verify that when a customer updates their email address in their account, subsequent invoices go to the new address, but previously sent invoices are not re-sent.
- Verify the invoice email contains a fallback link to download the invoice from the customer account in case the attachment is blocked by corporate email filters.
- Verify no invoice email is sent for cancelled or fully-refunded orders where no taxable transaction occurred.

### Credit note document for returns

- Initiate a return for a completed order and verify a credit note (Gutschrift) PDF is generated when the refund is processed.
- Verify the credit note contains a unique document number that is sequential within the credit note number series (separate from invoice numbers).
- Verify the credit note references the original invoice number it is correcting (e.g., "Credit note for invoice #2024-00123").
- Verify the credit note shows the returned items with negative amounts (or as positive amounts clearly labelled as credit), matching the refunded quantities.
- Verify the credit note shows the correct VAT amounts being reversed — the same VAT rate(s) as the original invoice, applied to the refunded amounts.
- Verify the credit note shows the total credit amount (gross) matching the actual refund amount processed via the payment provider.
- Test a partial return — verify the credit note only covers the returned items and not the entire order, with correct pro-rata VAT calculation.
- Verify the credit note is accessible in the customer account under the relevant order.
- Verify the credit note is sent to the customer via email upon refund processing.
- Verify the credit note PDF contains all mandatory legal fields: seller address, buyer address, credit note date, original invoice reference, credit note number.
- Test a scenario where the return spans different VAT rates (e.g., a mixed cart with standard and reduced rate items) — verify the credit note correctly separates the VAT breakdown by rate.
- Verify no credit note is generated for an exchange where no money is refunded — or alternatively, verify a zero-value credit note is not sent unnecessarily.
- Verify the credit note is not marked as an invoice and cannot be confused with one (clear document type heading: "Credit Note" / "Gutschrift").

### Sequential and correct invoice numbers

- Place three consecutive orders and verify the invoice numbers are sequential with no gaps (e.g., `2024-00121`, `2024-00122`, `2024-00123`).
- Verify the invoice number format is consistent across all invoices — same prefix, same zero-padding length, same delimiter.
- Verify that a cancelled order before invoice generation does not consume an invoice number, leaving a gap in the sequence.
- Verify that a failed payment does not result in an invoice being generated with an invoice number that is then abandoned (creating a gap).
- Verify that if the shop uses annual numbering resets (e.g., `2024-00001` → `2025-00001`), the reset happens exactly at the year boundary and not mid-year.
- Test concurrent order placement (two orders submitted simultaneously) — verify both receive unique, non-duplicate invoice numbers (race condition test on the number generator).
- Verify that voiding or cancelling an invoice after issuance does NOT re-use that invoice number — cancelled invoice numbers must remain permanently retired, and a credit note must be issued instead.
- Verify invoice numbers are purely numeric or follow a fixed alphanumeric pattern — no random characters, no UUIDs used as invoice numbers (legal requirement in Germany: §14 Abs. 4 Nr. 4 UStG — eindeutige, fortlaufende Nummer).
- Verify that test orders created in a staging environment do not share the invoice number sequence with production (separate sequences or clearly different prefixes for test documents).
- Retrieve the last 50 invoice numbers from the database or admin export and run a gap-check script — confirm zero gaps in the sequence.
- Verify the invoice number is printed prominently on the PDF (not only stored in the database) so it is visually identifiable on the document.
- Verify that re-generating / re-downloading an existing invoice does not assign a new invoice number — the number is immutable once assigned.
- Verify that the invoice number displayed in the customer account, in the admin backend, and on the PDF document are identical — no discrepancy between storage and rendering.
