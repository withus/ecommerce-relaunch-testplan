# 47. Images & Media

### Broken image detection (missing product images)

- Open the browser DevTools Network tab, filter by "img", and navigate to every major page type (homepage, category page, product page, cart, checkout). Verify zero requests return HTTP 404 or HTTP 410.
- Run an automated crawler (e.g., Screaming Frog, broken-link-checker) across the full sitemap and assert the count of broken image URLs is 0.
- Simulate a missing image by temporarily renaming a product image file on the server. Verify the browser renders a defined fallback image (e.g., a "no-image-available" placeholder) rather than a broken icon.
- Verify that server-side image existence is validated at upload time: attempt to add a product without uploading an image and confirm the fallback placeholder is assigned automatically, not a blank `src`.
- In the admin backend, navigate to the media library and confirm a bulk "broken image audit" or equivalent check reports no unresolved missing files.
- Check that product images are referenced by absolute or root-relative URLs, not relative paths that break when viewed from sub-routes (e.g., `/produkt/kategorie/artikel`).
- After a product is deleted, verify its image file is either retained for CDN cache purposes or properly removed, and that no other page still references that URL producing a 404.

---

### Alt text for all product images present and descriptive

- Using an accessibility audit tool (axe DevTools, WAVE, or Lighthouse Accessibility), scan the homepage, a category page with 12+ products, and a product detail page. Assert zero "missing alt attribute" violations.
- Inspect the raw HTML of a product list and verify each `<img>` tag has a non-empty `alt` attribute that contains the product name plus at least one descriptive qualifier (e.g., color, material, angle) — not just `alt="product"` or `alt="image1.jpg"`.
- For decorative images (background textures, spacer images, icon-only buttons), verify `alt=""` is used and `role="presentation"` or `aria-hidden="true"` is set, so screen readers skip them.
- Navigate the product gallery using a screen reader (NVDA + Firefox, or VoiceOver + Safari). Confirm each gallery thumbnail announces a meaningful description of the image, not the filename.
- Open the sitemap XML or product feed and cross-check that `<image:caption>` or the corresponding alt value is populated for Google Image Search indexing.
- Check auto-generated alt text for variants: when a user selects a different color swatch, verify the gallery image `alt` attribute updates to reflect the selected variant (e.g., "Sofa Luna in Dark Grey, front view").
- Verify that CMS-uploaded images in editorial content (blog posts, landing pages) also have non-empty, descriptive alt text — not auto-filled filenames like `DSC_0042.jpg`.

---

### Lazy loading correct (above-the-fold NOT lazy loaded)

- On the homepage on a desktop viewport (1920×1080) and mobile viewport (375×812), identify all images visible in the initial viewport without scrolling. Verify none of these carry `loading="lazy"` or `data-src` deferred-loading attributes.
- Verify that the hero banner image (LCP candidate) has `loading="eager"` (or no `loading` attribute) and is included in a `<link rel="preload" as="image">` in the `<head>`.
- Scroll down slowly on a category page with 24+ products and confirm that off-screen product images are loaded only as they approach the viewport (verify in Network tab — requests appear only when the user scrolls near them).
- Disable JavaScript and reload a category page. Confirm that `<noscript>` fallback `<img>` tags exist for lazy-loaded images so products remain visible without JS.
- Verify the Intersection Observer threshold is tuned correctly: images should begin loading when approximately 200–300 px from the viewport edge, not only when they enter the viewport (to avoid visible pop-in on fast scroll).
- Test on a throttled connection (Chrome DevTools → 3G Slow): confirm above-the-fold images load fully before DOMContentLoaded, while below-the-fold images remain deferred.
- In Lighthouse, run a performance audit and confirm no LCP element has a `loading="lazy"` attribute flagged under "Avoid lazy loading largest contentful paint image".

---

### Image format optimization (WebP/AVIF with fallback)

- Inspect the HTML source of a product image on the product detail page and verify the markup uses `<picture>` element with source format order: AVIF first, WebP second, JPEG/PNG as final `<img>` fallback — e.g.:
  ```html
  <picture>
    <source srcset="product.avif" type="image/avif">
    <source srcset="product.webp" type="image/webp">
    <img src="product.jpg" alt="...">
  </picture>
  ```
- In Chrome (supports AVIF + WebP), open DevTools Network tab, filter images, and verify the browser requests `.avif` files for product images. Confirm status 200 and correct `Content-Type: image/avif` response header.
- In Safari 13 (supports WebP but not AVIF in older versions) or a browser with AVIF disabled, verify the browser falls back to `.webp` — not `.avif` — and images display correctly.
- In a browser or environment where WebP is disabled (simulate via `Accept` header manipulation or an old browser), verify the final fallback `<img src="product.jpg">` is served and renders correctly.
- Verify AVIF file sizes are at least 30–50% smaller than the equivalent JPEG at comparable visual quality. Check a sample of 10 product images and compare byte sizes in the Network tab.
- Confirm that image optimization is applied at upload time (server-side conversion pipeline) and not only at build time — i.e., newly uploaded product images also receive AVIF/WebP variants.
- Validate that `Content-Type` response headers match the actual file format served. A file named `.webp` must return `Content-Type: image/webp`, not `image/jpeg`.
- Confirm that CDN caches format variants separately using `Vary: Accept` header, so AVIF is not served to a browser that sent `Accept: image/jpeg` only.
- Test srcset with multiple resolutions: verify the `<source>` elements include a `srcset` with at least 3 size descriptors (e.g., `480w, 800w, 1200w`) to serve correctly sized images per device, not a single large file to all viewports.

---

### Product gallery: zoom, swipe, fullscreen

