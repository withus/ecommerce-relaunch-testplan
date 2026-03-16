# Navigation & UX Patterns (detailed)

### Breadcrumb navigation correct and complete

- Verify that every page below the homepage renders a breadcrumb trail matching the actual URL hierarchy (e.g. Home > Category > Subcategory > Product Name).
- Confirm the current page is the last breadcrumb segment and is either visually distinct (not a link) or carries `aria-current="page"`.
- Navigate directly to a deeply nested product page via an external URL and confirm the full breadcrumb chain is rendered — not just the immediate parent.
- Change a product's category in the CMS and verify the breadcrumb on the product page reflects the new category without a cache flush being required.
- Click each breadcrumb link and confirm it navigates to the correct page and does not produce a 404.
- Confirm breadcrumb structured data (`BreadcrumbList` schema) is present in the page source and validates in Google's Rich Results Test.
- On mobile viewports (<375 px width) confirm the breadcrumb either truncates gracefully (ellipsis or horizontal scroll) and does not overflow or wrap in a way that breaks the layout.
- Verify that a category page which also acts as a landing page (e.g. `/sale/`) shows a breadcrumb consistent with its position in the nav tree, not a generic "Home > Sale" stub.
- Open a product page in a new tab (no referrer). Confirm breadcrumb is rendered based on the page's own canonical hierarchy, not the referring page.
- On the checkout and account pages, confirm either no breadcrumb is displayed (by design) or a clear simplified trail is shown — never a broken or empty `<nav>` element.

### Back button behavior (browser history correct?)

- Add a product to the cart from a category page. Press the browser back button. Confirm the category page re-loads at the exact scroll position and with the same active filters, not at the top.
- Navigate: Homepage → Category → Subcategory → Product page. Press Back four times in sequence. Confirm each step returns to the correct previous page without any step being skipped or repeated.
- Open a product quick-view modal on a category page. Verify that pressing Back closes the modal and does NOT navigate away from the category page (modal should push a history entry or use a hash, then pop it on close).
- Complete the checkout address step, advance to the payment step, press Back. Confirm the address form is re-displayed with previously entered data intact — not an empty form.
- After a successful order, press the browser Back button from the confirmation page. Confirm the application does not re-submit the order but instead redirects to the homepage or order history page with a suitable message.
- Open a product page, then use the browser Forward button (after pressing Back). Confirm the page loads at the correct scroll position and all images are visible.
- Navigate to a filtered category URL (e.g. `?color=red&size=M`). Press Back from a product page. Confirm the filter state is preserved and the URL matches the filtered state.
- In a Single-Page Application (SPA) router context: verify that the browser's `history.length` increments by exactly one for each distinct page navigation and does not accumulate duplicate entries for re-renders.
- Navigate through 10+ pages in sequence, then use the browser history dropdown (long-press Back). Confirm all entries are distinct and correctly labelled.
- After a session timeout, press Back. Confirm the application does not display a partially authenticated or broken state but instead re-directs to the login page.
- Verify that hash-based scroll anchors (e.g. `#reviews`) do not pollute the forward/back stack with extra entries when scrolled to programmatically.

### Pagination vs. "Load more" vs. infinite scroll

- On a paginated category page, navigate to page 3, then press Back. Confirm the browser returns to page 2, not to page 1 or the top of page 3.
- Verify that each paginated page has a unique, shareable URL (e.g. `?page=3` or `/category/page/3/`) and that navigating directly to it loads the correct product set.
- Click "Load more" five times, then press Back. Confirm the browser returns to the state before the last "Load more" click — NOT to the very first batch of results. (The URL must encode the loaded state or the UI must restore it.)
- On an infinite scroll page, scroll down to trigger three auto-loads, then click a product. Press Back. Confirm the scroll position and loaded items are restored to where they were before clicking the product.
- Verify that `rel="next"` / `rel="prev"` link tags are present in the `<head>` for paginated pages (or the current SEO-equivalent approach is documented and implemented).
- Confirm that the "Load more" button shows a loading spinner while fetching and is disabled during the fetch to prevent double-requests.
- Test infinite scroll with a slow network (throttled to 3G in DevTools). Confirm a loading indicator is visible before new items appear, and content does not jump (no CLS).
- Verify that infinite scroll stops gracefully when all products are loaded — a clear "All products shown" or similar end-of-feed message is displayed and no further API calls are made.
- Navigate to page 5 of a paginated list via direct URL. Confirm the product count and pagination controls reflect the correct total (not just the items on page 5).
- Test that changing a filter on a paginated list resets to page 1 and updates the URL accordingly — remaining on page 5 after applying a filter that yields only 12 products would be a bug.

