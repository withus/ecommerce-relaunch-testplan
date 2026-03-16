# 20. Cross-Browser Compatibility

---

### Chrome (desktop + mobile)

- Verify full checkout flow on Chrome stable (current release, e.g. 123+) on Windows 10/11 and macOS 14 Sonoma — all steps from cart to order confirmation complete without error
- Test on Chrome Beta and Chrome Dev channel to catch upcoming rendering changes before they reach stable
- Verify Chrome on Android 12+ (Pixel, Samsung Galaxy): product images, sliders, and quantity steppers render and function correctly
- Confirm that Chrome's native autofill correctly populates billing and shipping address fields (name, street, postal code, city, country) and does not break form validation
- Verify Chrome payment handler API (Google Pay, browser-saved cards) triggers correctly on the checkout payment step
- Test that Chrome's aggressive caching does not serve stale product prices or inventory status — add cache-busting headers and verify
- Confirm that service worker (if used for offline caching) does not intercept checkout API calls in Chrome
- Verify CSS Grid and Flexbox layouts (category page, product gallery, checkout columns) render pixel-accurately in Chrome DevTools responsive mode at 360px, 768px, 1280px, 1920px
- Check that `IntersectionObserver`-based lazy loading of product images works correctly in Chrome
- Confirm Chrome's strict third-party cookie blocking (Privacy Sandbox rollout) does not break session persistence, cart state, or tracking pixels

---

### Firefox (desktop + mobile)

- Test complete purchase flow on Firefox ESR (currently 115.x) and Firefox latest stable on Windows and macOS
- Verify Firefox's enhanced tracking protection (ETP) in Standard and Strict mode does not block payment provider iframes (Stripe, Klarna, PayPal) — if blocked, a fallback or user message must appear
- Confirm Firefox autofill behavior: Firefox uses its own autocomplete engine (distinct from Chrome). Test that `autocomplete` attributes on address and payment fields are correctly specified (`given-name`, `family-name`, `address-line1`, `postal-code`, `cc-number`, etc.) so Firefox autofill populates them
- Verify date and datetime inputs: Firefox does not render native `<input type="date">` spinners the same way as Chrome — test any delivery date selector or coupon expiry display for visual correctness
- Test CSS `gap` property in Flexbox (supported only since Firefox 63) — ensure no layout gaps in category grids on older Firefox ESR
- Check that `scrollbar-gutter` and custom scrollbar CSS do not cause double scrollbars in Firefox (Firefox does not support `::-webkit-scrollbar` pseudo-elements)
- Verify that `position: sticky` on the header and the mobile add-to-cart bar works correctly in Firefox — Firefox has historically had sticky issues inside overflow containers
- Test password manager integration: Firefox Password Manager prompts should appear correctly after registration and login; confirm they do not interfere with checkout form progression
- Verify Firefox on Android (Firefox for Android, currently Fenix-based): test hamburger menu, product image swipe, and checkout form keyboard behavior
- Confirm that `Intl.NumberFormat` and `Intl.DateTimeFormat` (used for price and date formatting) produce correct German/English locale output in Firefox

---

### Safari (desktop + mobile / iOS)

