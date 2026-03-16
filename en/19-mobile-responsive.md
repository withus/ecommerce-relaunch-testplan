# 19. Mobile / Responsive

---

### All pages correct on mobile

- **iPhone SE (375×667px):** Load homepage — verify no horizontal scrolling, all text readable without zooming, no content overflow beyond viewport width.
- **iPhone 15 Pro Max (430×932px):** Load category page — verify product grid reflows to 2-column layout, images scale correctly, no elements cut off at edges.
- **Samsung Galaxy S23 (360×780px):** Load product detail page — verify all sections (images, title, price, description, add-to-cart) are visible and correctly stacked vertically.
- **Samsung Galaxy A54 (412×915px):** Load cart page — verify all line items, quantity controls, subtotal, and checkout button are visible without horizontal scroll.
- **Xiaomi Redmi Note 12 (393×851px):** Load order confirmation page — verify order number, summary table, and CTA links are fully visible.
- **iPhone 14 (390×844px):** Load 404 error page — verify error message, search field, and navigation links render correctly.
- **Google Pixel 7 (412×915px):** Load contact page — verify form, phone number, map embed (if present) all display correctly stacked.
- **Motorola Moto G Power (412×892px):** Load blog/article page — verify heading hierarchy, text, and images are readable and correctly sized.
- **iPhone 12 Mini (360×780px):** Load checkout address step — verify all form fields, labels, and the "Continue" button are visible without overflow.
- **General (320px min-width):** Test all key pages at 320px viewport width — the absolute minimum for modern mobile support — verify no content is clipped or overlapping.

---

### All pages correct on tablet

- **iPad (768×1024px, portrait):** Load homepage — verify layout transitions from mobile to tablet layout (e.g., wider product grid, potentially visible sidebar or navigation).
- **iPad Pro 12.9" (1024×1366px, portrait):** Load category page — verify product grid shows 3–4 columns, filters are accessible, pagination controls are fully visible.
- **Samsung Galaxy Tab S8 (800×1280px, portrait):** Load product detail page — verify image gallery and product info display side-by-side or in a well-structured tablet layout.
- **iPad Mini (744×1133px, landscape):** Load checkout — verify multi-step form is not overly stretched, form fields have appropriate max-width.
- **iPad Air (820×1180px, portrait):** Load customer account dashboard — verify all account sections (orders, addresses, wishlist) display cleanly.
- **Amazon Fire HD 10 (800×1280px):** Load cart page — verify all cart items, coupon field, and order summary are visible with correct spacing.
- **Samsung Galaxy Tab A8 (800×1280px, portrait):** Load navigation/header — verify whether hamburger or full horizontal nav is shown, verify all links are reachable.
- **iPad (768px, landscape → 1024px):** Rotate tablet from portrait to landscape — verify layout reflows without blank spaces or broken columns.

---

### Touch targets large enough (min. 44×44px)

- **iPhone SE (375px):** Inspect all navigation links in the hamburger menu — each item must have a tap area of at least 44×44 CSS pixels (use browser DevTools or manual measurement).
- **iPhone 14 (390px):** Verify all "Add to Cart" buttons on category page thumbnails meet 44×44px minimum — no small icon-only buttons that are difficult to tap.
- **Samsung Galaxy S23 (360px):** Verify quantity increment/decrement buttons on cart and product pages are at least 44×44px, easily tappable with a thumb.
- **iPad (768px):** Verify filter checkboxes and their labels are tappable — label should extend the hit area so small checkboxes are not the only target.
- **Any mobile device:** Verify all icon-only buttons (wishlist heart, close X, share icon) have sufficient tap area — icon may be 20px but surrounding padding must reach 44×44px total.
- **Any mobile device:** Verify form radio buttons and checkboxes in checkout (payment selection, T&C) have large enough tap areas — test by tapping near but not directly on the visual control.
- **iPhone 15 Pro Max (430px):** Verify the cookie consent banner "Accept" and "Decline" buttons are large enough to tap without errors.
- **Samsung Galaxy A54 (412px):** Verify pagination controls (previous/next page buttons, page number links) all meet minimum tap target size.
- **Any mobile device:** Verify that linked text elements (e.g., "Forgot password?" link) are not so short that they are difficult to tap — minimum width should be enforced via padding.
- **Chrome DevTools touch simulation:** Use "Rendering > Show layout shift regions" + Accessibility panel to audit tap target sizes programmatically across all key pages.