### Scroll position restoration after back navigation

- From a long category page (50+ products), scroll to the bottom and click the last product. Press Back. Confirm the page scrolls back to the vertical offset where the clicked product is visible — not the top of the page.
- Repeat the above test on mobile (iOS Safari and Android Chrome), as each browser implements `history.scrollRestoration` differently.
- Set `history.scrollRestoration = 'manual'` detection: if the app manages scroll restoration itself, verify that the saved `scrollY` value is accurate and is restored within 100 ms of the page becoming interactive.
- Navigate back to a page that has a sticky header. Confirm the restored scroll position accounts for the sticky header height so the previously visible item is not obscured.
- Test scroll restoration after a tab is put to sleep by the browser (background tab for 5+ minutes on mobile). Confirm the restored position on re-activation is correct.
- On a page with lazy-loaded images, confirm the scroll position is restored AFTER lazy images in the restored viewport have loaded — not before, which would cause the page to jump.
- Verify that anchor-link navigation (`<a href="#section-3">`) does not interfere with history-based scroll restoration when Back is pressed after following an anchor.
- Test with an accordion or tab component: expand an accordion item, scroll down, click a link, press Back. Confirm both the scroll position AND the accordion's expanded state are restored.
- Confirm that scroll restoration does not trigger a visible "scroll animation" — it should be instantaneous (or a CSS `scroll-behavior: auto` override is applied during restoration).
- On a search results page, enter a query, scroll down, click a result, press Back. Confirm the query is still in the search field and the results list is restored at the previous scroll position.

### Sticky header/navigation behavior on scroll

- Scroll down 200 px on a page with a sticky header. Confirm the header sticks to the top of the viewport and remains fully visible with no partial clipping.
- Measure the sticky header's height and verify that in-page anchor links (`#section`) offset their target by the header height (e.g. via `scroll-margin-top` or a JS offset), so the target is not hidden behind the header.
- On mobile, confirm the sticky header does not consume more than 15% of the viewport height, leaving sufficient space for content.
- Scroll down to show the sticky header, then scroll back up. Verify that the header does not flicker (appear/disappear rapidly) at the threshold where it transitions between sticky and static positioning.
- With a sticky header that hides on scroll-down and reveals on scroll-up ("smart header"): confirm the reveal happens within 150 ms of the scroll direction reversing, with no layout shift.
- Confirm that opening a mobile navigation menu from within the sticky header locks the page scroll (no scroll-through behind the open menu).
- Test with browser zoom at 150% and 200%. Confirm the sticky header does not overflow its container or overlap content.
- Open the browser print dialog. Confirm the sticky header does not duplicate on every printed page (it should use `position: static` in the print stylesheet).
- Verify that the sticky header's `z-index` is high enough to appear above product image zoom overlays, but does not prevent interacting with any modal or cookie consent layer that should appear on top.
- On iOS Safari, confirm the sticky header remains correctly positioned when the address bar hides/shows on scroll (viewport height change), without a 1 px gap or jump.

### Footer links (all functional, no dead links)

- Crawl all footer links programmatically (or manually) and confirm every link returns HTTP 200. Record any 301 redirects and confirm they resolve to the correct destination.
- Verify that external links in the footer (social media profiles, partner sites, certification badges) open in a new tab (`target="_blank"`) and include `rel="noopener noreferrer"`.
- Confirm that the legal-mandatory links (Privacy Policy, Imprint, Terms & Conditions, Cancellation Policy) are present in the footer on every page type — including checkout pages where the main navigation may be stripped.
- Check all footer links on mobile. Confirm tap targets are at least 44x44 px with sufficient spacing to prevent mis-taps.
- Verify that the footer's payment method icons either link to a payment methods information page or carry descriptive `alt` text — they must not be empty decorative images without context.
- After a site relaunch, compare the old footer link set against the new one. Confirm no links have been silently removed (especially contact or support links).
- Test footer links with JavaScript disabled. Confirm all links are standard `<a href>` elements — none rely on JS click handlers to navigate.

### Mega menu behavior on desktop and mobile