- Test complete purchase flow on Safari 17 (macOS 14 Sonoma) and Safari on iOS 17 (iPhone 14/15) — Safari on iOS is the only browser engine allowed on iOS, making this critical
- Verify `<input type="date">` behavior: Safari renders a native date picker on iOS that looks different from Chrome. Test any delivery date, coupon expiry, or date-of-birth field — ensure the field value is submitted in the correct format (`YYYY-MM-DD`) regardless of how Safari displays it to the user
- Test Safari's ITP (Intelligent Tracking Prevention): third-party cookies are blocked by default. Verify that cross-domain tracking pixels (Meta, Google Ads) use server-side events or first-party proxies and do not break session continuity
- Verify Apple Pay: on Safari + macOS with Touch ID, or Safari + iOS, the Apple Pay button must appear and complete a test transaction. Confirm `PaymentRequest` API is used correctly; test that shipping address selection within the Apple Pay sheet updates shipping costs
- Check CSS `-webkit-` prefixes: ensure `backdrop-filter`, `text-size-adjust`, `-webkit-overflow-scrolling: touch` (deprecated but sometimes needed for older iOS), and sticky positioning work correctly
- Test `position: sticky` for the header and sticky add-to-cart button — Safari has a well-known bug where `sticky` fails inside elements with `overflow: hidden` or `overflow: auto`; verify the DOM structure avoids this
- Verify that focus styles are visible in Safari — by default, Safari outlines focusable elements differently than Chrome/Firefox. Confirm that custom focus styles are not stripped by `outline: none` without a visible replacement
- Test form `<select>` dropdowns for country and shipping method selection — Safari renders native `<select>` elements using an iOS bottom sheet on mobile; ensure the options are correct and selection updates dependent fields (e.g., shipping cost recalculation)
- Verify `<video>` autoplay behavior on Safari iOS: autoplay is only allowed if `muted` and `playsinline` attributes are present. Test any product hero videos or background videos
- Test checkout on Safari with "Private Browsing" mode enabled — `localStorage` and `sessionStorage` have reduced quotas; test that cart state and checkout progress are not lost mid-session
- Confirm `100vh` behavior on iOS Safari: the address bar causes `100vh` to overflow. Test full-height modals, overlays, and the mobile checkout flow on iPhone for this viewport unit bug
- Verify that custom fonts load correctly in Safari — Safari requires CORS headers on font files served from a CDN; test for font fallback or FOUT (flash of unstyled text)

---

### Edge (desktop)

- Test complete purchase flow on Microsoft Edge stable (Chromium-based, current version) on Windows 10 and Windows 11
- Verify Microsoft Autofill / Microsoft Wallet integration: Edge has its own autofill engine and password manager. Test that Edge's autofill populates address and payment fields correctly and does not conflict with custom JS validation
- Test that Edge's built-in "Shopping" feature (price comparison overlay) does not visually obstruct CTAs or the checkout button
- Confirm that Edge's tracking prevention (Basic, Balanced, Strict) does not break payment iframes or third-party widgets — test in all three modes
- Verify IE compatibility mode is not triggered: Edge can render pages in IE compatibility mode for sites on certain lists. Confirm the `X-UA-Compatible` header is NOT set to `IE=edge` in a way that would trigger legacy mode, and test that the site renders in Chromium mode
- Test Edge on Windows 11 with the Snap Layout feature — verify that the shop layout at 50% / 33% split-screen widths (approx. 640px and 426px) renders correctly
- Confirm that PDF invoice downloads open correctly in Edge's built-in PDF viewer without requiring a third-party plugin
- Verify that Windows Hello / WebAuthn flows (if implemented for passwordless login) work in Edge on Windows 11

---

### Samsung Internet (mobile)

- Test complete purchase flow on Samsung Internet 23+ (current) on Samsung Galaxy S23 / A54 running Android 13/14
- Verify that Samsung Internet's "Smart Anti-Tracking" and Samsung Pay do not interfere with the checkout flow or payment provider iframes
- Test Samsung Internet's custom text size scaling (Samsung devices allow larger system font sizes up to 130% or more) — verify that product titles, prices, and CTA buttons do not overflow their containers at increased font scale
- Confirm that Samsung Internet correctly handles the `viewport` meta tag and does not add extra zoom or scaling
- Test the hamburger menu, mobile navigation, and image sliders using touch gestures in Samsung Internet — verify swipe detection is not intercepted by Samsung's own gesture navigation (e.g., "swipe from edge" back gesture)
- Verify that Samsung Internet correctly renders CSS custom properties (CSS variables) used for theming and color tokens
- Test Dark Mode: Samsung Internet supports a built-in "Dark Mode" that can force-invert page colors. Verify that product images, logos, and trust badges are not affected, or implement `color-scheme: only light` if needed
- Confirm checkout form numeric keyboard appears for quantity, postal code, and phone number fields (correct `inputmode` attributes)

---

### Opera (optional)

