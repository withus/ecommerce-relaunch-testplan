# 17. SEO

### Title Tags on All Pages

- Verify that every page (homepage, category, product, CMS, checkout, account) has a `<title>` tag present in the HTML `<head>`.
- Confirm title tag length is between 50 and 60 characters for all key landing pages; flag anything below 30 or above 70 characters.
- Check that the homepage title includes the primary brand keyword and shop name (e.g., "Brand Name — Online Shop for [Product Category]").
- Verify product page titles follow a consistent template, e.g., "[Product Name] — [Category] | [Brand]".
- Confirm category page titles include the category name and a differentiating keyword (e.g., "[Category Name] — Buy Online | [Brand]").
- Check that no two pages share an identical title tag (duplicate title audit via Screaming Frog or similar crawler).
- Confirm CMS pages (About Us, FAQ, Contact) each have unique, descriptive titles.
- Verify checkout and account pages (cart, login, my-account) use `noindex` or have non-keyword-stuffed titles that won't appear in SERPs.
- Confirm title tags do not contain special characters that render incorrectly (`<`, `>`, `&` without HTML entities).
- Verify the `<title>` renders correctly after JavaScript execution (if titles are rendered client-side, test with JS disabled to confirm fallback or SSR).

### Meta Descriptions on All Pages

- Verify that every indexable page has a `<meta name="description">` tag present.
- Confirm meta description length is between 150 and 160 characters; flag anything under 70 or over 170 characters.
- Check that meta descriptions for product pages include the product name, a key benefit, and a soft call-to-action (e.g., "Order now", "Free shipping from €X").
- Verify category meta descriptions summarize the category's offering without keyword stuffing.
- Confirm no two pages share an identical meta description (crawl-based duplicate check).
- Check that meta descriptions do not contain double quotes (breaks the HTML attribute value) unless properly encoded.
- Verify meta descriptions are present and unique for all blog/guide pages.
- Confirm that auto-generated meta descriptions (e.g., from product short description) are truncated cleanly and do not cut off mid-word.
- Check that the meta description matches the visible page content — no misleading summaries.
- Verify that pages marked `noindex` still have meta descriptions (for consistency if the noindex directive is ever removed).

### H1 Structure (Exactly One Per Page)

- Crawl all pages and confirm each has exactly one `<h1>` tag; flag any page with zero or more than one H1.
- Verify the H1 on product pages matches (or closely mirrors) the product title displayed visually.
- Confirm the H1 on category pages is the category name, not a generic label like "Products" or "Items".
- Verify the homepage H1 represents the primary value proposition or brand name.
- Check that H1 text is not hidden via CSS (`display:none`, `visibility:hidden`, `opacity:0`, or positioned off-screen).
- Confirm the H1 is not the same text as the `<title>` tag (minor duplication issue) — verify there is value in slight variation.
- Verify H1 tags are not wrapped inside anchor tags.
- Check H1 is rendered in the HTML source (not only injected via JavaScript without SSR/pre-rendering).

### Heading Hierarchy (H1 > H2 > H3)

- Crawl all key page types and verify the heading hierarchy never skips levels (e.g., no H3 immediately after H1 without an H2 in between).
- Confirm H2 headings on product pages represent meaningful subsections (e.g., "Product Description", "Specifications", "Customer Reviews").
- Verify H2/H3 headings on category pages reflect filter groups, SEO copy sections, or subcategory names.
- Check that decorative or non-content headings (e.g., widget titles in sidebar) use appropriate levels and do not disrupt the main content hierarchy.
- Confirm blog/guide pages use a logical H1 → H2 → H3 structure matching the content outline.
- Verify that no heading tags are used purely for visual styling purposes (bold, large text) — presentational styling should use CSS classes on `<p>` or `<span>` instead.
- Run an automated heading-structure audit on the top 20 most-visited pages using a tool like Screaming Frog or axe-core.

### Canonical URLs

- Verify every indexable page includes a `<link rel="canonical" href="...">` tag in `<head>`.
- Confirm that product pages with URL parameters (e.g., `?color=red&size=L`) point their canonical to the clean base URL.
- Check that paginated category pages (page 2, 3...) carry a self-referencing canonical (or point to page 1 only if that strategy is intentional — document the decision).
- Verify that HTTP and HTTPS versions of a page have the canonical pointing to the HTTPS URL.
- Confirm that `www` and non-`www` variants both canonicalize to the preferred domain.
- Check that product variant URLs canonicalize to the main product URL.
- Verify that canonicals are absolute URLs (including protocol and domain), not relative paths.
- Confirm that no page has a canonical pointing to a 404 or redirecting URL.
- Check that CMS pages with trailing-slash and non-trailing-slash versions resolve to one canonical.
- Run a cross-check: canonical URL in HTML `<head>` must match the URL in the XML sitemap for each page.

