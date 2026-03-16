# Visual Regression Testing

### Screenshot comparison before/after code changes

- Set up Percy (`@percy/cli`) or BackstopJS (`backstop.json`) baseline snapshots on a known-good build; store baseline images in version control so they travel with the branch.
- Configure Percy with `percy snapshot` calls in the CI step that runs after every pull-request build; assert that no snapshot exceeds a 0.2% pixel-diff threshold (Percy default) before the PR can be merged.
- In BackstopJS set `"misMatchThreshold": 0.1` for critical pages (homepage, PDP, cart, checkout) and `"misMatchThreshold": 0.5` for less critical CMS pages; document threshold rationale in `backstop.json` comments.
- Integrate Chromatic (`chromatic --project-token=...`) for Storybook component snapshots; require Chromatic approval as a required GitHub status check so no UI-component change reaches `main` unreviewed.
- Run a "before" snapshot suite on the `main` branch and an "after" snapshot suite on the feature branch in the same CI pipeline step; diff results are posted automatically as a PR comment with thumbnail previews.
- Cover at minimum these URL paths per snapshot run: `/` (homepage), `/[category]` (category listing), `/[category]/[product]` (PDP), `/cart`, `/checkout/address`, `/checkout/payment`, `/checkout/confirmation`, `/my-account`, `/search?q=test`.
- Store failed diff images as CI artifacts (e.g., `artifacts: paths: backstop_data/bitmaps_test/`) so developers can download and inspect them without re-running locally.
- Add a manual "approve baseline" CI job that can be triggered to update baselines after intentional design changes; require two-person approval to prevent accidental baseline overwrites.
- Write a smoke-test that verifies the snapshot runner itself produced the expected number of snapshots (e.g., assert `backstopjs test --config=backstop.json` exits 0 and the report JSON contains exactly N scenarios); fail the build if fewer snapshots than expected were captured (indicates a crashed page).
- Test that screenshot comparison runs within a time budget (e.g., < 5 minutes for 20 pages) by checking CI step duration; flag if it grows beyond threshold to prevent pipeline slowdown.
- Verify that the comparison tool correctly ignores dynamic content (timestamps, countdown timers, live stock counters) by configuring `hideSelectors` (BackstopJS) or `percy-ignore` CSS class on those elements before taking snapshots.
- Validate that the CI job fails with exit code 1 and posts a meaningful error message when a regression is detected, so developers are not silently misled by a passing build.

### Consistency across different page types (PDP, PLP, cart, checkout)

- Create one BackstopJS scenario per page type with viewport sizes `[375, 812]` (mobile), `[768, 1024]` (tablet), `[1440, 900]` (desktop); verify that shared components (header, footer, breadcrumbs) render pixel-identically across all page types.
- Capture PDP, PLP, cart, and checkout in a single Percy build so Percy's "Group" feature can visually diff the same component (e.g., the cart icon in the header) as it appears on each page type side-by-side.
- Write an automated check that the header height in pixels is the same on PDP, PLP, cart, and checkout (extract via `document.querySelector('header').getBoundingClientRect().height` in a Playwright snapshot script); fail if values differ by more than 2 px.
- Verify the footer layout is consistent: compare footer screenshots from PDP and checkout; any difference (e.g., checkout hides footer links) must be an intentional, documented decision.
- Test the product card component as rendered on PLP, search results, recently-viewed widget, and cart cross-sell widget; assert card image dimensions and price alignment match across all contexts.
- Capture the "Add to cart" / CTA button in all its states (default, hover, loading, disabled, sold-out) on PDP and PLP and compare with Chromatic; ensure button dimensions and colors are identical on both page types.
- Run a page-type consistency check after every sprint by comparing the current snapshot set against a reference set stored in a `visual-baseline/` Git LFS repository; report any new per-page-type divergence.
- Verify trust badges and payment-method icons appear in the same position and size on product pages and at the checkout footer; pixel-diff threshold: 0.1%.
- Test that breadcrumb styling (font, color, separator character, spacing) is identical on PDP, PLP, and CMS pages by comparing isolated breadcrumb-component screenshots.
- Automate a "grid alignment" check on PLP: use Playwright to screenshot the first row of product cards, then assert via image analysis that card top-edges align within 1 px tolerance.
- Assert that the sticky "Buy" bar on PDP does not visually overlap with the header on any viewport by screenshotting after scrolling 300 px and inspecting the resulting diff.