- Test core checkout flow on Opera stable (Chromium-based, current version) on desktop — since Opera uses the Blink engine, most Chrome tests pass, but verify Opera-specific UI elements do not interfere
- Test Opera's built-in ad blocker: enable it and verify that the shop loads correctly, product images are not blocked, and payment provider scripts are not blocked
- Verify that Opera's free VPN feature (which routes traffic through Opera's servers) does not cause issues with geolocation-based features (country-specific pricing, shipping zone detection)
- Test Opera GX (gaming browser variant) — relevant if the product catalog targets gaming/tech audiences; verify the layout and checkout work without issues

---

### Various OS versions (iOS 15+, Android 10+)

- Test on iOS 15 (Safari 15): verify that the CSS `aspect-ratio` property (used for product image containers) renders correctly — this was a late addition in Safari 15 and may fail on older iOS builds
- Test on iOS 16 (Safari 16): verify that `focus-visible` pseudo-class works correctly (added in Safari 16) and that custom focus ring styles appear properly
- Test on iOS 17 (Safari 17): verify autocomplete and autofill for checkout address fields work with the updated WebKit autofill engine
- Test on Android 10 (Chrome 80-era, WebView older): verify that `CSS gap` in Flexbox is not used without a fallback, that `optional chaining` (`?.`) in JS is transpiled for older V8 versions, and that `Intl.RelativeTimeFormat` is available or polyfilled
- Test on Android 12+ with the new predictive back gesture: verify that pressing back during the checkout flow returns to the correct previous step without triggering unwanted navigation
- Test on Android 13+ with per-app language settings: if the user has a different language set for the browser app, verify that the shop still serves the correct locale and that form validation messages are in the expected language
- Test on Android 10 with Chrome Custom Tabs (used by apps with embedded WebViews): confirm that the checkout flow works inside a Custom Tab, including payment redirects
- Verify that on iOS 15–16, `<input type="number">` spinners are hidden or styled consistently — iOS renders these differently than Android
- Test on iOS 15 that `navigator.share()` (used for product share buttons) falls back gracefully when not available in older WebKit builds
- Test on Android 10 devices with small RAM (2 GB): simulate memory pressure and verify that the product configurator and checkout do not crash or lose state due to tab discarding

---

### Various screen resolutions

- Test at 1920x1080 (Full HD, most common desktop): verify 3-column or 4-column product grid, hero banner proportions, and checkout two-column layout
- Test at 2560x1440 (QHD / 2K, common for gaming monitors): verify that the layout does not stretch excessively or leave large empty margins; check max-width constraints are applied
- Test at 3840x2160 (4K): verify that vector assets (SVG icons, logos) are crisp and that raster images are served at 2x or 3x resolution via `srcset` or `<picture>`
- Test at 1366x768 (still common on budget laptops and older machines): verify that the header, navigation, and checkout do not overlap or collapse unexpectedly at this width
- Test at 1024x768 (iPad landscape in older browsers, some tablets): confirm the tablet layout breakpoint fires correctly and that the mega menu does not overflow
- Test at 375x667 (iPhone SE 2020, small phones): verify that all text is readable, touch targets are at least 44x44px, and no horizontal scrollbar appears
- Test at 414x896 (iPhone 11/XR): verify portrait and landscape modes; pay special attention to the landscape layout where the visible viewport height is very small
- Test at 360x800 (Samsung Galaxy A-series, common Android mid-range): verify the mobile layout, hamburger menu, and checkout form
- Test at 768x1024 (iPad portrait): verify that tablet-specific breakpoints (if any) handle this resolution correctly, especially the product image gallery and checkout layout
- Test with browser zoom levels of 110%, 125%, and 150% on desktop: verify that the layout does not break, overflow, or clip content — particularly important for accessibility and users with visual impairments
- Test at HiDPI / Retina displays (device pixel ratio 2x and 3x): verify that CSS pixel layouts are correct and that images are not blurry on retina screens
- Test with browser font size set to "Large" or "Very Large" in system accessibility settings: verify that increased base font size does not break button sizes, nav items, or form field heights
