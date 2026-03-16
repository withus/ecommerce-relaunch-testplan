# 14. CMS / Content Pages

### Legal notice / imprint

- Verify the legal notice page is accessible via a direct URL (e.g., `/imprint` or `/legal-notice`) and via the footer link.
- Confirm all legally required fields are present: full company name, legal form, registered address, phone number, email address, registration court and number, VAT ID, and responsible person for editorial content.
- Verify the page contains no broken links and all referenced contact details are accurate and clickable (phone as `tel:`, email as `mailto:`).
- Check that the page does not use a redirect that bypasses the original URL (imprint must be one-click reachable from every page).
- Confirm the page loads without errors on mobile and desktop, with correct font size and readable contrast.
- Verify the imprint page is excluded from search indexing only if intentionally set to `noindex`; by default it should be indexable.
- Confirm the imprint link is present in the footer on every page of the shop, including checkout steps and account pages.
- Check that the imprint page has its own proper `<title>` and `<h1>` tag and is not a generic CMS placeholder.
- Verify the page renders correctly when JavaScript is disabled.

### Privacy policy

- Confirm the privacy policy page is reachable from the footer on every page and from the registration, checkout, and contact forms.
- Verify the policy includes all required sections: data controller identity, types of data collected, purposes of processing, legal bases (GDPR Art. 6), data retention periods, third-party data sharing, cookie usage, and user rights (access, deletion, portability, objection).
- Check that each third-party tool mentioned (e.g., Google Analytics, Meta Pixel, Stripe, PayPal, Klarna, newsletter provider) is individually listed with its own data processing description.
- Verify that the last-updated date on the privacy policy is current and reflects the live version.
- Confirm the privacy policy link in cookie consent banners and opt-in checkboxes points to the correct, live page and not to a staging URL.
- Check that the page is screen-reader accessible: headings are hierarchically structured, no information is conveyed by color alone.
- Verify that internal anchor links (table of contents) within the privacy policy work correctly.
- Confirm the page does not contain placeholder text (e.g., "[Insert Company Name]") from a template.
- Check that the privacy policy is not blocked in `robots.txt` and that the canonical URL is self-referential.

### Terms & conditions

- Confirm T&Cs are accessible via footer link, during checkout (inline link in checkbox), and via direct URL.
- Verify the checkbox in checkout is not pre-checked and requires explicit user interaction before placing an order.
- Check that the T&C link in the checkout checkbox opens in a new tab or a modal and does not navigate away from the checkout flow.
- Confirm the T&Cs cover all required areas: scope, contract formation, prices and payment, shipping, cancellation rights, warranty, liability, and governing law.
- Verify that a downloadable or printable version is available (e.g., PDF download link or browser print function works without layout breaks).
- Check that the page is properly versioned — if T&Cs change, existing orders should remain bound to the version accepted at time of purchase.
- Confirm the T&Cs page has a proper `<h1>` and logical heading structure for accessibility.
- Verify no outdated references (e.g., old company name, old payment methods, defunct legal references) are present in the text.
- Check that the page loads correctly on mobile with no horizontal overflow.

### Cancellation policy

- Confirm the cancellation policy page is linked in the checkout process (mandatory pre-order display) and in order confirmation emails.
- Verify the policy includes the full legal cancellation period (14 days in the EU), instructions for how to exercise the right (contact details, sample form), and consequences of cancellation.
- Check that a sample cancellation form is either displayed inline or available as a downloadable PDF on the page.
- Confirm the cancellation policy correctly addresses exceptions (e.g., custom/made-to-order products that are exempt from cancellation rights) if applicable.
- Verify the "Order with obligation to pay" button and T&C/cancellation policy checkboxes in checkout all link to the live, correct version.
- Check that the cancellation policy is not blocked by a login requirement or session gating.
- Confirm the page is accessible via a permanent, stable URL that does not change between deployments.
- Verify that returning the sample cancellation form via email or postal mail instructions are complete and accurate.

### Shipping information

