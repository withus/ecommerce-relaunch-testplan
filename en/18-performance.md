# 18. Performance / Load Times

---

### Homepage load time

- Measure homepage load time using Lighthouse (Chrome DevTools) on a cold cache; target: fully interactive in under 3 seconds on a mid-range mobile device (Moto G4 equivalent, 4G throttle).
- Run WebPageTest from a European test node (Frankfurt or Amsterdam) with Cable connection profile; target: Document Complete < 3.0s, Fully Loaded < 5.0s.
- Repeat Lighthouse audit 3 times and record the median score; Performance score must be >= 90 (desktop) and >= 75 (mobile).
- Verify that the homepage loads in under 2.0 seconds on a fast broadband desktop connection via GTmetrix (Vancouver or London node).
- Check that there are no render-blocking resources delaying the above-the-fold paint; Lighthouse "Eliminate render-blocking resources" audit must show 0 critical blocking resources or a combined blocking time < 150ms.
- Confirm total page weight is under 2 MB uncompressed / under 800 KB transferred for a typical homepage without video.
- Test with browser cache cleared (first visit) vs. warm cache (repeat visit); repeat visit load time should be at least 50% faster than first visit.

---

### Category page load time

- Measure category page (with product grid, filters, pagination) using Lighthouse on mobile simulation (4G throttle); target: Time to Interactive (TTI) < 4.0s.
- Run WebPageTest with a product category containing 24–48 product thumbnails; target: Fully Loaded < 5.5s on Cable.
- Verify lazy loading is active on product images below the fold: first 6–8 products should load eagerly, remainder must carry `loading="lazy"`.
- Test category page with active filter combination (3+ filters applied) to ensure filtered result set loads within 2.0s of filter interaction.
- Check category page performance with maximum products per page setting (e.g., 96 items); Fully Loaded must not exceed 8.0s on Cable.
- Confirm pagination or "Load More" triggers no full-page reload; only the product grid should re-render, measured via Network tab (XHR/fetch response < 500ms).
- Verify no cumulative layout shift caused by late-loading filter sidebar or product badges; CLS < 0.1 on category page.

---

### Product page load time

- Run Lighthouse audit on a product page with image gallery (6+ images), reviews section, and variant selector; target Performance score >= 85 (desktop), >= 70 (mobile).
- Measure LCP on the product page: the hero product image must be the LCP element and must render in < 2.5s on desktop (fast connection), < 4.0s on simulated 4G mobile.
- Verify product page total transfer size is under 1.5 MB on a typical product with standard image gallery (no video).
- Test product page load time for a configurable product with 50+ variants; variant selector must be interactive within 3.5s of navigation (TTI < 3.5s).
- Confirm product page images are served in WebP or AVIF format with JPEG/PNG fallback; check via Network tab that `.webp` or `.avif` variants are delivered to supporting browsers.
- Check product page performance on a slow 3G simulation (Lighthouse Slow 4G preset); page must be usable (interactive) within 10s.
- Measure page load time including review widget / third-party review embeds; the third-party script must not block first meaningful paint (defer or async loading required).

---

### Checkout load time

- Measure load time of each checkout step (address, shipping, payment, summary) using WebPageTest; each step must load in under 2.5s on a 4G connection.
- Run Lighthouse on the checkout page; Performance score must be >= 80 (desktop), >= 65 (mobile).
- Test checkout page TTI with all payment method widgets loaded (Stripe Elements, PayPal SDK, Klarna); TTI must be < 5.0s on desktop.
- Verify payment iframe/widget injection does not cause significant CLS; CLS for checkout page must be < 0.1 after all payment widgets load.
- Measure time from clicking "Proceed to checkout" to the first checkout step being interactive; target < 2.0s (measured via browser Performance timeline).
- Test checkout under a slow connection (simulate 3G in DevTools); verify the form is usable within 8s and does not appear broken while loading.
- Confirm order confirmation page (post-purchase thank-you page) loads within 3.0s including any tracking pixels or GA4 events firing.

---

### Time to First Byte (TTFB)

