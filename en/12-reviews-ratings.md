# 12. Reviews / Ratings

### Submit review (stars + text)

- Navigate to a product page as a logged-in, verified buyer — confirm the review submission form is visible and accessible.
- Submit a review with a 5-star rating and a text body of 10–500 characters — confirm the submission is accepted and the review enters the moderation queue (or displays immediately if moderation is disabled).
- Submit a review with a 1-star rating and a short negative text — confirm all star values (1–5) are selectable and saved correctly.
- Attempt to submit a review with stars selected but no text — if text is required, confirm a validation error is shown; if text is optional, confirm the review is accepted.
- Attempt to submit a review with text entered but no star rating selected — confirm a validation error is shown and submission is blocked.
- Attempt to submit a review with the text field completely empty and no stars — confirm the form cannot be submitted and both fields show validation errors.
- Submit a review text containing leading/trailing whitespace only — confirm the system either trims it and treats it as empty (triggering validation) or rejects it explicitly.
- Submit a review as a guest (not logged in) — confirm access to the form is blocked or the user is redirected to login, depending on shop policy.
- **[Security]** Attempt to submit a second review for the same product with the same account — confirm the system blocks it with a clear message (e.g., "You have already reviewed this product").
- **[Security]** As User A, attempt to POST a review submission with User B's account ID in the payload (e.g., via modified form data or API call) — confirm the server ignores the supplied user ID and attributes the review to the authenticated session owner, or rejects the request entirely.
- Confirm the review submission form includes CSRF protection (token present in form, missing/invalid token causes rejection).
- **[Security — XSS]** Submit a review text containing `<script>alert('xss')</script>` — confirm the script tag is not executed on the product page and is either stripped or HTML-encoded in the output.
- **[Security — XSS]** Submit a review text containing `"><img src=x onerror=alert(1)>` — confirm the payload is sanitized and no JavaScript executes on any page that displays the review.
- **[Security — XSS]** Submit a review with Markdown-style injection (`[click](javascript:alert(1))`) — confirm it is rendered safely (no executable link).
- Submit a review text that exactly meets the maximum character limit — confirm it is accepted.
- Submit a review text that exceeds the maximum character limit by one character — confirm it is rejected with a clear length validation error (not a silent truncation or server error).
- Verify the character counter (if present) updates in real time as the user types and correctly prevents submission when the limit is exceeded.
- Submit a review containing special characters and Unicode (e.g., German umlauts, emoji, Arabic text) — confirm they are stored and displayed correctly without corruption.
- Submit a review containing SQL injection patterns (e.g., `'; DROP TABLE reviews; --`) — confirm the text is stored as a literal string and no database error occurs.
- Confirm the review form is accessible via keyboard (Tab to all fields, Enter/Space to select stars, Enter to submit) and that star ratings are operable without a mouse.

### Review with image/video

- Submit a review and attach a valid image file (JPEG, PNG, WebP) within the allowed size limit — confirm the image uploads successfully and is associated with the review.
- Submit a review and attach a valid video file (MP4) within the allowed size limit — confirm the upload is accepted (if video is supported) or rejected with a clear message (if not).
- Attempt to upload a file with a disallowed MIME type (e.g., `.exe`, `.php`, `.svg`) — confirm the upload is rejected with a descriptive error and no file is stored on the server.
- **[Security]** Attempt to upload a file named `shell.php` with a `.jpg` extension (content-type spoofing) — confirm the server validates the actual file content, not just the extension, and rejects or safely stores it with no execution risk.
- Attempt to upload an image that exceeds the maximum allowed file size — confirm a size validation error is shown before or after upload, and the oversized file is not stored.
- Attempt to upload more images than the allowed maximum per review — confirm excess files are rejected and a clear limit message is shown.
- Submit a review without any media attachment — confirm this is still valid (media is optional unless stated otherwise).
- After a review with an image is approved and displayed, verify the image renders correctly on the product page and does not expose the server's internal file path in the URL.
- Confirm uploaded media files are stored outside the web root or behind access controls so that direct path traversal cannot retrieve arbitrary server files.
- Verify that uploaded images are stripped of EXIF metadata (location data, device info) before being served to end users.

### Reviews only for verified buyers