### Hreflang Tags (Multilingual)

- If the shop is multilingual, verify all language versions of each page include bidirectional `hreflang` annotations (e.g., German page points to English version, English page points to German version).
- Confirm the `x-default` hreflang value is set on the appropriate fallback page.
- Verify hreflang attribute values use ISO 639-1 language codes and ISO 3166-1 alpha-2 country codes where appropriate (e.g., `de-DE`, `en-GB`, `en-US`).
- Check that hreflang URLs are absolute and do not contain session IDs or tracking parameters.
- Confirm that all URLs referenced in hreflang tags return a 200 status code (no pointing to redirected or 404 URLs).
- Verify hreflang annotations are consistent across all language versions of the same page (no orphaned references).
- Test that Google Search Console does not report hreflang errors after the sitemap is submitted.
- If the shop is single-language, confirm no hreflang tags are present (unnecessary noise).

### XML Sitemap Present and Correct

- Verify `sitemap.xml` (or sitemap index) is accessible at `https://domain.com/sitemap.xml`.
- Confirm the sitemap returns HTTP 200 with Content-Type `application/xml`.
- Verify the sitemap is referenced in `robots.txt` via the `Sitemap:` directive.
- Check that the sitemap includes all key indexable pages: homepage, category pages, product pages, blog posts, key CMS pages.
- Confirm the sitemap does NOT include URLs that return 301 redirects, 404 errors, or are marked `noindex`.
- Verify `<lastmod>` dates are accurate (not all set to the same static date, not set to a future date).
- Check that the sitemap does not exceed 50,000 URLs or 50 MB per file; if it does, confirm a sitemap index is used.
- Validate the sitemap XML syntax via Google Search Console ("Test sitemap") or an online validator.
- Submit the sitemap to Google Search Console and Bing Webmaster Tools; confirm no errors are reported.
- Verify sitemap includes image sitemaps (via `<image:image>` extensions) for product image discoverability.

### Robots.txt Correct

- Verify `robots.txt` is accessible at `https://domain.com/robots.txt` and returns HTTP 200.
- Confirm the file uses plain-text format and does not accidentally block Googlebot or all crawlers (`Disallow: /` on `User-agent: *` must NOT be present on production).
- Check that checkout, cart, account, and internal search result URLs are disallowed (`Disallow: /checkout`, `/cart`, `/account`, `/search`).
- Verify admin and backend URLs are disallowed and not crawlable.
- Confirm the `Sitemap:` directive points to the correct sitemap URL.
- Check that robots.txt does not reveal sensitive directory structures (admin panel paths, internal API endpoints).
- Verify the file is present in the root domain and not accidentally on a subdomain or subdirectory.
- Test that blocked URLs are not indexed by searching `site:domain.com/checkout` in Google.
- Confirm there are no syntax errors (no wildcard patterns not supported by all crawlers that could cause unintended behavior).

### 301 Redirects from Old Site

- Compile the full list of URLs from the old site (via sitemap export, crawl, or Google Search Console) and verify each important URL has a 301 redirect to the equivalent new URL.
- Verify redirects return HTTP status 301 (permanent), not 302 (temporary) or other codes.
- Confirm redirect chains are no longer than one hop: old URL → new URL directly (no old URL → intermediate URL → new URL).
- Check that no redirect loops exist (URL A → URL B → URL A).
- Verify high-traffic URLs from Google Search Console (top landing pages) all have valid 301 redirects.
- Confirm that 301 redirects preserve the URL path where possible (e.g., `/old-category/old-product` → `/new-category/new-product`).
- Test redirects using curl or an HTTP header checker to confirm status codes server-side, not relying solely on browser behavior.
- Verify that redirected URLs are excluded from the XML sitemap (only the destination URLs should be in the sitemap).
- Check that Google Search Console shows no significant drop in crawled pages or indexed URLs post-launch (monitor for 2–4 weeks).
- Confirm category page redirects, brand page redirects, and filter URL redirects are all handled, not only product pages.

### No 404 Errors on Important URLs

