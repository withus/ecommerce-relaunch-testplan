# Data Migration & Validation (relaunch-specific)

---

### Product data fully migrated (title, description, prices, images)

- Count total products in source system (e.g., `SELECT COUNT(*) FROM products`) and compare against target system; counts must match exactly or variance must be documented and justified.
- Count total product variants/SKUs in source and target; verify the delta is zero or matches known intentional exclusions.
- For a random sample of 50 products (covering different categories, price ranges, and statuses), manually compare title, short description, long description, and meta description field by field between source and target.
- Verify that all HTML formatting in long descriptions (bold, bullet lists, tables) survived migration without corruption or entity-encoding artifacts.
- Verify that all product prices (regular price, sale price, tiered/volume prices) match source values to two decimal places; export both as CSV and run a diff.
- For every product with a sale price, verify the sale price is lower than the regular price and that sale start/end dates carried over correctly.
- Confirm all product images are accessible (HTTP 200) on the new system; run a batch URL checker against the full image URL list exported from the target.
- Verify image filenames or alt texts were migrated so no product displays a broken image placeholder.
- For products with multiple images, confirm image order/sequence matches the source.
- Confirm product status (active, inactive, draft, archived) migrated correctly; spot-check 10 inactive products to ensure they are not publicly visible.
- Verify product slugs/URLs are correct and match redirects or canonical references.
- Confirm that digital products (if any) have their downloadable file references intact.
- Verify product weight, dimensions, and shipping class fields migrated correctly for all physical products.
- Confirm product tax class assignments are preserved and result in the same tax calculation as in the source.

---

### Customer data correct (addresses, order history, password hashes)

- Compare total customer record count between source and target databases; counts must match.
- Export customer email list from source and target and run a diff; identify any missing or extra records.
- For a sample of 20 customers, verify all profile fields: first name, last name, email address, phone, date of birth (if stored), account creation date.
- Verify that each customer's default billing address and default shipping address migrated correctly, including all subfields (street, house number, postal code, city, country, company, address addition).
- For customers with multiple saved addresses, confirm the full address book transferred and the correct address is flagged as default.
- Confirm password hashes are present in the target system and that a representative set of test customers can log in with their existing passwords without a forced reset.
- Verify that password hashing algorithm compatibility was confirmed before migration; if algorithms differ, confirm the migration strategy (re-hash on first login, forced reset) is working.
- Check that no plaintext passwords appear anywhere in the migrated data or migration logs.
- Confirm customer account status (active, suspended, email-verified) migrated correctly.
- Verify that customers' newsletter subscription preferences (opt-in/opt-out) are correctly reflected in both the shop system and the connected email service provider.
- Confirm that customer group assignments (B2B, VIP, wholesale) migrated and result in correct pricing and access rights.
- Verify that the account creation date is preserved (not overwritten with the migration date).
- Check for duplicate customer records created during migration; run a query for duplicate email addresses.
- Confirm that customers registered but not yet verified (pending double opt-in) are handled consistently.

---

### Order history viewable and correct

- Compare total order count between source and target; counts must match.
- Verify all order statuses (paid, shipped, cancelled, refunded, etc.) migrated and display correctly in the customer-facing order history.
- For a sample of 30 orders (spread across time periods and statuses), compare all fields: order number, order date, line items, quantities, unit prices, discounts, shipping cost, VAT breakdown, and total.
- Confirm that order numbers retained their original values and did not get re-sequenced during migration.
- Verify that customers can see their historical orders in the account area immediately after logging in on the new system.
- Confirm that downloadable invoices (PDFs) attached to historical orders are still accessible, either via regeneration or file migration.
- Verify that shipment tracking numbers from historical orders are intact and that tracking links remain functional.
- For orders with multiple shipments, confirm all shipment records migrated.
- Check that guest orders are visible via the order status lookup tool (order number + email) on the new system.
- Confirm that cancelled orders with full refunds show correct refunded amounts.
- Verify that partially refunded orders show the correct remaining and refunded amounts.
- Check that orders with applied coupon codes still display the coupon code and discount amount in the order detail view.
- Confirm that B2B orders with net price display are still showing net prices in the order history for those customer groups.

---

### 301 redirects set up for all old URLs