- Log in as a customer who has completed a purchase of Product A — confirm the review form for Product A is accessible.
- Log in as a customer who has NOT purchased Product A — confirm the review form is hidden, disabled, or shows a message like "Only verified buyers can review this product."
- Log in as a customer who purchased Product A but whose order was cancelled or refunded — confirm the system checks order status and denies review access if the policy excludes non-fulfilled orders.
- Attempt to access the review submission endpoint directly (e.g., via API or direct POST) for a product the authenticated user has not purchased — confirm the server-side verification rejects the request with an appropriate HTTP error (e.g., 403 Forbidden), not just a UI check.
- Log in as a customer who purchased Product A multiple times (multiple orders) — confirm they can submit only one review regardless of purchase count.
- Verify that the verified-buyer badge is displayed next to reviews submitted by confirmed purchasers, and is absent on reviews submitted through any guest or non-purchase path (if such a path exists).
- After a purchase is made, check how quickly review eligibility is activated (e.g., immediately after payment, after delivery confirmation) — confirm the timing matches the documented policy.

### Review moderation (not immediately visible)

- Submit a review as a verified buyer — confirm the review does not appear publicly on the product page immediately after submission.
- After submission, confirm the user sees a confirmation message such as "Your review has been submitted and is awaiting approval."
- Log in to the admin backend and locate the submitted review in the moderation queue — confirm it appears with the correct content, author, product, and timestamp.
- Approve a review in the admin backend — confirm it becomes visible on the product page within a reasonable time (accounting for any caching).
- Reject a review in the admin backend — confirm it does not appear on the product page and (if applicable) the user receives a notification email.
- Submit a review containing obvious spam or prohibited content — confirm the moderation queue flags or auto-rejects it (if automated pre-filtering is active).
- Verify that the average star rating on the product page is only updated after a review is approved, not at the time of submission.
- Confirm that a review pending moderation is not indexed by search engines (not rendered in the page HTML or structured data before approval).
- Verify that admins cannot accidentally approve a review without the required content (e.g., approve an empty text review).

### Edit review

- As the review author, navigate to a submitted and approved review — confirm an "Edit" option is present and functional.
- Edit the star rating of an existing review — confirm the change is saved and the updated rating is reflected in the product's average score after re-moderation (if edits trigger re-moderation).
- Edit the text of an existing review — confirm the updated text replaces the old text on the product page after approval.
- **[Security]** As User B, attempt to edit a review authored by User A by manipulating the review ID in the URL or request payload (IDOR test) — confirm the server returns 403 Forbidden and no change is made to User A's review.
- **[Security]** As an unauthenticated user, attempt to send an edit request to the review API endpoint — confirm the server returns 401 Unauthorized.
- Attempt to edit a review and submit an XSS payload in the updated text — confirm the output is sanitized as with the original submission.
- Confirm that editing a review updates the "last modified" timestamp (if displayed) and does not create a duplicate review.
- Attempt to edit a review after the allowed editing period has expired (if a time limit exists) — confirm the edit is blocked with a clear message.

### Delete review

- As the review author, delete their own review — confirm the review is removed from the product page and the average rating is recalculated.
- **[Security]** As User B, attempt to delete a review authored by User A via a direct DELETE request or manipulated form submission (IDOR test) — confirm the server returns 403 Forbidden and User A's review remains intact.
- **[Security]** As an unauthenticated user, attempt to send a DELETE request to the review endpoint — confirm the server returns 401 Unauthorized.
- As an admin, delete a review from the backend — confirm it is removed from the product page and the rating average updates correctly.
- After a review is deleted, confirm it is permanently removed (or soft-deleted and hidden) and does not reappear after a cache flush.
- Confirm that deleting the only review for a product results in the average rating being removed or reset to zero/no-rating on the product page.
- Verify that structured data (Review schema) is updated or removed after deletion so that search engines do not display deleted reviews.

### Sort reviews (newest, best, worst)

- On a product page with multiple reviews, select "Newest" sort — confirm reviews are ordered chronologically descending (most recent first).
- Select "Oldest" sort (if available) — confirm reviews are ordered chronologically ascending.
- Select "Best" (highest rating first) — confirm 5-star reviews appear first, followed by 4-star, and so on.
- Select "Worst" (lowest rating first) — confirm 1-star reviews appear first.
- Select "Most helpful" sort (if available) — confirm the ordering corresponds to the helpfulness votes recorded.
- Change the sort option multiple times in sequence — confirm each change correctly re-renders the list in the expected order.
- Verify that the default sort order on page load matches the documented/designed default (e.g., "Most recent" or "Most helpful").
- Apply a sort while a star filter is also active — confirm both parameters are respected simultaneously and the URL reflects both states for shareability.

### Filter reviews (by stars)