- Crawl the site with Screaming Frog (or equivalent) and export all 4xx responses; triage any 404 on pages with inbound internal links.
- Verify that the top 50 pages by organic traffic (from Google Search Console pre-launch) all return HTTP 200 post-launch.
- Check all internal navigation links (menu items, breadcrumbs, footer links, related product links) resolve to 200 status.
- Verify that all product URLs listed in the XML sitemap return 200 (not 404 or 301).
- Confirm that out-of-stock or discontinued product pages either redirect to the category or display a custom "unavailable" page (not a generic 404).
- Verify that the custom 404 error page itself loads with a genuine HTTP 404 status code (not a soft 404 returning 200).
- Check that the 404 page includes helpful navigation: search bar, links to top categories, link to homepage.
- Monitor Google Search Console "Coverage" report after launch for any spike in "Not Found" errors.
- Verify that blog/guide post URLs from the old site do not 404 (either redirect or migrate content).
- Confirm API endpoints and feed URLs used by Google Shopping or other integrations do not return 404.

### Image Alt Texts

- Crawl all product images and verify every `<img>` tag has a non-empty `alt` attribute.
- Confirm alt text for product images is descriptive and includes the product name (e.g., `alt="Red Leather Sofa 3-Seater — Model XYZ"`), not generic labels like "product image" or "img1.jpg".
- Verify that decorative images (icons, dividers, backgrounds) use `alt=""` (empty alt for presentational images).
- Check that image file names follow a descriptive naming convention (e.g., `red-leather-sofa-3-seater.jpg` not `IMG_4587.jpg`).
- Verify that category banner images have descriptive alt text referencing the category.
- Confirm logo image alt text contains the brand name.
- Check that CMS page images (blog, guides, About Us) all have contextual alt text.
- Run an automated accessibility scan (e.g., axe, WAVE) to catch any missing or empty alt attributes on key pages.
- Verify that dynamically loaded images (lazy loaded, JS-rendered) also carry correct alt attributes in the rendered DOM.
- Confirm alt texts are unique across images where content differs (not all product images on a page sharing the same alt text).

### Internal Linking

- Verify that all main category pages are reachable from the homepage within one click via the navigation menu.
- Confirm each product page links to its parent category via breadcrumbs and the category navigation.
- Check that product pages include "Related Products" or "Customers Also Viewed" sections with working internal links.
- Verify that blog/guide articles link to relevant product or category pages using descriptive anchor text (not "click here").
- Confirm footer contains links to top-priority pages: key categories, CMS pages (FAQ, About, Contact, Shipping), and the sitemap.
- Check that anchor text for internal links is descriptive and includes relevant keywords (e.g., "View all leather sofas" not "See more").
- Verify there are no orphan pages (pages with zero internal links pointing to them) for any indexable page.
- Crawl the site and confirm no internal links return 404, 301, or other non-200 responses.
- Verify that the number of internal links per page does not become excessive (crawl budget concern: flag pages with 200+ internal links).
- Confirm that pagination links, "Load More" buttons, and infinite scroll allow search engines to discover all paginated content.

### Breadcrumbs

- Verify breadcrumbs are present on all product pages, category pages, and blog posts (not required on homepage or checkout).
- Confirm breadcrumb structure accurately reflects the URL hierarchy (e.g., Home > Sofas > Leather Sofas > Product Name).
- Verify every breadcrumb segment is a clickable link except the current page (last item).
- Confirm breadcrumb structured data (`BreadcrumbList` schema) is implemented on all pages that show breadcrumbs.
- Validate breadcrumb structured data via Google's Rich Results Test (`https://search.google.com/test/rich-results`).
- Check that breadcrumbs update correctly when a product belongs to multiple categories (canonical breadcrumb path matches canonical URL).
- Verify breadcrumbs render correctly on mobile (no overflow or truncation that makes them unusable).
- Confirm breadcrumb links do not use JavaScript `onClick` handlers without a valid `href` — they must be crawlable.
- Check that Google Search Console shows breadcrumb rich results without errors after indexing.

### Structured Data (Organization, Product, Review, FAQ, Breadcrumb)