---

### Swipe gestures (image gallery, slider)

- **iPhone 14 (390px):** On product detail page, swipe left/right through the image gallery — verify each image transitions smoothly, no accidental page scroll triggered during horizontal swipe.
- **Samsung Galaxy S23 (360px):** Swipe through homepage hero/banner slider — verify momentum scrolling feels natural, images do not flicker or overlap during transition.
- **iPad (768px, portrait):** Swipe through product image thumbnails carousel — verify gesture works with single finger, images load correctly after swipe.
- **iPhone SE (375px):** In image lightbox/fullscreen view, swipe to next/previous image — verify gesture works and pinch-to-zoom is not accidentally triggered.
- **Any mobile device:** Perform a fast flick gesture on the product gallery — verify the slider does not over-scroll past the last image or snap back erratically.
- **Samsung Galaxy Tab S8 (800px):** Swipe through a "Related products" horizontal scroll carousel on product page — verify smooth scrolling, no visual tearing.
- **iPhone 15 Pro Max (430px):** While viewing a slider/gallery, attempt a vertical scroll at the same time — verify the page correctly distinguishes vertical scroll from horizontal swipe.
- **Any mobile device:** Tap-and-hold on an image in the gallery — verify no browser default context menu appears that would interrupt the swipe experience (if intentionally suppressed).
- **Any mobile device:** Swipe between checkout steps (if step indicator is interactive) — verify only intentional navigation triggers step change, not accidental swipes.

---

### Mobile navigation (hamburger menu)

- **iPhone SE (375px):** Tap the hamburger icon — verify the menu opens fully without overlapping the viewport edge, all top-level category links are visible.
- **iPhone 14 (390px):** Open hamburger menu and tap a category with subcategories — verify expand/collapse animation works, subcategory links are reachable.
- **Samsung Galaxy S23 (360px):** Open hamburger menu and navigate 3 levels deep (e.g., Category > Subcategory > Sub-subcategory) — verify back navigation and breadcrumbs within the menu work correctly.
- **iPad Mini (744px, portrait):** Verify whether hamburger or full desktop nav is shown at this breakpoint — whichever is used must be fully functional.
- **Any mobile device:** Open hamburger menu, then press the browser back button — verify the menu closes and the user is not navigated away from the current page.
- **iPhone 15 Pro Max (430px):** Open hamburger menu and attempt to scroll within it (if menu is longer than viewport) — verify internal scroll works without dismissing the menu.
- **Samsung Galaxy A54 (412px):** Tap anywhere outside the open hamburger menu — verify the menu closes correctly.
- **Any mobile device:** Open hamburger menu and verify the close button (X) is visible, correctly positioned, and has a minimum 44×44px tap target.
- **Any mobile device:** Open hamburger menu with a screen reader enabled (TalkBack/VoiceOver) — verify the menu is announced correctly and focus is trapped inside while open.
- **iPhone 14 (390px):** Verify that the hamburger menu does not appear at desktop breakpoints (≥1024px) — only visible on mobile/tablet.

---

### Mobile checkout optimized

- **iPhone SE (375px):** Go through the full checkout flow — verify all steps (address, shipping, payment, confirmation) fit the screen without horizontal scroll, no fields are cut off.
- **iPhone 14 (390px):** On the payment step, verify that payment method selection (e.g., radio buttons for card/PayPal/invoice) is clearly readable and tappable with spacing between options.
- **Samsung Galaxy S23 (360px):** Fill in the address form — verify input fields are wide enough to type comfortably, labels do not overlap fields, error messages display below the affected field.
- **iPad (768px, portrait):** Complete checkout — verify form layout uses available space appropriately (e.g., first name and last name side-by-side on tablet vs. stacked on mobile).
- **Any mobile device:** In checkout, tap the order summary accordion/toggle — verify it expands/collapses smoothly to show/hide line items without reflowing the page layout.
- **iPhone 15 Pro Max (430px):** Verify the "Place Order" button is always visible (sticky or after scrolling) and does not require the user to scroll past distracting content.
- **Samsung Galaxy A54 (412px):** Enter a coupon code in checkout on mobile — verify the coupon input field and "Apply" button are side-by-side or clearly stacked, neither is obscured by the keyboard.
- **Any mobile device:** Complete checkout using autofill (Safari/Chrome autofill for address) — verify autofill populates all fields correctly without breaking layout.
- **iPhone 14 (390px):** On checkout confirmation page, verify the order number is prominently visible above the fold on mobile.
- **Any mobile device:** Start checkout as a guest, verify the "Register" or "Login" option during checkout is presented clearly without cluttering the primary flow.