- Click the "5 stars" filter — confirm only 5-star reviews are displayed and the count matches the number shown in the filter label.
- Click the "1 star" filter — confirm only 1-star reviews are displayed.
- Click each star filter (1–5) individually and verify the correct subset is shown each time.
- Click the same star filter a second time (to deactivate/toggle) or click "All" — confirm all reviews are restored.
- Apply a star filter on a product that has no reviews for that star level — confirm an appropriate empty state message is shown (e.g., "No 3-star reviews yet").
- Combine a star filter with a sort option — confirm both are applied together and the URL or state reflects both parameters.
- Verify that the review count displayed next to each star in the filter matches the actual count of visible reviews for that rating tier.
- Confirm that filtering does not affect the displayed average rating (the average always reflects all approved reviews, not just the filtered subset).

### Average rating correctly calculated

- Add two reviews (e.g., 4 stars and 2 stars) and verify the displayed average is 3.0.
- Add three reviews (5, 4, 3 stars) and verify the average is 4.0.
- Add a review with an odd-numbered sum that produces a non-integer average (e.g., 5 + 4 + 3 + 2 = 14 ÷ 4 = 3.5) — confirm the displayed value rounds or truncates according to the documented rule (e.g., one decimal place).
- After a review is deleted, confirm the average updates immediately (or after cache expiry) and is mathematically correct.
- After a review is edited and its star rating changes, confirm the average reflects the updated value.
- On a product with zero reviews, confirm the average rating is not displayed, or is shown as "No reviews yet" — not as "0 stars."
- Confirm the review count (e.g., "47 reviews") displayed next to the average star widget matches the actual number of visible, approved reviews.
- Verify the average rating shown on the product listing page (category page star snippet) matches the average shown on the product detail page.
- Confirm that pending/unapproved reviews are excluded from the average calculation.

### Reviews on product page

- Navigate to a product page — confirm the reviews section is present, displays approved reviews, and shows the average star rating and total count.
- Confirm each individual review shows: author name (or anonymized form), date, star rating, review text, and verified-buyer badge where applicable.
- Confirm reviews are paginated or "load more" functionality works when there are more reviews than the initial display limit.
- Verify that the reviews section is present in the page HTML (server-side rendered) and not loaded exclusively via JavaScript, to ensure crawler visibility.
- On a product with no reviews, confirm a call-to-action (e.g., "Be the first to review this product") is shown in place of the empty reviews list.
- Confirm the reviews section is accessible: headings are correct, star ratings are conveyed via ARIA labels (not just visual icons), and the form is keyboard-operable.
- Confirm review content is displayed with proper HTML encoding — angle brackets and ampersands in review text are escaped and not rendered as markup.

### Reviews on category page (stars)

- Navigate to a category page — confirm that products with at least one approved review display a star rating snippet (aggregate stars + count) in the product tile.
- Confirm products with no reviews display no star widget (not a "0 stars" display) on the category page.
- Confirm the aggregate star count shown in the product tile on the category page is identical to the average shown on the corresponding product detail page.
- After a new review is approved for a product, confirm the category page star snippet updates (accounting for cache) to reflect the new average.
- Verify that the star widget in product tiles is purely informational and does not trigger the review submission form on click; it should link to the reviews section of the product page.
- Confirm that star snippets are present in the HTML output on the category page for SEO crawlers to detect (not injected solely via deferred JavaScript).

### Review schema (rich snippets)

- Load the product page and inspect the page source or use Google's Rich Results Test — confirm a valid `Review` or `AggregateRating` Schema.org block is present.
- Verify the `aggregateRating` object contains: `ratingValue`, `reviewCount`, and `bestRating`/`worstRating` values that match the visible on-page data.
- Confirm that `ratingValue` in the schema matches the average displayed to the user (no discrepancy).
- Confirm that `reviewCount` in the schema matches the number of visible, approved reviews shown on the page.
- After a review is approved, confirm the structured data is updated accordingly (accounting for cache TTL).
- Confirm structured data is absent (or uses `reviewCount: 0` / is omitted entirely) on products with no approved reviews — Google may penalize schema that claims reviews that don't exist.
- Validate the schema against the Google Rich Results Test and confirm no errors or warnings are reported.
- Confirm individual `Review` items in the schema include `author`, `datePublished`, and `reviewRating` with a `ratingValue`.
- **[Security]** Confirm that review text injected into the schema block is properly JSON-escaped — a review containing `"` or `</script>` should not break the JSON-LD block or allow script injection.
- Verify the schema is present on the server-rendered HTML, not only after JavaScript execution, to ensure Googlebot can read it during crawl.