- On a product detail page on desktop, hover over the main product image. Verify a zoom effect activates (inline magnification or separate zoom pane) and the high-resolution source loads only on hover, not on initial page load.
- Verify the zoomed image is served at a resolution at least 2× the display size (e.g., if displayed at 600 px, the zoom source should be ≥1200 px wide) and is in WebP or AVIF format.
- On mobile, swipe left and right through the product gallery. Verify swipe gesture transitions are smooth (no layout jank), images transition to the correct adjacent image, and the thumbnail indicator updates accordingly.
- Test keyboard navigation of the gallery: using Tab to focus the gallery, then arrow keys to cycle through images. Confirm focus is visible and the active thumbnail is highlighted.
- Click or tap the fullscreen / lightbox trigger. Verify the overlay opens, the image fills the available screen area, and closing via Escape key, close button, or backdrop click returns focus to the trigger element.
- In fullscreen/lightbox mode, verify swipe navigation (mobile) and keyboard arrow navigation (desktop) work between images, and the image count indicator (e.g., "3 / 7") is correct.
- Verify that when a product variant is selected (e.g., color swatch), the gallery updates to show variant-specific images. The thumbnail strip should scroll/highlight to the variant's first image.
- Confirm the gallery does not trigger CLS: the image container has explicit `width` and `height` attributes (or `aspect-ratio` CSS) so the layout space is reserved before the image loads.
- Test with only one image: verify zoom and swipe controls degrade gracefully (no swipe arrows rendered, no broken carousel state).
- Verify touch targets for gallery navigation arrows on mobile are at least 44×44 px per WCAG 2.5.5.

---

### Video embedding on product pages

- Navigate to a product page with an embedded video. Verify the video player renders correctly and does not produce a JavaScript console error.
- Confirm the video does not autoplay with sound; verify `autoplay` attribute is absent or accompanied by `muted` to comply with browser autoplay policies.
- On a slow connection (3G throttle), verify the video poster image (thumbnail) displays before the video loads, preventing a blank/black area that could cause CLS.
- Click play on the video. Verify playback starts, progress bar is functional, mute/unmute controls work, and fullscreen mode opens correctly on both desktop and mobile.
- Verify the video is embedded via `<iframe>` (YouTube/Vimeo) using facade / lazy-load pattern: the actual iframe and its third-party scripts must not load until the user clicks the play button, reducing initial page weight.
- Check that the video `<iframe>` has a descriptive `title` attribute for screen reader accessibility (e.g., `title="Product demonstration video – Sofa Luna"`).
- Confirm the video element does not block or overlap the "Add to cart" button or other primary CTAs, especially on tablet and mobile viewports.
- Verify that structured data for the product page does not incorrectly mark the video as the product image (`image` property in Product schema should reference static images, not video thumbnails).

---

### 360-degree product view (if applicable)

- If a 360° viewer is present, verify it loads only after user interaction (click/tap) to avoid loading all frame images (~36–72 images) on initial page load.
- Drag/swipe the 360° viewer across the full rotation cycle and confirm all frames render without gaps, broken images, or flickering.
- Verify that frame images use WebP format and are served at an appropriate resolution (e.g., 800 px width per frame) — not raw high-resolution files.
- Test keyboard interaction: with the 360° viewer focused, confirm left/right arrow keys rotate the product, with visible focus indicator.
- Verify a descriptive `aria-label` is present on the 360° viewer container (e.g., `aria-label="360-degree product view, use arrow keys to rotate"`) for screen reader users.
- On mobile, confirm pinch-to-zoom within the 360° viewer does not conflict with browser zoom or page scroll, and that two-finger scroll still scrolls the page.
- Confirm that when 360° is unavailable (e.g., viewer JS fails to load), a standard product image gallery is shown as fallback — not a broken widget.

---

### Image size consistency in product lists

- Navigate to a category page in grid view and verify all product card images render at identical dimensions — no cards with taller or shorter images that distort the grid alignment.
- Switch to list view and verify product images are uniformly sized in the list layout.
- Verify that images are constrained using CSS `object-fit: cover` (or equivalent) so portrait and landscape images both fill the defined container without distorting aspect ratio or leaving whitespace gaps.
- Add a product with an unusually tall image (e.g., 3:4 ratio) and one with an unusually wide image (e.g., 16:9 ratio) to the same category. View the category page and confirm both render at identical card height.
- On mobile (375 px viewport), verify product card images in the grid resize correctly and maintain consistent proportions across all screen widths tested (375, 414, 768 px).
- Confirm images in the cart line-item display, order confirmation page, and email templates are also consistently sized — not stretched or squashed.
- Verify that the CLS score is 0 for the category page image grid by checking that image containers have defined `width` and `height` or `aspect-ratio` values before image load completes.

---

### Placeholder/skeleton during image loading

- On a throttled connection (3G Slow via DevTools), load a category page and verify a skeleton screen or low-quality image placeholder (LQIP) is visible in each product card image slot before the full image loads — no blank white boxes or layout shifts.
- Confirm the skeleton placeholder dimensions exactly match the final image dimensions, so no CLS occurs when the image replaces the placeholder (measure CLS score in Lighthouse; target < 0.1).
- Verify that the dominant-color or blurred LQIP placeholder is generated at upload time and embedded inline (as a base64 data URI or CSS background-color) so it displays instantly with zero additional network requests.
- Test the product detail page main image: confirm a placeholder of the correct size appears immediately while the full image loads, preventing the page from reflowing vertically.
- Confirm placeholders have `aria-hidden="true"` or are otherwise excluded from the accessibility tree so screen readers do not announce "loading" states.
- With JavaScript disabled, verify that placeholders do not remain permanently — the `<noscript>` fallback must render the actual `<img>` element directly.
- After the image loads, verify the placeholder fade-out transition (if any) is smooth and does not cause a repaint affecting surrounding text or CTA buttons.