- Implement and validate `Organization` schema on the homepage: includes `name`, `url`, `logo`, `contactPoint`, `sameAs` (social media profiles).
- Implement and validate `Product` schema on all product pages: includes `name`, `description`, `image`, `sku`, `brand`, `offers` (with `price`, `priceCurrency`, `availability`, `url`).
- Verify `Product` schema `availability` value is updated dynamically based on stock status (e.g., `InStock` vs. `OutOfStock`).
- Implement and validate `Review` / `AggregateRating` schema on product pages: includes `ratingValue`, `reviewCount`, `bestRating`.
- Confirm `AggregateRating` is only present when at least one review exists (Google penalizes fake or empty rating markup).
- Implement and validate `FAQPage` schema on FAQ pages and product pages with FAQ tabs: each Q&A pair uses `Question` and `Answer` types.
- Implement and validate `BreadcrumbList` schema on all pages with breadcrumbs (see Breadcrumbs section above).
- Validate all structured data via Google's Rich Results Test and via Schema.org Validator (`https://validator.schema.org/`).
- Check Google Search Console "Enhancements" reports for structured data errors or warnings after indexing.
- Confirm no structured data is hidden from users but visible only to crawlers (cloaking violation).
- Verify structured data is present in the HTML source or rendered DOM, not only in a JavaScript variable that never gets executed.
- Check that prices in `Product` schema match the displayed prices (price mismatch causes manual actions from Google).

### Open Graph Tags

- Verify every indexable page has `og:title`, `og:description`, `og:url`, `og:image`, and `og:type` meta tags.
- Confirm `og:title` is set (can match the `<title>` tag or be a social-optimized variant).
- Confirm `og:description` is present and between 100 and 200 characters for optimal social preview display.
- Verify `og:image` URLs are absolute, return HTTP 200, and point to an image with at least 1200×630 pixels for optimal display across social platforms.
- Confirm `og:image` for product pages shows the product's main image (not a generic logo).
- Verify `og:url` matches the canonical URL of the page.
- Confirm `og:type` is set appropriately: `website` for homepage, `product` for product pages, `article` for blog posts.
- Test OG tag rendering using Facebook Sharing Debugger (`https://developers.facebook.com/tools/debug/`) for the homepage, a product page, and a blog post.
- Verify that OG tags render correctly after cache purge (Facebook caches OG tags; use the "Scrape Again" function).
- Confirm `og:site_name` is set to the shop's brand name on all pages.
- Check that no OG tag contains unclosed quotes or encoding errors that could break the preview.

### Twitter Cards

- Verify every indexable page has `twitter:card`, `twitter:title`, `twitter:description`, and `twitter:image` meta tags.
- Confirm `twitter:card` is set to `summary_large_image` for product and blog pages (provides a larger preview image in the tweet).
- Verify `twitter:title` is 70 characters or fewer (Twitter truncates longer titles in card previews).
- Confirm `twitter:description` is 125 characters or fewer for optimal display.
- Verify `twitter:image` URLs are absolute, return HTTP 200, and meet Twitter's minimum image size (300×157 pixels; recommended 1200×628 pixels).
- Confirm `twitter:site` is set to the shop's Twitter/X handle (e.g., `@brandname`).
- Validate Twitter Card implementation using the Twitter Card Validator (`https://cards-dev.twitter.com/validator`).
- Check that Twitter Card tags do not duplicate OG tag values unnecessarily when the content differs (e.g., optimized description length).
- Verify Twitter Card tags are present on key page types: homepage, product pages, blog posts, key category pages.

### PageSpeed / Core Web Vitals (LCP, FID, CLS)

- Run PageSpeed Insights (`https://pagespeed.web.dev/`) for the homepage, a category page, and a product page — both mobile and desktop.
- Verify Largest Contentful Paint (LCP) is under 2.5 seconds on mobile and desktop.
- Confirm Interaction to Next Paint (INP) is under 200 milliseconds (replaces FID as of March 2024).
- Verify Cumulative Layout Shift (CLS) score is under 0.1 on all key page types.
- Check that hero images and above-the-fold product images are not lazy loaded (they must be loaded eagerly to avoid LCP delay).
- Confirm hero images use `<link rel="preload">` or are served with high fetch priority.
- Verify that web fonts do not cause layout shifts (use `font-display: swap` or `optional` and size font containers correctly).
- Confirm no third-party scripts (chat widget, analytics, ad pixels) block rendering or cause significant LCP delay.
- Check Google Search Console "Core Web Vitals" report shows "Good" URLs outnumbering "Poor" URLs for both mobile and desktop.
- Verify CLS is not caused by dynamically injected banners, cookie consent bars, or chat widgets that push content down on load.
- Run field data checks via CrUX (Chrome User Experience Report) in addition to lab data from PageSpeed Insights.

### Mobile-Friendliness

