# Navigation / Header

### Main menu all links working

- Click every top-level menu item and verify the correct category/page loads (URL, page title, H1 match expected destination).
- Hover over each top-level item that has children and confirm the dropdown/sub-menu appears without delay.
- Verify that clicking a menu link that points to an anchor on the same page scrolls smoothly to the correct section.
- Check that no menu item returns a 404, 500, or redirect loop (automated link-check pass + spot manual verification).
- Confirm that menu labels match the actual page names/H1 headings they link to (no stale copy after relaunch rename).
- Verify the active/current-page state is visually highlighted in the menu for the page currently being viewed.
- Test with JavaScript disabled: confirm at least a basic fallback link list is rendered (graceful degradation).
- Check that all menu links use correct protocol (https, not http) and do not trigger mixed-content warnings.
- Resize to mobile width (375 px) and confirm the desktop menu is hidden — no overlapping or invisible clickable areas remain.
- Navigate to a deeply nested page and verify the correct ancestor menu item shows an active/open state.

### Mega menu / dropdown menus

- Hover over each mega-menu trigger on desktop and confirm the panel opens within ~200 ms.
- Move the mouse off the trigger/panel and verify the panel closes cleanly without flickering or staying open.
- Click a link inside the mega-menu panel and confirm correct navigation (category page, brand page, CMS page).
- Verify all column headings and sub-links within the mega-menu are clickable and resolve correctly.
- Check that product images or promotional banners inside the mega-menu (if present) load, display the correct alt text, and link to the right destination.
- Test keyboard access: Tab into the trigger, press Enter/Space to open, Tab through sub-links, press Escape to close — all must work without a mouse.
- Confirm the mega-menu does not obscure the page content permanently (z-index / overflow correct).
- On tablet (768 px): verify the mega-menu either adapts to touch (tap to open) or collapses into the mobile hamburger menu.
- Confirm that the mega-menu closes when the user clicks anywhere outside the panel.
- Verify that ARIA attributes are correct: `aria-expanded` toggles on open/close, `aria-haspopup` is set on triggers, role="menu"/"menuitem" used appropriately.
- Scroll down 300 px and re-hover the trigger: confirm the mega-menu still opens in the correct position relative to the sticky header.

### Mobile hamburger menu

- On a viewport ≤ 767 px, confirm the hamburger icon is visible and the desktop menu is hidden.
- Tap the hamburger icon and verify the mobile nav panel opens with a smooth animation (no layout jump).
- Tap the close button (or hamburger again) and verify the panel closes and focus returns to the hamburger button.
- Expand a category accordion item within the mobile menu and confirm sub-links are revealed correctly.
- Collapse an expanded accordion item and confirm sub-links hide without leaving residual space.
- Tap a sub-link and confirm correct navigation; verify the menu panel closes after navigation.
- Verify the mobile menu panel covers the full viewport height and is scrollable when it contains many links.
- Confirm the hamburger menu does not interfere with the browser's native pull-to-refresh gesture.
- Test on both iOS Safari and Android Chrome for consistent appearance and touch behavior.
- Verify that the mobile menu is keyboard and screen-reader accessible (focus trap inside open panel, Escape closes it).
- Open the mobile menu, then rotate to landscape mode; confirm the layout reflows correctly without the menu getting cut off.
- Confirm the mobile menu is completely inaccessible to keyboard/screen readers when it is in the closed state (e.g., `display:none` or `aria-hidden="true"` applied).

### Sticky header behavior on scroll

- Load the homepage, scroll down 400 px, and confirm the header becomes sticky (fixed to the top of the viewport).
- Verify the sticky header does not cover any anchor-linked section headings (check offset/scroll-padding-top is accounted for).
- Confirm the sticky header's height in sticky mode is the same as or intentionally smaller than its initial height, and that no layout shift occurs when it transitions.
- Scroll back to the top and verify the header returns to its expanded/full state if applicable (non-sticky state).
- Test sticky behavior on all main page types: homepage, category page, product page, checkout — confirm the header is consistent.
- On mobile, confirm the sticky header does not eat too much vertical space (max ~56–64 px acceptable on a 375 px viewport).
- Check the CLS (Cumulative Layout Shift) score: the sticky transition must not cause a visible jump in page content.
- Verify that the cart icon, search field, and account link inside the sticky header remain fully functional (clickable, correct positioning).
- Scroll to the very bottom of a long page and back to the top — confirm no JavaScript errors related to scroll event listeners appear in the browser console.
- On Safari iOS with rubber-band scrolling: confirm the sticky header does not flicker or disappear at the top overscroll.

### Logo links to homepage

- Click the logo on every major page type (homepage, category, product, cart, checkout, account, CMS page) and confirm it redirects to the homepage root URL.
- Verify the logo `href` points to the canonical homepage URL (e.g., `https://www.example.com/`) without unnecessary query strings or redirect hops.
- Confirm the logo image loads correctly (no broken image), has appropriate alt text (brand name), and does not generate a 404 for its source file.
- On the homepage itself, verify the logo either has no click action or links to the homepage without causing a full reload that resets scroll position unexpectedly (accessibility: logo on homepage should ideally be non-linked or announced appropriately to screen readers).
- Check that the logo is visible in both the default and sticky header states (correct size, no clipping, no overlap with other elements).
- Verify the logo link has a visible focus style when tabbed to (keyboard navigation).
- On mobile, confirm the logo is proportionally sized and fully visible in the hamburger-menu-closed state.

### Cart icon with counter