- Measure TTFB for homepage, category page, product page, and checkout via WebPageTest (Connection View waterfall); target: TTFB < 800ms for all key pages.
- Verify TTFB from a CDN edge node (if CDN is active) is < 200ms for static assets and < 400ms for cached HTML responses.
- Test TTFB for uncached (first-hit) requests vs. cached responses; uncached TTFB should be < 1.5s, cached TTFB < 200ms.
- Run `curl -o /dev/null -s -w "%{time_starttransfer}\n"` against all key page types to baseline server TTFB outside browser overhead; target < 600ms without CDN caching.
- Check TTFB during simulated load (10 concurrent users via k6 or Apache Bench); TTFB must not exceed 1.5s at p95 under moderate load.
- Verify that TTFB is not degraded by synchronous session initialization, cookie parsing, or database queries on every request; use server-side profiling (Blackfire, Tideways, or XHprof) to confirm no query runs > 100ms on the critical path.
- Check TTFB for API endpoints (product variants, price calculation, shipping costs): each API call must return first byte within 300ms under normal load.

---

### Largest Contentful Paint (LCP)

- Run Lighthouse on homepage, category page, and product page; LCP must be < 2.5s (Good threshold per Google) on desktop and < 4.0s on mobile 4G simulation.
- Use Chrome DevTools Performance panel to identify the LCP element; confirm it is the expected above-the-fold element (hero image or product image), not a fallback placeholder.
- Verify the LCP image is preloaded with `<link rel="preload" as="image">` and is not lazy-loaded; check page source for `fetchpriority="high"` attribute on the LCP image tag.
- Measure LCP in Chrome User Experience Report (CrUX) via PageSpeed Insights for the production domain; target: >= 75% of real-user page loads in "Good" range (< 2.5s).
- Test LCP after enabling and disabling CDN to quantify CDN impact; CDN must reduce LCP by at least 20% compared to direct origin for geographically distant users.
- Confirm LCP does not regress after deploying new features; establish a baseline LCP budget per page type (e.g., homepage <= 2.0s) and block deployments that exceed it by > 10%.
- Verify LCP is not blocked by custom web fonts; fonts must use `font-display: swap` or `optional`, and the LCP element must not be a text node blocked by font download.

---

### First Input Delay (FID) / Interaction to Next Paint (INP)

- Measure INP (replaces FID as Core Web Vital from March 2024) via PageSpeed Insights field data; target: INP < 200ms (Good threshold).
- Run Lighthouse lab measurement with Total Blocking Time (TBT) as proxy for FID/INP; target: TBT < 200ms (desktop), < 600ms (mobile).
- Use Chrome DevTools Performance tab to record main-thread activity during page load; identify any long tasks (> 50ms) and confirm they are broken up or deferred.
- Simulate user interactions (click on add-to-cart, filter selection, variant change) and measure response time via PerformanceObserver (INP API); each interaction should produce a visible response within 200ms.
- Test INP on the product configurator page where complex JS runs on option change; price update and image swap must complete within 200ms of user input.
- Verify third-party scripts (analytics, chat, marketing pixels) do not cause long tasks > 100ms during page interaction; use Lighthouse "Avoid long main-thread tasks" audit.
- Check that search autocomplete responds within 100ms of keystroke (perceived input delay); if server-side, debounce must be <= 200ms with a loading indicator shown immediately.

---

### Cumulative Layout Shift (CLS)

- Run Lighthouse on all key page types; CLS must be < 0.1 (Good threshold) on every page.
- Use WebPageTest filmstrip view to visually inspect layout shifts; identify any element that moves after initial render (ads, banners, late-loading images, consent banners).
- Verify all product images have explicit `width` and `height` attributes (or `aspect-ratio` CSS) to reserve space before loading; check via Lighthouse "Properly size images" and "Avoid large layout shifts" audits.
- Test CLS on the category page when the filter sidebar loads; sidebar must not push content or shift the product grid after initial render.
- Measure CLS on the checkout page as payment widgets (Stripe iframe, PayPal button) inject themselves; target CLS < 0.1 on checkout.
- Check that cookie consent banner and any promotional popups do not cause CLS > 0.05; banners must either be part of the initial render or use fixed/sticky positioning that does not displace content.
- Verify CLS on mobile when sticky add-to-cart bar appears; the bar must use `position: fixed` or `sticky` and must not shift page content downward.
- Run CLS measurement during navigation between pages (SPA or AJAX transitions); CLS must be < 0.1 per navigation event, not just on initial load.