- Verify the shipping information page lists all available shipping methods with their associated costs, estimated delivery times, and conditions (e.g., free shipping threshold).
- Check that information on the shipping page matches what is displayed during checkout (no discrepancies in prices or delivery times).
- Confirm special destinations are covered: islands, remote areas, PO boxes, parcel lockers, international shipping zones.
- Verify that delivery time estimates are clearly stated as business days, and any exceptions (e.g., public holidays, pre-order items) are noted.
- Check that all shipping carrier names and tracking process instructions are up to date (no defunct carriers listed).
- Confirm the page contains correct information for express and premium delivery options if offered.
- Verify the shipping page is linked in the cart and checkout, not only in the footer.
- Check that the page is indexable and has a correct SEO title and meta description.
- Confirm shipping cost tables are readable on mobile (no broken tables or clipped columns).

### Payment methods page

- Verify the payment methods page lists all actively available payment options with their logos, a brief description, and any applicable conditions (e.g., invoice only for returning customers, Klarna eligibility criteria).
- Confirm the payment methods shown on this page exactly match the methods available in the checkout — no method listed that is not functional.
- Check that each payment method entry includes relevant details: processing time, any additional fees, refund process.
- Verify payment method logos are correctly licensed for use and displayed without distortion.
- Confirm the page links to the relevant third-party payment provider's terms where legally required (e.g., Klarna BNPL disclosure).
- Check that the page is accessible from the footer and from within the checkout flow.
- Verify the page renders correctly when no user session is active (accessible to non-logged-in visitors).
- Confirm the page has correct Open Graph metadata so shared links render properly on social media.

### FAQ / frequently asked questions

- Verify the FAQ page contains questions organized in logical categories (e.g., Orders, Shipping, Returns, Payment, Products).
- Check that all internal links within FAQ answers point to valid, live pages (no 404s from FAQ anchor links).
- Confirm FAQ items use accordion or expandable panels that open and close correctly with mouse and keyboard (Enter/Space keys).
- Verify that FAQ structured data (FAQPage Schema) is implemented and validates without errors in Google's Rich Results Test.
- Check that the FAQ page has a functional in-page search or filter to find relevant questions quickly.
- Confirm that questions about cancellation, returns, and payment reference the correct policies and match the actual shop behavior.
- Verify the page is updated to reflect current shop capabilities (e.g., if express shipping was added, the FAQ mentions it).
- Check that frequently asked questions are consistent with information on the shipping page, T&Cs, and cancellation policy.
- Confirm the FAQ page has a correct canonical URL and is not duplicated across multiple paths.
- Verify the page is included in the XML sitemap.

### About us

- Confirm the About Us page is accessible from the footer and, where applicable, from the main navigation.
- Verify the page contains meaningful company information: founding story, values, team or location, USPs — no placeholder content.
- Check that all images on the page have descriptive alt text and load without broken image indicators.
- Confirm any embedded Google Maps or location widgets load correctly and are not blocked by cookie consent (or are correctly gated behind consent).
- Verify that team member photos or company imagery are optimized (WebP format, lazy loading for below-the-fold images).
- Check that any links to social media profiles, press mentions, or certifications are functional and open in a new tab.
- Confirm the page has a unique `<title>` tag and meta description, and is not a duplicate of the homepage or another page.
- Verify the page does not expose internal emails, personal phone numbers, or sensitive staff information publicly unless intentional.

### Contact page

- Confirm the contact page is accessible from the main navigation or footer on every page.
- Verify the page displays all relevant contact options: phone number, email address, postal address, and business hours.
- Check that the phone number is clickable on mobile (`tel:` link) and the email address is clickable (`mailto:` link).
- Confirm any embedded Google Maps or location map loads correctly and is cookie-compliant.
- Verify that social media links on the contact page point to the correct, active accounts and open in a new tab.
- Check the page includes business hours clearly and that time zones are specified if international visitors are expected.
- Confirm a link or button to the live chat widget is present if live chat is an available support channel.
- Verify the contact page is included in the XML sitemap and has a proper canonical URL.
- Check that the page loads correctly in all major browsers and on mobile without layout shifts.

### Contact form (validation + submission)

