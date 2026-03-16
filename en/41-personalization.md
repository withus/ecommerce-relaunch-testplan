# 41. Personalization & Recommendations

### "Recently viewed" widget correctly populated

- Browse 5 different products and verify all 5 appear in the "Recently viewed" widget in correct reverse-chronological order (most recently viewed first).
- Browse 1 product as a guest, then log in; verify the recently viewed history is either preserved or merged with any server-side history for that account.
- Browse 20+ products and confirm the widget caps its display at the configured maximum (e.g., 10 items) and does not overflow its container.
- Revisit a product already in the recently viewed list and verify it moves to the top position rather than appearing as a duplicate.
- Clear browser cookies/local storage (guest session) and verify the recently viewed widget is empty on next visit — no data leaks from a previous anonymous session.
- As a logged-in user on device A, view 3 products; switch to device B, log in with the same account, and verify the recently viewed list is synchronized cross-device (if server-side persistence is implemented).
- Add a product to recently viewed, then delete or unpublish that product in the admin; verify the widget handles the missing product gracefully (no broken image, no PHP/JS error, ideally the item is silently removed).
- Navigate to a category page, a CMS page, and the cart; verify that only product detail page visits populate the widget, not non-product pages.
- Verify the "Recently viewed" widget does not appear (or appears empty with a fallback message) on a brand-new session before any products have been visited.
- Check that the widget renders correctly and all images load on mobile (portrait and landscape), tablet, and desktop breakpoints.
- Inspect network requests to confirm the recently viewed data is fetched without exposing other customers' browsing data (no shared cache keys between users).
- Confirm that when JavaScript is disabled, the "Recently viewed" section either degrades gracefully (server-side rendering) or is cleanly hidden — no broken layout.

### "Similar products" recommendations relevant

- Open a product in a specific category (e.g., "Running Shoes"); verify that all recommended "similar products" belong to the same or a closely related category, not unrelated categories.
- Open a product with a specific price point; verify that the similar products displayed are within a comparable price range (within ~30–50%) and do not surface items that are wildly more expensive or cheaper.
- On a product with only one item in its category, verify the similar products widget either shows reasonable cross-category alternatives or displays an appropriate empty/fallback state — no broken widget.
- Switch to a product variant (e.g., color or size); verify that the similar products list either updates to reflect the variant context or remains stable and relevant — it must not show the same product under a different variant as a "similar" recommendation.
- Check that similar products recommendations exclude out-of-stock items (or clearly label them as unavailable) so customers are not directed to products they cannot buy.
- Verify that the similarity algorithm is not purely based on the same manufacturer, which would make the widget a hidden upsell tool rather than a genuine discovery aid — at least some recommendations should come from different brands if category similarity warrants it.
- Inspect the widget on a product page where the product belongs to multiple categories; verify that the recommendations reflect a balanced mix rather than only one of the categories.
- After a cold start (new shop, sparse data), verify that a fallback recommendation strategy (e.g., bestsellers from the same category) is active and the widget does not render empty.
- Confirm that recommendations are not static/cached per product page in a way that never updates — simulate a new product addition and verify it can surface as a similar product within a reasonable time frame.
- Check the widget for duplicate entries: no single product should appear twice in the same recommendations list.
- Verify that the similar products widget respects customer-group visibility rules — a B2B-restricted product should not appear as a recommendation to a B2C guest.

### "Customers also bought" logic