---

### Mobile payment (Apple Pay, Google Pay)

- **iPhone 14 (390px) + Safari:** On product detail page, verify the Apple Pay button is visible and correctly sized — tap it and verify the Apple Pay sheet appears with correct order total and item name.
- **iPhone 15 Pro Max (430px) + Safari:** Verify Apple Pay button is NOT shown in Chrome on iOS (Apple Pay is Safari-only) — the button must be conditionally rendered.
- **Android (Chrome, Samsung Galaxy S23):** On product detail page, verify the Google Pay button is visible and correctly sized — tap it and verify the Google Pay sheet appears with correct amount.
- **iPad (768px) + Safari:** Verify Apple Pay button appears on product page and in cart — complete a test Apple Pay transaction in sandbox mode.
- **Any Android device:** Verify Google Pay button does NOT appear if the device/browser does not support it (feature detection via `PaymentRequest` API).
- **iPhone 14 + Safari:** In the express checkout flow via Apple Pay, verify the shipping address populated from the Wallet is transferred correctly to the order.
- **Samsung Galaxy S23 + Chrome:** Complete a Google Pay test payment — verify redirect to confirmation page with correct order number.
- **Any mobile device:** Cancel Apple Pay / Google Pay sheet mid-flow — verify the user is returned to the product page or cart without an error state or orphaned payment intent.
- **Any mobile device:** Verify Apple Pay / Google Pay buttons have correct branding (Apple/Google guidelines: no custom-styled buttons, correct logo usage).
- **iPhone SE (375px):** Verify Apple Pay button width scales correctly on small screens — no overflow, no cropped button text.

---

### Landscape + portrait mode

- **iPhone 14 (390×844px portrait → 844×390px landscape):** Rotate device on product detail page — verify layout reflows, images rescale, no text is cut off, and the page is usable in both orientations.
- **iPhone SE (375×667px portrait → 667×375px landscape):** Rotate on checkout address form — verify form fields remain usable in landscape (keyboard takes significant space; form must scroll above keyboard).
- **Samsung Galaxy S23 (360×780px portrait → 780×360px landscape):** Rotate on category page — verify product grid adjusts column count appropriately.
- **iPad (768×1024px portrait → 1024×768px landscape):** Rotate on homepage — verify hero banner, navigation, and content sections reflow correctly without blank space or broken alignment.
- **Any mobile device:** Open the hamburger menu, then rotate device — verify the menu either closes gracefully or adapts to the new orientation without visual breakage.
- **Any mobile device:** During a swipe-image-gallery interaction, rotate device — verify gallery state is preserved (same image visible) after rotation.
- **iPhone 15 Pro Max (430×932px portrait → 932×430px landscape):** Open modal/popup (e.g., quick view, login overlay) and rotate — verify modal resizes correctly and close button remains accessible.
- **Any mobile device:** During checkout, rotate device while a keyboard is open — verify no content is permanently hidden behind the keyboard or OS UI elements after rotation.
- **iPad Pro (1024×1366px portrait → 1366×1024px landscape):** Verify that the desktop navigation (not hamburger) appears at the larger landscape breakpoint.

---

### Pinch-to-zoom on product images