- Verify that all required fields (name, email, message) display clear error messages when left empty on submit.
- Confirm email field validation rejects malformed addresses (e.g., `user@`, `user@domain`, `@domain.com`).
- Check that the message field has a minimum length requirement and shows a counter or validation message when the minimum is not met.
- Verify spam protection is active: test honeypot field behavior and/or reCAPTCHA challenge triggers correctly without blocking legitimate users.
- Confirm form submission sends a confirmation email to the user with a copy of their message and an expected response time.
- Verify a shop notification email is sent to the correct internal email address on each submission.
- Check that successful submission displays a clear on-page confirmation message and does not leave the user on a blank or reloaded form.
- Confirm double-submit is prevented: clicking "Send" twice does not create two tickets or send two emails.
- Verify that the form is functional with JavaScript disabled (or displays an appropriate fallback).
- Check for XSS protection: submitting `<script>alert('xss')</script>` in any field should not execute in confirmation email or admin view.
- Confirm the form is fully keyboard-navigable and accessible via screen reader (all fields have `<label>` elements or `aria-label`).
- Verify subject line dropdown or category selector (if present) correctly pre-fills or routes the inquiry to the right department.
- Check that long text inputs (3,000+ characters) are handled gracefully without crashing or truncating silently.

### Blog / guides

- Verify the blog overview page displays a paginated list of articles with title, excerpt, category, publication date, and featured image.
- Check that the newest articles appear first by default when sorted by date.
- Confirm all featured images have descriptive alt text and are lazy-loaded for articles below the fold.
- Verify the blog overview page has a correct `<h1>` (e.g., "Blog" or "Guides") with article titles as `<h2>`.
- Check that the blog overview is included in the XML sitemap and has a proper canonical URL.
- Confirm pagination links (Previous/Next or numbered) work correctly and do not produce duplicate content (use `?page=2` or clean URLs, not multiple canonical pages pointing to the same content).
- Verify the blog is accessible without login and visible to search engine crawlers.
- Check that breadcrumbs on the blog overview show the correct path (e.g., Home > Blog).

### Blog single page

- Verify each blog article page displays the full content, author name (or brand name), publication date, and last-updated date.
- Confirm the article has exactly one `<h1>` matching the article title, with logical heading hierarchy (`<h2>`, `<h3>`) throughout the body.
- Check that in-article images have descriptive alt text, are properly sized and do not cause Cumulative Layout Shift (CLS).
- Verify that internal links within articles point to valid, live pages (no 404 links within blog content).
- Confirm structured data (Article or BlogPosting schema) is implemented and validates in Google's Rich Results Test.
- Check that social sharing buttons are functional and pre-fill the correct title and URL for each article.
- Verify the Related Articles or Recommended Reading widget at the bottom loads correctly and links to published articles.
- Confirm the article page has a unique meta title and meta description (not the same as the blog overview).
- Check that the estimated reading time (if displayed) is correctly calculated.
- Verify comments section (if applicable) has spam protection and does not display unapproved comments.
- Confirm the article URL is a clean slug (e.g., `/blog/how-to-choose-a-desk-chair`) and does not include dynamic parameters.
- Check that the article page is correctly indexed (no accidental `noindex` from CMS draft settings left over).

### Blog categories

- Verify each blog category page lists only articles belonging to that category, with no cross-category bleed.
- Confirm category pages have unique `<h1>` tags (e.g., "Category: Buying Guides") and unique meta descriptions.
- Check that pagination on category pages works correctly and does not break at the last page (no empty page shown after last article).
- Verify category URLs are clean and consistent (e.g., `/blog/category/buying-guides/`) with no numeric ID exposure.
- Confirm that creating a new article in a category immediately appears on the category page after publish (no stale cache issue).
- Check that empty categories either display a friendly "No articles yet" message or are redirected rather than showing a blank page.
- Verify breadcrumbs on category pages correctly show Home > Blog > [Category Name].
- Confirm category pages are included in the XML sitemap if they contain at least one published article.
- Check that category filter/navigation on the blog overview correctly activates and deactivates category filters without full page reload issues.

### Blog search

