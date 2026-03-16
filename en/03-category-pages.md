# 3. Category Pages

### Category overview display

- Navigate to a top-level category URL and verify the page title matches the category name
- Confirm the category H1 heading is present, unique, and matches the page title
- Verify that all child categories (subcategories) are displayed with correct names, images, and links
- Check that a category with 0 products either shows an appropriate empty state or is hidden from the overview
- Confirm the category overview layout renders correctly at 320px, 768px, 1024px, and 1440px viewport widths
- Verify category cards/tiles display consistent image sizes and aspect ratios across all entries
- Check that clicking a category card navigates to the correct category URL
- Confirm no broken images appear in the category overview grid
- Verify the page loads without JavaScript errors in the browser console

### Subcategory navigation

- From a top-level category page, click each visible subcategory link and confirm it leads to the correct page
- Verify the subcategory page itself shows its own sub-subcategories (if applicable) in a navigation block
- Confirm the active/current category is visually highlighted in any sidebar or breadcrumb navigation
- Check that the browser back button returns to the correct parent category page without full reload side effects
- Verify deep nesting (3+ levels) renders without layout overflow or broken navigation
- Confirm subcategory links are keyboard-accessible via Tab and activatable with Enter
- Check that subcategory navigation is present and usable on mobile without horizontal scrolling

### Product list correct display

- Open a category that has products and verify each product card shows: product image, product name, price, and an action button
- Confirm all product cards are visually consistent in height and alignment across the grid
- Verify product names are not truncated in a way that hides critical information (or confirm tooltip/title attribute is present)
- Check that clicking a product card navigates to the correct product detail page URL
- Verify the product count displayed on the category page (e.g., "42 products") matches the actual number of visible items
- Confirm that products with multiple variants show the correct default variant information (price, image, availability)
- Check that the product grid reflows correctly when the browser is resized between desktop and mobile breakpoints

### Product images load correctly

- Scroll through the full product listing and confirm no product image returns a 404 or renders as a broken image icon
- Verify images use lazy loading (images below the fold are not loaded until the user scrolls to them)
- Confirm images above the fold are NOT lazy-loaded (they must load immediately for LCP performance)
- Check that images render in the correct aspect ratio and are not stretched or distorted
- Verify that WebP or AVIF format images are served to browsers that support them, with JPEG/PNG fallback otherwise
- Confirm image alt text is present and descriptive (not empty, not just the filename) for each product card image
- Resize the browser to mobile and verify images scale proportionally without overflow

### Price display (incl. RRP, discount, unit price)

- Verify the current selling price is displayed clearly on every product card
- For products with a recommended retail price (RRP), confirm the original price is shown with a strikethrough style
- Check that the discount amount or percentage badge (e.g., "-20%") is displayed where applicable and the calculation is correct
- Verify unit price (e.g., price per kg, per m², per piece) is shown for products that require it under applicable law
- Confirm that VAT inclusion note (e.g., "incl. 19% VAT") is displayed where legally required
- Check prices are formatted correctly: correct currency symbol, correct decimal separator, no rounding errors
- For products with price ranges (multiple variants), verify the display shows "from X.XX €" or the correct range format
- Add a discounted product to cart and verify the cart reflects the same discounted price shown on the category page

### Sorting (price ascending/descending, popularity, newest, name, rating)

- Open the sort dropdown and confirm all expected sort options are present: price low-to-high, price high-to-low, newest, popularity, name A-Z, name Z-A, average rating
- Select "Price: Low to High" and verify the first product has the lowest price and prices increase through the list
- Select "Price: High to Low" and verify the first product has the highest price and prices decrease through the list
- Select "Newest" and verify products are ordered by most recently added (check dates if visible, or confirm with known newly added product)
- Select "Name A-Z" and verify products appear in alphabetical order
- Select "Name Z-A" and verify products appear in reverse alphabetical order
- Select "Popularity" or "Best Sellers" and confirm the order is consistent and not random
- Select "Average Rating" and verify higher-rated products appear before lower-rated ones
- Change the sort order and confirm the URL updates (e.g., `?sort=price_asc`) so the sorted view is bookmarkable and shareable
- Navigate away and use the browser back button; confirm the previously selected sort order is restored
- Verify the active sort option is visually indicated in the dropdown/menu