### Font rendering and typography

- Capture full-page screenshots of the homepage and a long-form CMS page using Percy with `percyCSS: "* { animation: none !important; transition: none !important; }"` to prevent rendering jitter; compare against baseline.
- Test font loading: disable the web-font CDN (via Playwright `route.abort()`) and verify the fallback font does not cause text overflow or layout shift (CLS > 0.1 is a failure).
- Verify heading hierarchy visually: screenshot all H1–H4 headings on PDP and assert that each level is visually distinct (larger/bolder/different color) using a pixel-diff comparison with the approved design reference.
- Check font sizes match the design system: use Playwright to read `getComputedStyle(el).fontSize` for body text, product title, price, and CTA button text; assert values match the design token values (e.g., body 16 px, H1 32 px).
- Run a cross-browser typography test with BackstopJS on Chrome, Firefox, and Safari (via Playwright browser engines); pixel-diff threshold for text-heavy pages: 0.5% to accommodate sub-pixel rendering differences.
- Test that line-height and letter-spacing are applied consistently: capture a product description paragraph at 1440 px and 375 px; assert the ratio of rendered text height to container height stays within ±5% of the baseline.
- Verify that long product names (>60 characters) do not overflow their containers or cause text truncation that hides price information; inject a 90-character test product name and compare the screenshot.
- Check German-language special characters (Umlauts: ä, ö, ü, Ä, Ö, Ü, ß) render correctly and do not become replacement characters or question marks; use Percy on a page containing all Umlauts.
- Test that `font-display: swap` does not produce a visible flash of unstyled text (FOUT) in screenshots; compare first-paint screenshot (font not loaded) with fully-loaded screenshot to ensure layout shift is < 1 line of text.
- Validate that CTA button text never wraps to two lines at any standard viewport; inject longest expected button label (e.g., "Jetzt kostenlos in den Warenkorb legen") and screenshot at 320 px width.
- Verify print stylesheet typography: use Playwright's `page.emulateMedia({ media: 'print' })`, screenshot, and compare against the print-design reference; check invoice and order-confirmation pages specifically.

### Color consistency and brand compliance

- Define brand color hex values in a shared constants file (e.g., `brand-colors.json`); write a Playwright test that reads `getComputedStyle` for all primary CTA buttons, links, and headings and asserts the values match the brand constants exactly.
- Use Percy with the `--snapshot-flags` option to capture hover states on primary buttons, secondary buttons, and text links; compare against design-system reference screenshots with 0% tolerance for brand-color elements.
- Test that sale/discount price color (typically red) is consistent across PDP price, PLP card price, cart line item, and order confirmation; screenshot all four locations in a single Percy build and compare.
- Verify out-of-stock badge color and "Only X left" urgency badge color are consistent across PLP cards, PDP, and cart; pixel-diff threshold: 0% for status-indicator colors.
- Check that trust badge colors (e.g., Trusted Shops yellow, SSL-lock green) are not altered by global CSS rules; capture isolated screenshots of each badge and compare against the official badge asset.
- Run a WCAG color-contrast scan (axe-core via Playwright) alongside visual snapshot tests; fail the build if contrast ratio for any text-on-background combination falls below 4.5:1 (AA) or 3:1 for large text.
- Test dark mode (if implemented): enable `prefers-color-scheme: dark` in Playwright via `page.emulateMedia({ colorScheme: 'dark' })` and capture snapshots; compare against dedicated dark-mode design baselines.
- Validate that promotional banners (hero banner, sale ribbon, free-shipping bar) use exactly the approved colors by pixel-diffing with 0% threshold on isolated banner component screenshots.
- Verify gradient backgrounds (if used) are consistent: BackstopJS `misMatchThreshold: 0.05` on gradient areas; ensure no banding artifacts introduced by CSS changes.
- Test that CSS custom-property color overrides (e.g., white-label themes) do not bleed into the default theme; run visual regression on the default theme after loading a secondary theme and switching back.
- Capture the complete color palette as rendered in the living style guide / design-system page; diff this reference page on every deploy to detect accidental color regressions introduced by CSS specificity changes.