- **iPhone 14 (390px):** On product detail page, perform a pinch-to-zoom gesture on the main product image — verify zoom is activated (either native browser zoom or custom lightbox zoom) and image remains sharp.
- **Samsung Galaxy S23 (360px):** Pinch-to-zoom inside the image gallery lightbox — verify zoom level increases smoothly and the user can pan around the zoomed image.
- **iPad (768px):** Pinch-to-zoom on product image — verify the gesture does not accidentally trigger page-level zoom if custom zoom is implemented.
- **iPhone SE (375px):** Verify that if `user-scalable=no` is NOT set in the viewport meta tag, native pinch-to-zoom is available as a fallback for accessibility.
- **Any mobile device:** Double-tap on product image — verify it triggers zoom (either 2× native or a custom double-tap-to-zoom implementation).
- **Any mobile device:** After zooming into an image, swipe to pan — verify panning works within the zoomed image without accidentally triggering the image slider navigation.
- **Any mobile device:** After zooming into an image, pinch outward to zoom back out — verify the image returns to its original state correctly.
- **iPhone 15 Pro Max (430px):** Zoom into image, then attempt to swipe to the next image — verify the gesture distinction: pan while zoomed, swipe to next image when at zoom level 1.

---

### Sticky add-to-cart on mobile

- **iPhone 14 (390px):** On a long product detail page, scroll past the main "Add to Cart" button — verify a sticky/floating "Add to Cart" bar appears at the bottom of the viewport.
- **Samsung Galaxy S23 (360px):** Verify the sticky add-to-cart bar shows the correct product name and current price (including any variant-based price changes).
- **iPhone SE (375px):** Verify the sticky add-to-cart bar does not overlap important content permanently — it should appear only after scrolling past the primary button.
- **Any mobile device:** Tap the sticky add-to-cart button — verify the product is added to cart and the user receives the same confirmation feedback as when using the primary button.
- **Any mobile device:** On a product with variants (e.g., size/color), scroll down past the variant selector — verify the sticky bar either shows a "Select options" prompt or reflects the currently selected variant.
- **iPhone 15 Pro Max (430px):** Verify the sticky add-to-cart bar does not overlap the browser's bottom navigation bar (home indicator area on iOS) — safe area insets must be respected.
- **Samsung Galaxy A54 (412px):** Verify that the sticky add-to-cart bar disappears or is hidden when the user scrolls back up to the primary "Add to Cart" button area.
- **iPad (768px):** Verify whether the sticky add-to-cart bar is shown on tablet — it may only be needed on smaller mobile screens; verify the correct breakpoint behavior.
- **Any mobile device:** With the sticky bar visible, rotate device to landscape — verify the bar adapts to the shorter viewport height without covering too much content.

---

### Mobile keyboard (numeric for postal code, email keyboard for email)

- **iPhone 14 (390px):** Tap the postal code input in checkout — verify the numeric keyboard (`inputmode="numeric"` or `type="tel"`) appears automatically, not the full QWERTY keyboard.
- **Samsung Galaxy S23 (360px):** Tap the email address input — verify the email keyboard appears (with `@` and `.com` keys prominent) instead of the standard keyboard.
- **iPhone SE (375px):** Tap a phone number input — verify the telephone keypad (`type="tel"`) appears.
- **Any mobile device:** Tap a quantity input field (cart, product page) — verify a numeric keyboard appears rather than the full text keyboard.
- **Any mobile device:** Tap a credit card number field — verify the numeric keyboard appears (`inputmode="numeric"` or `autocomplete="cc-number"`).
- **Any mobile device:** Tap a credit card expiry date field — verify numeric keyboard appears and `autocomplete="cc-exp"` is set to enable autofill.
- **Any mobile device:** Tap a password field — verify the standard keyboard appears (not email or numeric) and the field has `type="password"` so input is masked.
- **iPhone 14 (390px):** Verify that tapping a search field shows the keyboard with a "Search" / return key labelled "Search" (using `type="search"` or `enterkeyhint="search"`).
- **Samsung Galaxy S23 (360px):** Verify that tapping a first-name or last-name field shows the standard keyboard with autocorrect disabled (`autocomplete="given-name"` / `autocomplete="family-name"`).
- **Any mobile device:** After filling in a form field and dismissing the keyboard, verify the viewport scroll position is correct — no content should be permanently pushed up or hidden.

---

### Forms usable on mobile