- Export a complete list of all publicly indexed URLs from the old system using Google Search Console, Screaming Frog crawl, or XML sitemap; this becomes the redirect test inventory.
- Verify that every URL in the inventory returns HTTP 301 (not 302, not 200 with a redirect via meta-refresh, not 404).
- Confirm that each 301 redirect points to the correct corresponding new URL (product to product, category to category) and not just to the homepage.
- Test that redirect chains are not longer than one hop (old URL → new URL directly, not old URL → intermediate URL → new URL); chains longer than one hop incur unnecessary PageRank dilution and latency.
- Verify that redirect targets return HTTP 200 and are not themselves further redirects.
- Check that URLs with trailing slashes and without trailing slashes are handled consistently (one canonicalized to the other via 301).
- Test that uppercase URL variants redirect to lowercase equivalents via 301.
- Verify that old URL parameters (e.g., `?sort=price&page=2`) either redirect cleanly by stripping parameters or are mapped to the corresponding new filter/pagination pattern.
- Confirm that the old homepage URL (if it changed) redirects correctly, including `http://` and `https://` and `www` vs. non-www variants.
- Test redirects for at least the top 100 highest-traffic URLs identified in Google Analytics/Search Console.
- Verify that paginated category URLs (e.g., `/category/sofas/page/2/`) either redirect to the first page or to the corresponding new pagination URL.
- Confirm redirects are configured at the server/CDN level (`.htaccess`, Nginx config, or CDN rules) and not only via PHP/application code to ensure they fire even for non-routable requests.
- Use a bulk redirect checker tool (e.g., Screaming Frog, httpstatus.io batch) to test all redirects in one pass and export a pass/fail report.
- Verify that image URLs and asset URLs that may be linked externally also redirect or are preserved.

---

### SEO metadata migrated (title, description, canonical)

- Compare total count of pages with custom SEO titles in source vs. target; counts must match.
- For a sample of 50 pages (homepage, top category pages, top product pages, CMS pages), compare `<title>` tag content between old and new site character by character.
- Verify that meta descriptions migrated for all pages that had them, and that none were truncated or corrupted.
- Confirm that canonical URLs in `<link rel="canonical">` tags reflect the correct final URL on the new domain/URL structure.
- Check that canonical tags do not point back to the old domain after migration.
- Verify that hreflang tags (if multilingual) are correctly migrated and reference valid alternate language URLs.
- Confirm the XML sitemap on the new site lists all migrated pages with correct URLs and does not contain old-domain links or 301-redirect URLs.
- Verify robots.txt on the new site does not accidentally block crawling of product or category pages.
- Check that all structured data (Product schema, Organization schema, BreadcrumbList) that was present on the old site is present and valid on the new site; use Google's Rich Results Test on a sample of 10 pages.
- Verify that Open Graph tags (`og:title`, `og:description`, `og:image`) migrated correctly for product and category pages.
- Confirm that H1 tags contain the same (or semantically equivalent) text as on the source site for key landing pages.
- Check that no pages that were previously indexed have gained a `noindex` directive during migration.
- Submit the new sitemap to Google Search Console and confirm no crawl errors or sitemap parsing errors appear within 48 hours post-migration.
- Monitor Google Search Console for a coverage report within the first two weeks; the ratio of indexed pages should not drop by more than 5% compared to the pre-migration baseline.

---

### Category structure correctly mapped

- Compare the total number of categories and subcategories in source vs. target; counts must match.
- Verify the full category tree hierarchy (parent–child relationships) is preserved; export both trees as JSON/CSV and diff them.
- For each category, confirm that the category name, slug/URL, and description text migrated correctly.
- Verify that every product is assigned to the same categories as in the source system; run a query to find products with a different number of category assignments.
- Confirm that category images and banners migrated and display correctly.
- Check that category sort order (manual or rule-based) is preserved so product listings appear in the expected sequence.
- Verify that category landing page SEO content (intro text, SEO title, meta description) migrated completely.
- Test navigation by clicking through the full category tree on the new site to confirm no dead links or missing subcategory pages.
- Confirm that filter attributes (facets) are correctly associated with each category (e.g., "Material" filter appears only in relevant categories).
- Verify that custom category layout assignments (e.g., a category using a full-width template) migrated correctly.
- Check that any category-level discounts or promotional rules still apply after migration.

---

### Customer reviews migrated