### Icon/button consistency

- Create Storybook stories for every icon variant (size: 16/24/32 px, color: default/hover/active/disabled) and publish to Chromatic on every CI run; require Chromatic approval for any story change before merging.
- Capture isolated screenshots of all icon-button states (cart icon, wishlist heart, account icon, search icon) at 1x and 2x pixel density (retina); compare with Percy at 0% threshold to catch sub-pixel rendering issues.
- Verify icon alignment within buttons: write a Playwright test that checks `getBoundingClientRect()` of the icon element vs. the button element and asserts the icon is horizontally and vertically centered within ±1 px.
- Test that SVG icons scale correctly: screenshot the same icon at 16 px, 24 px, and 32 px; run BackstopJS with `selectors: ['[data-testid="icon-cart"]']` for isolated comparison.
- Validate icon color states are defined via CSS (not inline SVG fill attributes) so that dark-mode and theme overrides work; use Playwright to assert `getComputedStyle(svgElement).fill` matches the current color token, not a hardcoded value.
- Check button width consistency: all primary CTA buttons on PDP must be the same width (or full-width on mobile); Playwright assertion: `button.offsetWidth` equals container width at 375 px.
- Test button loading-spinner icon: trigger the "Add to cart" action, screenshot the button while in loading state, and compare with the approved spinner design; Percy threshold: 0% for the button component.
- Verify that icon-only buttons (no visible text label) have `aria-label` attributes set and the aria-label text matches the icon's visual meaning; combine Playwright accessibility check with visual snapshot.
- Test disabled button state: disable the "Checkout" button by emptying the cart; screenshot and assert background-color changes to the approved disabled-state gray and opacity drops to 0.5.
- Validate that all payment-method icons (Visa, Mastercard, PayPal, Klarna, Apple Pay, Google Pay) on the checkout page render without distortion (no stretching); Playwright assertion: `img.naturalWidth / img.offsetWidth` ratio between 0.9 and 2.2 (normal to 2x retina).
- Run a cross-browser icon-rendering test using Playwright on Chromium, Firefox, and WebKit; pixel-diff threshold for icon-focused snapshots: 1% to tolerate minor anti-aliasing differences between engines.
- Detect accidentally swapped or missing icons: maintain a JSON manifest of `{ route: '/cart', expectedIcons: ['icon-trash', 'icon-plus', 'icon-minus'] }` and write a Playwright test that asserts each expected icon is present in the DOM and visible.

### Responsive breakpoints visual check