- On desktop: hover over a top-level navigation item that triggers a mega menu. Confirm the mega menu opens within 200 ms and does not flicker when moving the mouse from the trigger to the menu content.
- Move the mouse diagonally from the trigger toward a sub-item. Confirm the mega menu does not close prematurely during diagonal mouse movement (diagonal tolerance / "triangle" technique must be implemented).
- On desktop: open the mega menu, then move the mouse to a different top-level item. Confirm the previous mega menu closes and the new one opens — no two mega menus visible simultaneously.
- Open the mega menu and press Escape. Confirm the menu closes and focus returns to the trigger element.
- Navigate the mega menu entirely with the keyboard (Tab, arrow keys, Enter, Escape). Confirm all links are reachable without a mouse, and the focus order is logical (left-to-right, top-to-bottom within columns).
- Confirm that the mega menu does not cause a layout shift (CLS) when it opens by pushing down page content. It should overlay the page content using absolute/fixed positioning.
- On mobile: tap the hamburger icon. Confirm the mobile menu opens as a full-screen or side-drawer overlay, not as a tiny dropdown.
- On mobile: tap a parent category with sub-items. Confirm a second level slides in or expands inline — the user is never dumped onto the parent category page without choosing a sub-category (unless that is intended).
- On mobile: open the menu, then rotate the device (portrait ↔ landscape). Confirm the menu remains functional and does not become clipped or unscrollable after rotation.
- Close the mobile menu by tapping the close button, pressing Escape, or tapping outside the menu. Confirm focus returns to the hamburger trigger, the page scroll lock is released, and no invisible overlay remains blocking clicks.
- With a screen reader (NVDA/VoiceOver): confirm the mega menu is marked with `role="navigation"` and `aria-label`, sub-menus use `aria-expanded` on their triggers, and sub-menu visibility is communicated to the screen reader without visual focus.

### Keyboard navigation (tab order logical)

- Load the homepage and press Tab repeatedly from the start. Confirm focus moves in a logical visual order: skip link → header (logo, nav, search, cart) → main content → footer. No focus jumps to an unexpected area.
- Verify that every interactive element (links, buttons, form fields, custom dropdowns, sliders) receives focus in the correct document order and is reachable via Tab alone (no mouse required).
- Confirm that focus is NEVER trapped outside of an intentional modal or dialog. After closing a modal, focus must return to the element that opened it.
- Test a custom styled dropdown (e.g. a size selector built with `<div>` instead of `<select>`). Confirm it is focusable, opens on Enter/Space, arrow keys move between options, Enter/Space selects, and Escape closes without navigating away.
- Verify that interactive elements with `tabindex="-1"` are not reachable by Tab (correct) but are still programmatically focusable by the application (e.g. for dialog management).
- Confirm there are no positive `tabindex` values (`tabindex="1"`, `tabindex="2"`, etc.) in the page source, as these break the natural tab order.
- On the checkout multi-step form, confirm the tab order stays within the current step and does not allow Tab to reach fields in collapsed/hidden future steps.
- Test with the VoiceOver screen reader (macOS/iOS) and NVDA (Windows). Confirm announced focus descriptions match the visible label of each element.
- After a dynamic content update (e.g. filter results reload, new cart item added), confirm focus is managed appropriately — either moved to the updated region or remains on the triggering element, with an `aria-live` region announcing the change.
- Verify that the "Skip to main content" link is the first focusable element on every page, becomes visible on focus, and correctly moves focus past the header navigation to the first element of the main content area.

### Skip-to-content link for screen readers

- Press Tab once from the browser's address bar on any page. Confirm the skip link becomes visible (it may be visually hidden by default but must appear on focus — it must NOT remain permanently hidden with `display:none` or `visibility:hidden`).
- Activate the skip link via Enter. Confirm focus moves to the main content area (`<main>` or the element identified by the link's `href`).
- Confirm the skip link target element has `tabindex="-1"` so it can receive programmatic focus even if it is not normally focusable (e.g. a `<main>` element).
- On a page with multiple landmark regions (main nav, product filters sidebar, main content, footer), verify there is at least a "Skip to main content" link and optionally a "Skip to product list" link on category pages — testers should confirm with the UX specification what is required.
- Confirm the skip link is present on all page types: homepage, category pages, product pages, cart, checkout steps, account pages, CMS pages — not only the homepage.
- Test with keyboard only (no screen reader). Confirm sighted keyboard users can also use the skip link to bypass repetitive navigation.
- Verify the skip link's visual appearance on focus meets WCAG AA contrast requirements (4.5:1 for normal text).
- Test with NVDA + Firefox and VoiceOver + Safari. Confirm the skip link is announced correctly (not read as an invisible link) and activation lands focus correctly in both assistive technology combinations.