- **iPhone SE (375px):** Complete the full registration form — verify all fields (email, password, name) are reachable, labels do not overlap input fields, and error messages are visible below each field.
- **Samsung Galaxy S23 (360px):** Submit the contact form with intentionally invalid data — verify inline error messages appear below the relevant fields without pushing other content off-screen.
- **iPhone 14 (390px):** Fill in the checkout address form — verify that when the keyboard is open, the currently focused field scrolls into view above the keyboard and is not hidden behind it.
- **iPad (768px, portrait):** Complete the checkout address form — verify appropriate use of wider layout (e.g., city and postal code on the same row).
- **Any mobile device:** Tab/next-field navigation — verify that pressing "Next" on the mobile keyboard moves focus to the next logical form field in the correct sequence.
- **Any mobile device:** Verify all `<select>` dropdowns open the native OS picker (country selection, state, etc.) and selections are reflected correctly in the UI.
- **iPhone 15 Pro Max (430px):** Verify date picker inputs (e.g., preferred delivery date) open the native date picker on iOS and the selected date is correctly formatted for the server.
- **Any mobile device:** Verify placeholder text in input fields is sufficiently contrasted and does not disappear with the cursor in a way that makes the field label unclear.
- **Any mobile device:** Verify that multi-line `<textarea>` fields (e.g., order notes, product personalization text) are resizable or have sufficient default height on mobile without requiring pinch-to-zoom.
- **Samsung Galaxy A54 (412px):** Verify the password reveal toggle button (show/hide password) is visible and tappable without requiring precise tapping on small icon.

---

### Popups closable on mobile

- **iPhone SE (375px):** Open cookie consent banner — verify a clearly visible "Accept", "Decline", or "Close" button is present and tappable (min. 44×44px).
- **iPhone 14 (390px):** Trigger a product quick view popup — verify the close button (X) is positioned so it does not overlap with critical popup content and is easily tappable.
- **Samsung Galaxy S23 (360px):** Open a newsletter signup popup — verify the close button is not hidden behind the OS navigation bar or keyboard and is accessible with one tap.
- **Any mobile device:** Open a popup/modal and attempt to close it by tapping the backdrop (outside the modal) — verify this works as an alternative close method.
- **Any mobile device:** Open a popup/modal and press the device back button (Android) — verify the modal closes rather than navigating away from the page.
- **iPhone 15 Pro Max (430px):** Open a popup that contains a form (e.g., login modal) — tap a form field so the keyboard opens — verify the modal does not become inaccessible or uncloseable when the keyboard is visible.
- **iPad (768px):** Verify that popups designed for mobile are appropriately sized on tablet (not too small, not edge-to-edge) and the close button remains accessible.
- **Any mobile device:** Open a cookie consent popup on first visit — verify it does not auto-dismiss and cannot be accidentally closed by scrolling.
- **Any mobile device:** Trigger a "Size guide" or "Product zoom" modal — verify it opens full-screen or as a sheet and has a visible, accessible close control.
- **iPhone SE (375px):** On checkout, trigger a "Security info" or "Klarna info" tooltip/popup — verify it does not extend beyond viewport edges and can be dismissed.

---

### Viewport meta tag correct

- **All devices:** Inspect page source or use browser DevTools — verify `<meta name="viewport" content="width=device-width, initial-scale=1">` is present in the `<head>` on every page type (homepage, PDP, PLP, checkout, account, CMS pages).
- **Any mobile device:** Verify `user-scalable=no` or `maximum-scale=1` is NOT set in the viewport tag — these attributes block accessibility zoom and violate WCAG 1.4.4.
- **iPhone SE (375px):** Load a page and verify the viewport renders at the device's physical width with no horizontal scroll — confirming `width=device-width` is effective.
- **Android (Chrome):** Use Chrome DevTools remote debugging — verify no "viewport not configured" warning appears in the Console or Lighthouse audit.
- **Any mobile device:** Run a Google Lighthouse "Mobile" audit on key pages — verify the "Viewport" audit passes with no errors.
- **iPad (768px):** Verify that `initial-scale=1` is set so the page is not zoomed in or out by default when loaded on tablet.
- **Any mobile device:** Verify that pages do not rely on fixed pixel widths (e.g., `width: 1200px`) that would override the viewport meta tag and cause horizontal scrolling.
- **Any mobile device:** Verify that the viewport meta tag is consistent across all page templates — check at minimum: homepage, category page, product page, cart, checkout, and account pages.