### Filtering (price range, color, size, material, brand, rating)

- Open the filter panel and confirm all expected filter groups are present for the category (price, color, size, material, brand, rating)
- Set a price range filter using the slider or input fields and verify only products within that range are shown
- Enter a minimum price higher than all products' prices and verify the empty state is displayed correctly
- Select a color filter (e.g., "Red") and verify all displayed products are available in that color
- Select a size filter (e.g., "L") and verify all displayed products offer that size
- Select a material filter and verify results match; check a known product of that material is present
- Select a brand filter and verify only products from that brand appear
- Select a 4-star-or-above rating filter and verify no products with fewer than 4 stars are shown
- Confirm filter options that would yield 0 results are either hidden, grayed out, or show a "(0)" count
- Verify the product count on the page updates correctly after each filter is applied
- Confirm filter state is reflected in the URL (e.g., `?color=red&size=L`) for bookmarkability
- Reload the page with filter parameters in the URL and verify the same filters are pre-selected and results are filtered

### Filter combinations (multiple filters simultaneously)

- Apply two filters simultaneously (e.g., color "Blue" + size "M") and verify only products meeting both criteria are shown
- Apply three or more filters and verify the result set is the correct intersection of all active filters
- Apply a price range plus a brand filter and confirm results satisfy both conditions
- Apply filters that together yield 0 matching products and verify the empty state message is displayed (not a blank page or error)
- Verify the active filter count or summary (e.g., "3 filters active") is displayed somewhere on the page
- Confirm that changing one filter while others are active updates the product list without resetting the other filters
- Test a filter combination on mobile and verify the filter panel closes or collapses correctly after applying, revealing the updated product list

### Reset filters

- Apply one or more filters, then click "Reset filters" or "Clear all" and verify all filters are removed and the full product list is restored
- Verify the URL is updated (filter parameters removed) after reset
- Apply a filter, then manually remove it by deselecting (clicking the active filter again or clicking an "X" on a filter tag) and verify the product list updates correctly
- Confirm that "Reset filters" button is only visible when at least one filter is active; it should not appear on the unfiltered page
- After resetting filters, verify the selected sort order is preserved (reset should only clear filters, not sorting)
- Verify the "Reset" action works correctly on mobile without page reload side effects

### Pagination / infinite scroll

- On a category with more products than fit on one page, verify pagination controls (page numbers, previous/next) or infinite scroll trigger are present
- Click "Page 2" and verify a different set of products is displayed with no duplicates from page 1
- Verify the URL updates to reflect the current page (e.g., `?page=2`) so it is bookmarkable
- Navigate directly to `?page=3` via URL and verify the correct products for that page are shown
- On the last page, verify the "Next" button is disabled or hidden
- On page 1, verify the "Previous" button is disabled or hidden
- If infinite scroll is used: scroll to the bottom and verify the next batch of products loads automatically without a full page reload
- Verify a loading spinner or skeleton placeholder appears while the next page/batch loads
- After navigating to a product and returning via the back button, verify the user is returned to the same page and scroll position they were on (pagination state is preserved)
- Verify pagination links are crawlable by search engines (actual `<a href>` links, not only JavaScript triggers)

### Products per page (change quantity)

- Locate the "Products per page" selector (e.g., 24 / 48 / 96) and confirm it is present on the category page
- Select "48 products per page" and verify the page now shows up to 48 products instead of the default
- Verify the URL is updated (e.g., `?per_page=48`) so the preference is shareable
- Confirm the pagination total updates correctly after changing products per page (e.g., fewer pages needed if showing more items)
- Select the maximum products per page value and verify the page does not time out or error with large result sets
- Verify the selected products-per-page value persists across pagination (going to page 2 still shows the same count per page)

### Grid view / list view toggle

- Locate the view toggle (grid/list icons) on the category page and confirm both options are present
- Click "List view" and verify each product switches to a horizontal layout showing more detail (description snippet, larger image, specifications)
- Click "Grid view" and verify the layout returns to the multi-column card grid
- Confirm the active view mode is visually indicated (e.g., icon is highlighted)
- Verify the selected view mode is saved to a cookie or localStorage and persists when navigating to another category and returning
- Confirm both views are usable and correctly laid out on mobile (list view may default to single-column on small screens)
- Verify all product actions (add to cart, add to wishlist, quick view) are accessible in both view modes