- Verify the blog search field accepts input and returns results filtered to blog content only (does not mix product results).
- Confirm search results display article title, excerpt, category, and date for each match.
- Check that the search highlights or bolds matching terms within the result excerpts.
- Verify that a "no results" state displays a helpful message (e.g., "No articles found for 'X'. Try searching for...") rather than a blank page.
- Confirm the search is functional with special characters (umlauts, accents, hyphens) without throwing errors.
- Verify the search URL (e.g., `/blog?q=desk+chair`) is shareable and reproducible — pasting it into a new browser tab returns the same results.
- Check that the blog search field is accessible via keyboard Tab navigation and submits on Enter key.
- Confirm empty search submission either shows all articles or displays an instructional message, not an error.
- Verify the blog search does not expose internal CMS draft articles or unpublished content in results.
- Check that search input is sanitized against XSS: entering `<script>alert(1)</script>` does not execute.

### Landing pages

- Verify all campaign landing pages are accessible at their intended URLs and return HTTP 200 (not redirecting to homepage or returning 404).
- Confirm landing page content matches the campaign it was built for (correct product, offer, and call-to-action).
- Check that each landing page has a unique `<title>` and meta description tailored to the campaign.
- Verify that CTA buttons on landing pages (e.g., "Shop Now", "Claim Offer") link to the correct product pages or category pages.
- Confirm landing pages are excluded from the main navigation and breadcrumbs if they are standalone campaign pages.
- Check that conversion tracking (e.g., Google Ads conversion tag, Meta Pixel) fires correctly on landing page visits and after CTA clicks.
- Verify landing pages with time-limited offers automatically redirect to an alternative page (e.g., homepage or relevant category) after the offer expires rather than showing expired content.
- Confirm landing pages pass Core Web Vitals thresholds (LCP under 2.5s, CLS under 0.1) as measured via PageSpeed Insights.
- Check that landing page forms (lead capture, discount code sign-up) validate fields and submit correctly.
- Verify that `noindex` is applied only to landing pages that should remain out of organic search results (e.g., paid campaign landing pages); other landing pages should be indexable.
- Confirm landing pages are responsive and the primary CTA is above the fold on mobile screens.
- Check that landing pages load correctly without the main site navigation (if designed as standalone pages), and still include a link back to the shop (e.g., the logo).

### 404 error page

- Verify a custom 404 page is displayed for any non-existent URL (e.g., `/this-page-does-not-exist`), not a generic server error page or blank white page.
- Confirm the 404 page returns an actual HTTP 404 status code — not 200 (soft 404), which would mislead search engines.
- Check that the 404 page includes the main site navigation (header and footer) so users can continue browsing.
- Verify the 404 page contains a search bar so users can search for the content they were looking for.
- Confirm the 404 page offers helpful navigation suggestions, such as links to the homepage, popular categories, or bestseller products.
- Check that the 404 page displays correctly on mobile and that all interactive elements (search, links) are functional.
- Verify the 404 page does not expose any technical error details, stack traces, or file paths.
- Confirm that broken internal links (links within the site pointing to non-existent URLs) are identified and resolved before launch.
- Check that the 404 page design matches the overall shop branding (logo, colors, fonts) and is not a default CMS template.
- Verify that personalization or recommendation widgets on the 404 page (e.g., "You might like these products") load correctly and do not cause JavaScript errors.
- Confirm images on the 404 page have alt text and load without breaking.
- Check that the 404 page includes a brief, friendly message explaining what happened (e.g., "This page has moved or no longer exists") rather than only the error code.

### Maintenance page

- Verify the maintenance page is served with an HTTP 503 status code (Service Unavailable) and includes a `Retry-After` header, not a 200 or 404.
- Confirm the maintenance page is accessible to all visitors during maintenance mode, including logged-in users (except whitelisted admin IPs).
- Check that admin users with whitelisted IPs can bypass the maintenance page and access the live site for testing.
- Verify the maintenance page displays an estimated return time or a message about the ongoing maintenance.
- Confirm the maintenance page matches site branding (logo visible) and includes a contact email for urgent inquiries.
- Check that search engines receive the 503 status and do not de-index pages during short maintenance windows.
- Verify that the maintenance page does not expose the site's technology stack, CMS version, or file structure.
- Confirm that after maintenance ends, the maintenance page is fully deactivated and no residual redirect or caching causes users to still see it.
- Check that the maintenance page itself loads without external dependencies that may also be unavailable during maintenance (no reliance on CDN-hosted CSS if the CDN is the cause of maintenance).
- Verify that automated monitoring and uptime alerts are paused or adjusted during planned maintenance windows to avoid false-alarm notifications.