- Purchase product A; log out; verify that product A's page now shows the co-purchased item(s) when there is sufficient purchase co-occurrence data.
- Verify that the "Customers also bought" widget only reflects actual completed purchases (order status: paid/fulfilled), not abandoned carts or wishlist adds.
- Check that the widget excludes the product currently being viewed — it must not suggest the page's own product as a co-purchase recommendation.
- Simulate a scenario where a product has been bought only once, alone (no co-purchase data); verify the widget falls back gracefully (bestsellers, empty state, or hidden) rather than showing an error or irrelevant data.
- For a product with many co-purchase associations, verify the widget limits displayed items to the configured maximum (e.g., 4–6 items) and does not overflow the layout.
- Place an order containing 5 different products; verify that over time each of those 5 products shows the others as "customers also bought" recommendations (mutual co-purchase relationships).
- Verify the recommendation data updates within a defined SLA (e.g., within 24 hours of a new batch of orders) and is not permanently static.
- Check that a returned/refunded order is eventually excluded from co-purchase signal data, so a fraudulent or test purchase does not pollute recommendations.
- Confirm the widget is not visible to users before any purchase co-occurrence data exists (first week of a new shop) — a meaningful empty or fallback state should display instead.
- Inspect for privacy compliance: the widget must not expose which individual customer bought what; only aggregate co-occurrence data should drive recommendations.
- Verify cross-device/cross-session correctness: the same product page seen by two different users should produce the same "customers also bought" output (it is aggregate, not personal), unless the system implements hybrid personalization.

### "Frequently bought together" bundles

- Navigate to a product that has a "frequently bought together" bundle configured; verify the bundle section displays all included products with individual prices and a combined bundle price/discount clearly visible.
- Add the entire "frequently bought together" bundle to the cart in one click; verify that all products are added with correct quantities and that the bundle discount (if any) is applied in the cart.
- Deselect one item from the "frequently bought together" bundle UI; verify the total price updates in real time to reflect the revised selection, and that only selected items are added to cart.
- Verify that bundle pricing arithmetic is correct: sum of individual prices minus bundle discount equals the displayed bundle total, with no rounding errors.
- Check that if one product in the bundle is out of stock, the bundle UI clearly indicates this (greyed-out checkbox, "unavailable" label) and prevents that specific item from being added while still allowing purchase of the available items.
- Navigate to a product with no "frequently bought together" data; verify the bundle section is completely hidden — no empty container or broken widget is visible.
- Add a bundle to the cart; then navigate back and add the same bundle again; verify that the cart handles duplicate additions sensibly (merges quantities or warns the customer, depending on product type).
- Verify that bundle products are individually navigable (each product in the bundle widget links to its own product page).
- On mobile, verify that the "frequently bought together" layout does not overflow horizontally and all checkboxes/images are accessible with touch targets of at least 44×44 px.
- Check that bundle recommendations are merchandised consciously (complementary products, not random) — audit a sample of 10 bundles to ensure logical product pairings.
- Verify that the bundle discount is not stackable with other incompatible coupon codes unless explicitly intended, and that the cart validation reflects this rule.

### Personalized homepage based on browsing history

- As a new visitor (no history), verify the homepage shows a neutral/default state (bestsellers, featured products, or category highlights) rather than an empty personalization section.
- Browse products in category "Laptops" across 3 sessions; on the next visit verify that the homepage surfaces laptop-related recommendations prominently.
- Log in as a user who has never interacted with the shop before (fresh account, no orders, no browses); verify the homepage personalization section uses a sensible cold-start fallback (e.g., popular items) and is clearly labeled (e.g., "Trending now") rather than falsely claiming "Recommended for you."
- Verify that personalized sections are labeled honestly and transparently ("Based on your browsing" or "Because you viewed X") so users understand why they see specific recommendations.
- Clear browsing history / cookies and revisit the homepage; verify that personalization resets to the default state immediately and no prior session data bleeds through.
- Log in from an incognito window; verify that incognito sessions do not automatically inherit personalization from a previous logged-out session stored in regular browser storage.
- Check that the personalized homepage section does not display products the user has already purchased (unless repurchase makes sense for the product type) — completed purchase items should be filtered from "recommendations."
- Verify performance: the personalized homepage must not significantly increase Time to First Byte or LCP compared to the non-personalized version; personalization data should load asynchronously if needed.
- Inspect that the personalization engine does not expose sensitive inference (e.g., health or financial product categories) in a way that could embarrass or profile users — validate that privacy guidelines are followed for sensitive product categories.
- Verify that personalized content respects the user's cookie consent choices: if the user has declined tracking/personalization cookies, the homepage must not display behavior-based recommendations.
- Test with a user who has browsed only one product: verify the personalization section still renders correctly (no JS error, no empty broken widget) and shows reasonable recommendations based on that single signal.