---

### Image optimization (WebP, AVIF, lazy loading)

- Verify that all product images, category banners, and hero images are served in WebP format to Chrome and AVIF to supported browsers; confirm via Network tab (Response Headers: `Content-Type: image/webp` or `image/avif`).
- Check that JPEG/PNG fallback is provided for browsers without WebP/AVIF support (e.g., Safari < 14); test in Safari and confirm correct format is delivered.
- Confirm all images below the fold carry `loading="lazy"` attribute; inspect HTML source and verify absence of `loading="lazy"` on above-the-fold images (LCP image must not be lazy-loaded).
- Measure compression ratio: a 2000×2000px product image in WebP must be under 150 KB; a 400×400px thumbnail under 25 KB; validate via Network tab or GTmetrix waterfall.
- Run Lighthouse "Properly size images" audit; no image may be served more than 2× the display size (e.g., a 300px thumbnail must not fetch a 3000px source file).
- Check that responsive images use `srcset` and `sizes` attributes so mobile devices receive smaller image variants; confirm via DevTools Device Emulation that mobile fetches the correct breakpoint image.
- Verify image optimization pipeline (e.g., build tool, CDN, or server-side conversion) does not degrade image quality below acceptable level; conduct visual QA on 10 representative product images.
- Test placeholder/skeleton behavior: images must show a low-resolution placeholder or background color before loading, not a layout shift to empty space.

---

### CSS/JS minification

- Inspect all CSS files in the Network tab; confirm every `.css` file served in production has minification applied (no readable indentation, comments stripped).
- Inspect all JS files in the Network tab; confirm every `.js` file served in production is minified; use Lighthouse "Minify CSS" and "Minify JavaScript" audits, both must pass with 0 unminified files.
- Verify source maps are NOT publicly accessible in production (`.map` files must return 404 or be blocked); confirm via `curl https://example.com/assets/main.js.map`.
- Check that CSS and JS are combined into as few bundles as possible (code splitting allowed for lazy routes); no more than 3–5 CSS files and 5–8 JS files loaded on the critical path.
- Measure the total size of CSS transferred; target: < 100 KB (gzipped) of CSS on any page. Measure total JS transferred; target: < 300 KB (gzipped) for the initial bundle.
- Verify that unused CSS is eliminated via PurgeCSS or equivalent; Lighthouse "Reduce unused CSS" audit must show < 20 KB of unused CSS bytes.
- Confirm that unused JavaScript is not included in the initial bundle; Lighthouse "Reduce unused JavaScript" audit must show < 50 KB of unused JS bytes on initial load.

---

### HTTP/2 or HTTP/3

- Verify all assets (HTML, CSS, JS, images, fonts) are served over HTTP/2 or HTTP/3; check via Chrome DevTools Network tab (Protocol column: `h2` or `h3`).
- Test HTTP/2 server push or Early Hints for critical CSS/fonts; confirm preload headers are sent with the HTML response using `curl -I https://example.com` and checking for `Link: rel=preload` headers.
- Verify HTTP/2 multiplexing is active by confirming that multiple assets are fetched over a single connection (Connection ID column in DevTools should show same ID for multiple assets).
- Check that HTTP/3 (QUIC) is enabled and advertised via `Alt-Svc` header; run `curl -I --http3 https://example.com` to confirm HTTP/3 connectivity (if supported by hosting).
- Test that the CDN also serves assets over HTTP/2 or HTTP/3; inspect CDN asset requests in DevTools Protocol column.
- Confirm no assets fall back to HTTP/1.1 due to misconfiguration; any HTTP/1.1 connection should be flagged and investigated.
- Measure the waterfall improvement from HTTP/2 vs. HTTP/1.1 by running WebPageTest with and without HTTP/2; expect at least 30% fewer request queuing delays with HTTP/2.