- Define the canonical breakpoint set in a shared test-config file: `[320, 375, 390, 414, 768, 834, 1024, 1280, 1440, 1920]` widths; run BackstopJS with all these viewports for homepage, PDP, PLP, and checkout.
- Verify hamburger-menu appears at the correct breakpoint: Playwright test asserts that `.hamburger-menu` is `display: none` at 1025 px and `display: flex` at 1024 px (or whatever the configured breakpoint is); screenshot both states.
- Test layout reflow at 768 px: a two-column product grid on desktop must reflow to one column on mobile; screenshot at 769 px and 767 px and compare with BackstopJS to confirm the breakpoint triggers exactly at 768 px.
- Capture screenshots at 320 px width (minimum supported) for all primary user flows (homepage → category → product → cart → checkout); assert no horizontal scrollbar appears (`document.documentElement.scrollWidth <= window.innerWidth`).
- Test the sticky "Add to cart" bar: it must not be present on desktop (≥1025 px) but must be present and fully visible on mobile (≤767 px); Playwright assertion + screenshot comparison at both widths.
- Verify image aspect ratios are maintained at all breakpoints: on PLP, product card images must maintain 1:1 ratio; use Playwright to assert `img.offsetHeight / img.offsetWidth` equals 1.0 (±0.02) at all defined viewports.
- Test that the checkout form switches from two-column (desktop) to single-column (mobile) layout at the correct breakpoint; screenshot at breakpoint ±1 px and diff with BackstopJS.
- Validate that mega-menu dropdowns do not overflow the viewport on tablet (768–1024 px); trigger a hover-state screenshot with Playwright at 768 px and assert no element's `right` edge exceeds `window.innerWidth`.
- Check font-size scaling at breakpoints: product title must be 24 px on mobile and 32 px on desktop; Playwright asserts `getComputedStyle(h1).fontSize` at each defined viewport, aligned with the design-token values.
- Test that hero banner switches from landscape (16:9) to portrait (4:5) crop at the mobile breakpoint; screenshot at 375 px and 1440 px and pixel-diff against the pre-approved responsive banner design.
- Verify table layouts (e.g., order history, product comparison) convert to card/stacked layouts at mobile; screenshot at 375 px and assert no table element has `overflow-x: scroll` applied (indicating unconstrained width).
- Run the full BackstopJS breakpoint suite in CI on every pull request; set a 4-minute time budget and fail if the suite exceeds it; use `asyncCaptureLimit: 10` and `asyncCompareLimit: 50` to parallelize and stay within budget.

### Dark mode compatibility (if applicable)

- Enable dark mode via `page.emulateMedia({ colorScheme: 'dark' })` in Playwright for all primary page types (homepage, PDP, PLP, cart, checkout) and capture Percy snapshots; maintain separate dark-mode baselines distinct from light-mode baselines.
- Verify that all text elements meet WCAG AA contrast in dark mode by running `axe-core` with `rules: { 'color-contrast': { enabled: true } }` after emulating dark mode; fail the build on any contrast violation.
- Test that product images (which are not theme-aware) do not receive unwanted CSS filters (e.g., `filter: invert(1)`) in dark mode; Playwright asserts `getComputedStyle(img).filter === 'none'` for all product images.
- Check that SVG icons that use `currentColor` adopt the correct dark-mode text color; screenshot the icon set in dark mode and compare with the approved dark-mode icon reference using Percy at 0.2% threshold.
- Validate that the cookie consent banner is readable in dark mode: background must meet contrast ratio ≥ 4.5:1 against banner text; automate with axe-core on the banner element specifically.
- Test the dark-mode toggle button (if present): click it, verify `prefers-color-scheme` or `data-theme` attribute changes, then compare the full-page screenshot against the dark-mode baseline.
- Verify that third-party widgets (chat widget, trust badge, review widget) do not break the dark-mode color scheme; screenshot the full page with widgets loaded and assert the overall diff against baseline is < 2% (allowing for widget rendering variability).
- Test that `<meta name="color-scheme" content="light dark">` is present in the `<head>` so browsers can apply correct system UI colors; Playwright asserts element presence.
- Validate system-dark-mode on macOS Safari: use Playwright with `channel: 'webkit'` and `colorScheme: 'dark'`; compare snapshots against Chromium dark-mode baselines with a 1% tolerance for engine-specific rendering differences.
- Check that form inputs (text fields, dropdowns, checkboxes) in checkout adopt dark-mode-appropriate background and border colors; Percy diff on the checkout address form in dark mode; threshold: 0.2%.
- Confirm that the favicon and browser tab color (`<meta name="theme-color">`) update correctly for dark mode; Playwright asserts the meta tag `content` value changes when `colorScheme: 'dark'` is emulated.