### Recommendation widgets on 404 page and empty search results page

- Navigate to a non-existent URL (e.g., `/products/this-does-not-exist`); verify the 404 page displays a recommendation widget (e.g., "You might like these" or "Popular products") with at least 4 valid, in-stock products.
- Search for a term that returns zero results (e.g., a random string); verify that the empty search results page displays a recommendation widget with relevant or popular products, plus a clear "No results found for X" message.
- On the 404 page, verify that the recommendation widget is not the only content — the page must also include a search field, navigation links, and a link back to the homepage.
- Check that the recommendations on the 404 page are not hardcoded/static but reflect current availability — if recommended products go out of stock, they are replaced or removed.
- On the empty search results page, verify that the recommendation widget ideally shows products related to the failed search term (semantic fallback) rather than completely unrelated items; if no semantic matching is implemented, at least bestsellers from the site should appear.
- Verify that both the 404 and empty-search recommendation widgets render correctly on mobile, tablet, and desktop without overflow or broken images.
- Confirm that 404-page visits with recommendation widget loads are not counted as valid page views in analytics (the 404 status code should still be returned as HTTP 404, not 200).
- Check that the empty search results page clearly communicates search improvement suggestions (check spelling, try broader terms) alongside the recommendation widget — the widget must supplement, not replace, helpful guidance.
- Verify that recommendation widgets on these pages respect the user's consent status: if personalization cookies are declined, the widgets must fall back to non-personalized content.
- Test the 404 page with a logged-in user vs. a guest: if the system supports personalized 404 recommendations, verify that the logged-in user sees relevant history-based suggestions while the guest sees generic popular items.
- Verify that the recommendation widgets on the 404 and empty-search pages do not cause JS errors in the browser console — these pages are often overlooked in regression testing.

### A/B testing capability for recommendation algorithms

- Verify that the A/B testing framework can assign visitors to algorithm variants (e.g., collaborative filtering vs. content-based) in a statistically consistent manner — a visitor assigned to variant A on visit 1 must remain in variant A on subsequent visits (sticky assignment).
- Confirm that test variant assignment is handled server-side or via a persistent identifier (user ID or stable cookie) so that variant switching does not occur mid-session or across page loads.
- Verify that the A/B test configuration (which variants exist, what percentage of traffic each receives) is manageable without a code deployment — it should be configurable via an admin panel or feature-flag system.
- Check that A/B test events are correctly tracked in the analytics platform: each recommendation interaction (impression, click, add-to-cart from recommendation) must be tagged with the variant identifier so results can be analyzed per variant.
- Confirm that A/B testing does not contaminate the control group: users in the control group must receive the original recommendation logic with no changes.
- Verify that stopping an A/B test (removing a variant) does not cause errors or empty widget states for users who were previously assigned to the discontinued variant — graceful fallback to the winning or default algorithm must occur.
- Test that when multiple recommendation widgets are present on a single page (e.g., "Similar products" and "Customers also bought"), they can be A/B tested independently without interference.
- Verify that the A/B test respects GDPR: if the user has not consented to personalization/tracking cookies, they should either be excluded from the test (placed in a neutral state) or the test must be documented as a functional test that requires no additional consent under the applicable legal basis.
- Check that sample size and statistical significance guidelines are documented for the team — a test should not be "won" or "lost" before reaching the minimum required sample size.
- Simulate a new deployment that introduces a bug in one recommendation algorithm variant; verify that the system allows for quick disabling of that variant (kill switch) without affecting other variants or the rest of the site.
- Verify that A/B test assignments are logged and auditable — it must be possible to reconstruct which variant a specific user (by ID) was in at any given time, for debugging and support purposes.
- Confirm that the A/B testing mechanism itself does not introduce measurable performance degradation (additional latency per page load must be under a defined threshold, e.g., <50 ms).