---

### Gzip/Brotli compression

- Verify Brotli compression is active for all text-based assets (HTML, CSS, JS, SVG, fonts); check via `curl -H "Accept-Encoding: br" -I https://example.com` — Response Headers must include `Content-Encoding: br`.
- Confirm Gzip fallback is available for clients that do not support Brotli; test with `curl -H "Accept-Encoding: gzip" -I https://example.com` — Response must include `Content-Encoding: gzip`.
- Measure compression ratio: HTML documents should be compressed to < 30% of uncompressed size, CSS and JS to < 25%.
- Run WebPageTest and confirm the "GZIP Enabled" and "Compression" columns show passing status for all compressible resources.
- Verify that image files (JPEG, PNG, WebP) are NOT re-compressed by the server (images are already binary-compressed; double-compression wastes CPU without benefit).
- Check that font files (WOFF2) are served without additional compression headers (WOFF2 is already compressed); server must not apply gzip/brotli to WOFF2 files.
- Confirm compression is consistent across CDN edge nodes; test from at least 2 geographic locations via WebPageTest to ensure CDN does not strip compression headers.

---

### Browser caching correct

- Verify all static assets (CSS, JS, images, fonts) have a `Cache-Control: max-age` >= 31536000 (1 year) in production and use content-hash filenames for cache busting; confirm via DevTools Network tab Response Headers.
- Check that HTML responses have `Cache-Control: no-cache` or `max-age=0` with `must-revalidate` to ensure users always receive fresh HTML without stale content.
- Test cache behavior on repeat visit: load a page once, reload (Ctrl+R), and confirm static assets return status 304 (Not Modified) or load from disk/memory cache.
- Verify that API responses (product data, cart, checkout) are NOT cached by the browser; check for `Cache-Control: no-store` or `private` on AJAX endpoints.
- Confirm that user-specific pages (cart, account, checkout) are excluded from CDN caching; test by purchasing as User A and verifying User B does not see User A's cart via CDN cache.
- Run Lighthouse "Serve static assets with an efficient cache policy" audit; all static assets must have cache duration >= 1 year. Zero failures allowed.
- Test cache invalidation after a deployment: after updating a CSS file, confirm the new version loads without requiring a hard refresh (cache-busted URL must differ from the previous one).

---

### CDN working

- Verify CDN is active for all static assets; inspect CDN-specific response headers (e.g., `x-cache: HIT`, `cf-cache-status: HIT` for Cloudflare) in DevTools Network tab.
- Test CDN cache hit rate via CDN dashboard; target: >= 80% cache hit ratio for static assets in steady state.
- Run WebPageTest from multiple geographic locations (e.g., US East, EU, Asia); asset load times from CDN edge nodes must be < 100ms TTFB for cached static files.
- Verify CDN SSL termination is working correctly; all CDN URLs must respond with valid TLS and no mixed-content warnings.
- Test CDN failover/bypass: temporarily simulate CDN outage (or use WebPageTest with CDN disabled) and confirm the origin server can serve traffic directly as fallback within 10s.
- Confirm CDN does not cache POST requests or dynamic checkout/cart pages; test by placing an order and verifying the order confirmation page is not served from CDN cache to a second user.
- Check CDN cache purge mechanism works: after a product image update, manually purge the CDN cache and confirm the new image is delivered within 60 seconds.

---

### Database queries optimized

- Profile database queries on key pages (homepage, category, product page) using a query logging tool (Laravel Telescope, Debugbar, or MySQL slow query log); no page must generate more than 20 individual DB queries.
- Identify and eliminate N+1 query problems: category page with 24 products must not run 24+ individual queries for pricing, stock, or images; eager loading or batch queries required.
- Verify all frequently-used queries have appropriate database indexes; run `EXPLAIN` on the top 10 most frequent queries and confirm "type" is not "ALL" (full table scan) for tables > 1000 rows.
- Measure slow query log: no query may take longer than 100ms in normal operation; queries exceeding 200ms must be flagged and optimized.
- Test checkout total calculation query; confirm the cart totaling query (including discounts, VAT, shipping) runs in < 50ms.
- Check that full-text search queries (product search) complete in < 200ms for a catalog of 10,000+ products; use indexed search (MySQL FULLTEXT, Elasticsearch, or Algolia).
- Verify database connection pooling is configured; under 50 concurrent users, the application must not exhaust the DB connection pool (confirm via DB monitoring or `SHOW STATUS LIKE 'Threads_connected'`).