- Run Google's Mobile-Friendly Test (`https://search.google.com/test/mobile-friendly`) for the homepage, a product page, and a category page; all must pass.
- Verify the viewport meta tag is present: `<meta name="viewport" content="width=device-width, initial-scale=1">`.
- Confirm no content is wider than the screen (horizontal scrolling must not exist on any page type on 375px viewport).
- Check that tap targets (buttons, links) are at least 48×48 CSS pixels with adequate spacing between them.
- Verify text is legible without zooming on mobile (minimum 16px font size for body text).
- Confirm form inputs on mobile trigger the correct keyboard type (`type="email"`, `type="tel"`, `type="number"`).
- Check that Google Search Console "Mobile Usability" report shows zero errors post-launch.
- Verify that mobile-specific redirects (e.g., `m.domain.com`) are not used; responsive design must serve the same URL to all devices.

### URL Structure (Clean URLs)

- Verify all product URLs follow a logical, readable pattern (e.g., `/[category]/[product-slug]` not `/index.php?pid=12345`).
- Confirm category URLs are hierarchical and readable (e.g., `/sofas/leather-sofas/` not `/cat/45/sub/12/`).
- Check that URL slugs use lowercase letters, hyphens as word separators, and no underscores (Google treats hyphens as word separators, underscores join words).
- Verify that URLs do not contain unnecessary parameters, session IDs, or tracking tokens.
- Confirm URL slugs are derived from the page title/product name and contain a primary keyword.
- Verify that changing a product's name does not automatically change its URL slug (slug should remain stable after initial creation to avoid broken links).
- Check that URLs are not excessively long (under 100 characters recommended; Google truncates very long URLs in SERPs).
- Confirm special characters (umlauts, accents, spaces) in product names are properly URL-encoded or replaced with ASCII equivalents in slugs.

### Trailing Slashes Consistent

- Define the canonical trailing-slash convention for the site (trailing slash or no trailing slash) and apply it consistently across all URLs.
- Verify that accessing a URL with and without a trailing slash results in a 301 redirect to the canonical form (e.g., `/category/product` redirects to `/category/product/` or vice versa).
- Confirm the XML sitemap uses the canonical trailing-slash format for all URLs.
- Verify canonical tags use the canonical trailing-slash form.
- Check internal links consistently use the canonical trailing-slash form (no mix of `/product` and `/product/` linking to the same page).
- Confirm the homepage URL is handled correctly (`/` is the homepage; no redirect loop between `domain.com` and `domain.com/`).
- Crawl the site and verify there are no chains: non-canonical trailing slash → redirect → canonical form (one-hop maximum).

### Index/Noindex Correctly Set

- Verify that all primary landing pages (homepage, categories, products, blog posts) return `index, follow` in the `<meta name="robots">` tag or via the `X-Robots-Tag` HTTP header.
- Confirm the following page types are set to `noindex`: checkout pages, cart, login, registration, order confirmation, internal search result pages (with URL parameters), account pages, and thank-you pages.
- Verify that `noindex` pages are also excluded from the XML sitemap.
- Confirm that staging or development environments are blocked from indexing (`noindex` on all pages OR `Disallow: /` in robots.txt — do not rely on password protection alone).
- Check that paginated pages beyond page 2 (or further) have deliberate `index` or `noindex` treatment documented and applied consistently.
- Verify that thin or duplicate content pages (filtered URLs with minimal content change) are `noindex` or canonicalized.
- Confirm no manually `noindex`-ed page in the old site remains `noindex`-ed in the new site if it should now be indexed.
- Check Google Search Console "Coverage" → "Excluded" → "Excluded by 'noindex' tag" to confirm only intended pages are excluded.

### Pagination (rel=next/prev or Alternatives)

- Note: Google officially deprecated `rel=next`/`rel=prev` in 2019 — verify the chosen pagination strategy is documented and intentional.
- Recommended strategy: each paginated page (`/category/?page=2`) has a self-referencing canonical, is set to `index`, and contains unique content (distinct product set).
- Alternative acceptable strategy: set paginated pages beyond page 1 to `noindex` and ensure all products are discoverable via internal links (e.g., a "View All" page or filter navigation).
- Verify that paginated URLs are crawlable (not blocked by robots.txt or JavaScript-only navigation).
- Confirm paginated category pages are included in the sitemap if the chosen strategy is to index them.
- Check that the first paginated page (`/category/`) differs from `/category/?page=1` — both should resolve to the same canonical URL to avoid duplication.
- Verify "View All" pages (if used) do not cause performance issues due to loading hundreds of products at once.
- Confirm that Google Search Console does not show a large volume of "Discovered — currently not indexed" paginated URLs, which could indicate crawl budget issues.
- Test that pagination links (Previous / Next page buttons) are standard `<a href>` links, not JavaScript-only click handlers.