- Compare total review count in source vs. target database; counts must match.
- For a sample of 20 products, compare visible review count and average star rating between source and new site.
- Verify that each review's author name, star rating, review title, review body, and submission date migrated correctly.
- Confirm that review images (if the old system supported them) migrated and are accessible.
- Verify that reviews are correctly associated with the right products after migration; check that no review appears on the wrong product page.
- Confirm that reviews marked as "verified purchase" retain that status after migration.
- Check that reviews that were hidden/pending moderation in the source system are also hidden on the new system and did not become publicly visible.
- Verify that the aggregate average rating displayed on product pages and in search results is recalculated correctly from migrated data.
- Confirm that review structured data (AggregateRating schema) on product pages reflects the migrated ratings.
- Test that the review sort and filter functionality (sort by newest, highest, lowest) works correctly with the migrated review data.

---

### Coupon/discount codes still valid

- Export the complete list of active coupon codes from the source system and import/verify in the target system.
- Compare total coupon code count (active + inactive) between source and target.
- For each migrated coupon, verify: code string (case-sensitivity), discount type (percentage/fixed/free shipping), discount value, minimum order value, usage limit per code, usage limit per customer, applicable products/categories, and expiry date.
- Apply 10 representative coupon codes in a test checkout on the new system and confirm the calculated discount matches the expected value.
- Test a coupon that has already been used the maximum number of times; confirm it is rejected with the correct error message.
- Test an expired coupon and confirm it is rejected.
- Test a coupon restricted to specific product categories; confirm it applies only when qualifying items are in the cart.
- Confirm that usage counters (how many times a code has been redeemed) migrated correctly and are not reset to zero.
- Verify that single-use codes that have already been used are marked as consumed on the new system and cannot be reused.
- Test that automatically applied discounts (e.g., 10% off above €100) work correctly on the new system without requiring a code entry.

---

### Cart contents of existing customers (if possible)

- Identify the number of customers who had items in saved carts on the old system (query the abandoned cart / saved cart table); document the count.
- Verify that migrated cart data is associated with the correct customer accounts (no cart cross-contamination between customers).
- Test that a customer whose cart was migrated sees their items upon first login to the new system.
- Confirm that the migrated cart items still point to valid, active products; flag items where the product no longer exists or the variant is discontinued.
- Verify that prices displayed in the recovered cart reflect the current prices on the new system (not stale prices from the migration snapshot), with a clearly visible update notice if prices changed.
- Confirm that cart-level coupon codes that were applied at cart-save time are re-validated on the new system rather than silently accepted or rejected without user feedback.
- Test that a customer logging into the new system with an empty current cart but a migrated saved cart is prompted to restore the cart or that restoration is automatic, per the intended UX.
- Verify that guest cart sessions (cookie-based) were not expected to migrate (they cannot persist across domains); confirm this limitation is documented and that affected users see an empty cart with a clear explanation or a prompt to search for their previous items.

---

### Record count comparison: source vs. target

- Define and document the complete list of entity types to compare: products, product variants/SKUs, product images, categories, category-product assignments, customers, customer addresses, orders, order line items, order status history entries, invoices, shipments, reviews, coupons, coupon usage records, and any other migrated tables.
- For each entity type, run `SELECT COUNT(*)` on source and target and record the counts in a migration verification spreadsheet; the spreadsheet must be signed off before go-live.
- For any count discrepancy, investigate and either correct the migration or document the reason (e.g., duplicate source records intentionally deduplicated, test records excluded).
- Compute a row-level checksum for critical tables (orders, order_items, customers) using `MD5(GROUP_CONCAT(...))` or an equivalent technique; compare source and target checksums to detect silent data corruption.
- Verify foreign key referential integrity in the target database: every order must reference an existing customer, every order line item must reference an existing product, every review must reference an existing product and customer.
- Run a query to detect orphaned records: order line items without a parent order, addresses without a parent customer, etc.
- Verify that auto-increment IDs or UUID sequences on the target are set beyond the highest migrated value to prevent ID collisions on new inserts post-migration.
- Compare file system asset counts: number of product image files on source storage vs. target storage; verify the count and total file size match (or document intentional exclusions such as thumbnails that will be regenerated).
- Schedule a second record count check 24 hours post-migration to detect any ongoing sync issues if a dual-write or incremental migration strategy was used.
- Archive the completed verification spreadsheet and checksums as part of the migration sign-off documentation, with timestamps and the name of the person who executed and approved each check.