---

### Server response time

- Measure server response time (TTFB) for all key page types under zero-load conditions using `ab -n 10 -c 1` or `curl`; target < 300ms for cached pages, < 800ms for uncached dynamic pages.
- Run server response time test from the same data center region as the target market to exclude geographic latency; test must confirm application server processing time < 300ms.
- Check PHP-FPM (or equivalent) worker pool configuration; response time must not degrade when all workers are busy (configure queue or scale workers appropriately).
- Verify server response time for checkout confirmation (order processing including payment confirmation webhook handling); target < 2.0s end-to-end after the user clicks "Place Order".
- Monitor server response time during business hours via uptime tool (UptimeRobot, Pingdom, or Better Uptime); alert if p50 response time exceeds 1.0s.
- Test server response time for REST/GraphQL API endpoints called by the frontend; each API endpoint must return a complete response within 500ms at p99 under low load.
- Verify that server-side caching (Redis, Memcached, or OPcache) is active; confirm via a cache hit/miss counter that >= 70% of dynamic page requests are served from application cache.

---

### Load time under load (concurrent users)

- Run a k6 or Locust load test simulating 50 concurrent users browsing the shop (homepage, category, product page); median response time must remain < 2.0s and p99 < 5.0s throughout the test.
- Simulate 100 concurrent users adding to cart and proceeding to checkout; checkout page response time must remain < 3.0s at p95; no 500 errors permitted.
- Test checkout under peak load: simulate 20 simultaneous order submissions; all orders must complete successfully, no duplicate charges, no deadlocks, p99 response time < 10.0s.
- Perform a spike test: ramp from 0 to 200 concurrent users in 30 seconds, then back to 10; the application must not return more than 1% 5xx errors during the spike.
- Measure server resource utilization (CPU, memory, DB connections) during load test; CPU must not exceed 80% sustained; memory must not grow unboundedly (check for memory leaks).
- Test product search under load: 50 concurrent search queries must return results within 1.0s at p95.
- Verify auto-scaling (if applicable): when CPU exceeds 70% for > 60 seconds, new server instances must be provisioned and traffic distributed within 3 minutes.

---

### Third-party scripts impact

- Identify all third-party scripts loading on every page (Google Analytics, Meta Pixel, chat widget, Hotjar, etc.) via Lighthouse "Reduce third-party JavaScript" audit; record the total blocking time each contributes.
- Verify all non-critical third-party scripts are loaded with `async` or `defer` attribute; no third-party script may block HTML parsing or be loaded synchronously in `<head>`.
- Measure the impact of third-party scripts on TTI: run Lighthouse with all third-party scripts blocked (via DevTools Request Blocking) and compare TTI; TTI difference must be < 500ms, meaning third-party scripts add no more than 500ms to TTI.
- Test page load with a common ad blocker (uBlock Origin) active; the shop must remain fully functional (no JS errors, no broken layout) when third-party scripts are blocked.
- Verify Google Tag Manager (GTM) container is loaded asynchronously; confirm via Network tab that `gtm.js` does not appear in the critical rendering path.
- Measure chat widget load impact: Lighthouse "Third-party code" audit must show the chat widget contributing < 200ms TBT; if it exceeds this, implement lazy loading triggered by first user interaction.
- Run WebPageTest with "Block Ads" option enabled and compare Fully Loaded time; if blocking ads reduces load time by > 2 seconds, each blocked resource must be investigated and loaded more efficiently.
- Verify that consent management platform (CMP) script loads synchronously only the minimum necessary cookie-blocking code; the full CMP UI must load asynchronously after first paint.