### Empty category display

- Navigate to a category that exists but has no products assigned (or filter to produce 0 results) and verify a helpful message is shown (e.g., "No products found in this category")
- Confirm the empty state page does NOT show a 404 error; the category page itself should still load with HTTP 200
- Verify the empty state includes helpful navigation options such as "Browse similar categories" or a link back to the parent category
- Check the page still displays the category name, description, and any breadcrumbs correctly even when empty
- Confirm there is no broken layout, missing elements, or JavaScript errors on the empty category page

### Category description text (SEO)

- Open a category that has a description configured and verify the text is visible on the page (commonly above or below the product grid)
- Verify the description text contains the correct content and is not truncated unexpectedly
- Confirm the description uses correct heading hierarchy (e.g., it does not use an H1, which should belong only to the category name)
- Check the description text is indexable by search engines (not hidden via CSS `display:none` or inside a collapsed element that is invisible to crawlers)
- Verify the description renders HTML formatting correctly (bold, bullet lists, links) if the content is stored as rich text
- Confirm links inside the category description are functional and lead to the correct destinations

### Category image / banner

- Verify the category banner image loads correctly at full width without distortion or pixelation at desktop resolution
- Check the banner image is responsive and scales correctly on tablet and mobile viewports
- Confirm the banner image has a descriptive alt text attribute (not empty)
- Verify the banner image does not cause a Cumulative Layout Shift (CLS) — the space for the image should be reserved before it loads
- Check that text overlaid on the banner (if any) has sufficient color contrast against the image background (WCAG AA minimum 4.5:1 for normal text)
- Verify the banner image is served in an optimized format (WebP/AVIF) and is not excessively large in file size

### Quick view

- Hover over a product card and confirm the "Quick view" button appears
- Click "Quick view" and verify a modal or overlay opens with the product name, image, price, and variant options
- Confirm the modal displays correctly on desktop without overflow or clipping
- Verify that selecting a variant inside the quick view modal (e.g., color or size) updates the displayed price and image
- Click "Add to cart" inside the quick view modal and confirm the product is added to the cart correctly and the cart counter increments
- Close the quick view modal using the close button (X) and verify the underlying page is still intact and scrollable
- Press the Escape key and verify the modal closes
- Verify the quick view modal is accessible: can be opened and operated via keyboard, has ARIA role="dialog" and a focus trap
- Test quick view on mobile — confirm the trigger works correctly (tap) and the modal is fully usable on small screens
- Confirm the quick view modal does NOT open as a new page or cause a full page navigation

### Add to wishlist

- Locate the wishlist icon/button on a product card and click it; verify a visual confirmation appears (icon changes color/style, or a toast notification appears)
- Navigate to the wishlist page and confirm the product now appears there
- Click the wishlist button again on the same product and verify it is removed from the wishlist (toggle behavior)
- Verify the wishlist action works without requiring a login (guest wishlist via cookie/localStorage)
- Log in as a registered user, add a product to wishlist, log out, log back in, and confirm the wishlist item persists (server-side storage)
- Verify the wishlist icon shows a counter or indicator when items are in the wishlist
- Check that adding to wishlist does NOT add the product to the cart

### Add to cart directly from category page

- Click the "Add to cart" button on a product card (for a product with no required variant selection) and verify the cart counter increments by 1
- Confirm a success notification (toast or mini-cart flyout) appears after adding to cart
- Verify the cart mini-preview or flyout shows the correct product name, image, quantity, and price
- Attempt to add a product that requires a variant selection (e.g., size) directly from the category page; verify the user is either redirected to the product page or prompted to select the required option before adding
- Add the same product from the category page multiple times and verify the cart quantity increments correctly (not duplicated as separate line items)
- Add a product to cart while logged out, then log in, and verify the cart item persists and is not lost
- Verify the "Add to cart" button is disabled or replaced with "Out of stock" for products with zero inventory
- Check that the cart total in the header updates immediately after adding a product from the category page