- Add one product to the cart; verify the cart icon counter increments to 1 without a page reload.
- Add a second product; confirm counter shows 2.
- Remove a product from the mini-cart or full cart; confirm counter decrements correctly.
- Add a product with quantity 3; confirm whether the counter shows 1 (distinct items) or 3 (total quantity), consistent with the intended UX specification.
- Empty the cart completely; confirm the counter either disappears or shows 0 as specified.
- Reload the page after adding items; confirm the counter persists (session/cookie-based).
- Log in as a returning customer who already has items in their cart; confirm the counter reflects the correct existing cart count.
- Click the cart icon; confirm it opens the mini-cart overlay or navigates to the full cart page as designed.
- On mobile sticky header, confirm the cart icon and counter are legible and tappable (touch target ≥ 44 × 44 px).
- Verify counter badge does not overflow or visually break for two-digit numbers (e.g., 10, 25).
- Check screen-reader announcement: the cart icon should include an accessible label such as "Shopping cart, 3 items".

### Wishlist icon

- Click the wishlist icon while not logged in; confirm the expected behavior (redirect to login, guest wishlist, or tooltip prompt as designed).
- Log in, click the wishlist icon, and verify it navigates to the wishlist page.
- Add a product to the wishlist; confirm a visual indicator (filled heart, counter badge) appears on the wishlist icon.
- Remove all items from the wishlist; confirm the indicator resets to the empty state.
- Verify the wishlist icon has an accessible label (e.g., `aria-label="Wishlist"`) and a visible focus style.
- On mobile, confirm the wishlist icon is tappable (min 44 × 44 px touch target) and does not overlap other header elements.
- Reload the page after adding a wishlist item; confirm the indicator persists for logged-in users.
- Add the same product twice; confirm it is not duplicated in the wishlist (idempotent).

### Account icon / login link

- Click the account icon while logged out; confirm it navigates to the login page (or opens a login dropdown as designed).
- Log in and click the account icon; confirm it navigates to the customer account dashboard.
- Verify the account icon/label changes state when the user is logged in (e.g., shows the user's name or a filled icon).
- Click "Logout" from the account dropdown; confirm the session is destroyed and the icon reverts to the logged-out state.
- Verify the login link includes the current page URL as a `redirect` parameter so the user returns to the right page after login.
- Confirm the account icon has a clear accessible label ("My account", "Login") visible to screen readers.
- On mobile, confirm the account icon is visible in the hamburger header area and tappable.
- Verify that the account dropdown (if used) closes on Escape and on outside click.
- While logged in, navigate to a restricted page (e.g., order history) directly via URL; confirm access is granted without redirect loop.
- While logged out, navigate to a restricted page directly via URL; confirm redirect to login with the return URL preserved.

### Search field in header

- Click/tap the search field; confirm it receives focus and a cursor appears.
- Type a valid search term (e.g., a product name); confirm autocomplete suggestions appear after 2–3 characters.
- Select an autocomplete suggestion with the mouse; confirm navigation to the correct product or search results page.
- Press Enter after typing a search term; confirm navigation to the search results page with the correct query in the URL.
- Submit an empty search; confirm either no action, a validation message, or an appropriate empty-state results page — no error.
- Type a term with special characters (e.g., `<script>alert(1)</script>`); confirm the input is sanitized and no XSS occurs.
- Type a very long string (200+ characters); confirm the input field handles it gracefully (truncation, no layout break).
- On mobile, tapping the search icon should expand the search field or navigate to a dedicated search page; confirm the keyboard opens automatically.
- Verify the search field placeholder text is descriptive and disappears on focus.
- Confirm the search field has an associated `<label>` or `aria-label` for screen readers.
- Press Escape while the autocomplete dropdown is open; confirm it closes without closing the entire search field or navigating away.
- Verify the search field does not submit on Tab key press (Tab should move focus to the next element, not trigger search).

### Category tree fully navigable

- From the top-level navigation, traverse every first-level category and confirm the category page loads with the correct products.
- From each first-level category, navigate to each second-level (sub-) category and confirm the correct filtered product list loads.
- For any third-level categories, confirm navigation works correctly and breadcrumbs reflect all three levels.
- Verify there are no "dead ends": every category with sub-categories shows those sub-categories accessible either in the nav or on the category page itself.
- Check that empty categories either display a friendly "no products found" message or are hidden from the navigation (per design decision).
- Verify that the URL slug for each category is human-readable and matches the category name.
- Confirm that category pages show the correct category name as H1, matching the menu label.
- Test navigation using only the keyboard (Tab + Enter through the category tree) to confirm full accessibility.
- After navigating three levels deep, use the browser back button and confirm the previous category page loads correctly with its former scroll position or at least at the top.
- Verify that the category tree in the mobile hamburger menu matches the desktop mega-menu (same categories, same hierarchy).

### Breadcrumb navigation on all pages

- Visit a third-level category page and confirm the breadcrumb shows: Home > Level 1 Category > Level 2 Category > Level 3 Category.
- Visit a product detail page and confirm the breadcrumb shows the full category path leading to that product.
- Click each breadcrumb link and verify it navigates to the correct parent page.
- Verify the last breadcrumb item (current page) is not a link (or is aria-current="page") and matches the page H1.
- Visit a CMS page (e.g., About Us) and confirm the breadcrumb shows: Home > About Us.
- Verify the breadcrumb is marked up with `<nav aria-label="Breadcrumb">` and uses an ordered list (`<ol>`).
- Confirm breadcrumb structured data (Schema.org BreadcrumbList) is present in the page source and validates without errors in Google's Rich Results Test.
- On mobile, confirm the breadcrumb is readable and does not overflow horizontally — long paths should truncate or wrap gracefully.
- On the homepage, confirm no breadcrumb is shown (or only "Home" if designed that way).
- Verify breadcrumbs update correctly when a product belongs to multiple categories and is reached via different navigation paths.
- Confirm breadcrumb links use the canonical URL of each parent page (no session IDs or tracking parameters in breadcrumb hrefs).
