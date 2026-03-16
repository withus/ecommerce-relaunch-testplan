# Product Page

### Product images (gallery, zoom, lightbox)

- Navigate to a product page that has multiple images and verify the main product image loads correctly without distortion or pixelation.
- Click the main product image and verify a lightbox/fullscreen overlay opens displaying the full-resolution image.
- In the lightbox, verify the image can be closed by clicking an "X" button, pressing Escape, or clicking outside the image area.
- Hover over the main product image on desktop and verify a zoom effect activates, showing a magnified region of the image.
- On a touch device, pinch-to-zoom on the main product image and verify the image zooms in smoothly.
- Verify that navigating between images in the lightbox (next/previous arrows or keyboard arrows) works correctly and displays the correct image in sequence.
- Verify that the image gallery does not block any critical UI elements (e.g., add-to-cart button) on mobile.
- Load a product page with a slow network throttle (e.g., 3G in DevTools) and verify a placeholder or skeleton is shown while images load, with no layout shift once loaded.
- Verify that the main gallery image is not lazy-loaded (it must appear immediately as above-the-fold content).

### Image switching (thumbnails, slider)

- On a product page with multiple images, verify all thumbnail images are displayed below or beside the main image.
- Click each thumbnail in sequence and verify the main image updates to display the corresponding full-size image.
- Verify the currently active thumbnail is visually highlighted (e.g., border, opacity change) to indicate selection.
- On mobile, swipe left and right on the main image and verify it slides to the next or previous image in the gallery.
- Verify that the thumbnail strip or slider is scrollable when there are more thumbnails than fit in the visible area.
- After switching images via thumbnail, open the lightbox and verify it opens on the currently selected image (not always the first).
- Verify that video thumbnails (if present) play an embedded video rather than opening a lightbox image.

### Product name correct

- Open a product page and verify the product name displayed in the H1 heading matches the product name in the page title tag.
- Verify the product name displayed in the breadcrumb matches the H1 heading.
- Verify there is exactly one H1 on the page and it contains the product name.
- Navigate to the product from a category page and verify the product name shown on the category listing matches the H1 on the product page.
- Test a product with a long name (60+ characters) and verify the name does not overflow its container or get truncated in a way that loses meaning.
- Verify the product name is not empty or replaced with a placeholder (e.g., "undefined", "null", "Product Name") for any product.

### Price display (unit price, tiered pricing, base price)

- Verify the product's selling price is prominently displayed on the page in the correct currency and format (e.g., "€ 29,99").
- If tiered/volume pricing is available, verify that a pricing table or breakdown is displayed (e.g., "1–9 units: €10.00, 10–49 units: €8.50").
- Verify the base price (price per unit of measure, e.g., "€ 59,80 / 100 ml") is displayed where legally required.
- If a sale price is active, verify the original/RRP is shown as a strikethrough alongside the reduced price.
- Verify the price displayed on the product page exactly matches the price shown when the item is added to the cart.
- For configurable products, verify the base price updates in real time when a variant with a different price is selected.
- Check that the price is not displayed as "0.00", "NaN", or blank for any in-stock product.

### VAT notice

- Verify a VAT notice (e.g., "incl. 19% VAT") is displayed near the price on every product page.
- In a B2B context (if applicable), verify the display switches to "excl. VAT" and shows the net price.
- Verify the VAT percentage shown in the notice matches the product's actual tax category (standard rate vs. reduced rate).
- Verify the VAT notice text is visible and not hidden behind other elements on mobile viewports.
- Inspect the displayed price mathematically: verify that the gross price equals the net price multiplied by the correct VAT factor.

### Shipping cost notice

- Verify a shipping cost notice is displayed near the price (e.g., "plus shipping costs" or "free shipping").
- Verify the shipping cost notice links to the shop's shipping information page and the link is working.
- For products that qualify for free shipping, verify the notice correctly reads "free shipping" rather than a generic link.
- On mobile, verify the shipping cost notice is visible and not obscured by overlapping elements.
- Verify the shipping cost notice updates correctly if a user selects a different delivery country (if a country selector exists on the page).

### Availability status (in stock, low stock, out of stock)

- Verify a product with sufficient stock displays a green "In Stock" (or equivalent) indicator.
- Verify a product with stock below the low-stock threshold displays a "Only X left in stock" warning.
- Verify a product with zero stock displays an "Out of Stock" (or equivalent) indicator and the add-to-cart button is disabled or replaced.
- Verify that the availability status shown on the product page is consistent with what was shown on the category page listing.
- Test a product set to "out of stock" in the backend and verify the product page reflects this status without a browser refresh (or upon next page load).
- Verify that an out-of-stock product still shows the product details and does not return a 404 error.
- If a "Notify me when available" feature exists, verify it is shown for out-of-stock products and that the form submits successfully.

### Delivery time display

- Verify an estimated delivery time is displayed on every in-stock product page (e.g., "Delivery in 3–5 business days").
- Verify the delivery time shown on the product page matches the delivery time displayed during checkout for the same product.
- For out-of-stock products, verify either no delivery time or an appropriate message (e.g., "Currently unavailable") is shown instead.
- Test selecting an express shipping option (if available on the product page) and verify the delivery time updates accordingly.
- Verify the delivery time is not shown as a placeholder value (e.g., "X days", "TBD") for standard products.

### Product description (short + long)

- Verify the short product description is displayed in the main product area, above the fold or near the add-to-cart button.
- Verify the long/full product description is present on the page (in a tab, accordion, or below the fold).
- Verify that HTML formatting within the description (bold, bullet lists, headings) renders correctly and is not shown as raw markup.
- Verify no description content is missing or truncated unexpectedly (compare with the backend entry).
- Test a product with a very long description (1000+ words) and verify it does not break the page layout.
- Verify the description text is indexable (not hidden inside a noscript-only container or dynamically loaded in a way that blocks crawlers).

### Technical specifications

- Verify a product with defined technical specifications displays them in a structured format (e.g., a table or definition list).
- Verify all specification labels and values are populated and none are blank or show fallback strings.
- Verify the specifications section is accessible by keyboard (e.g., within a tab panel that can be reached via Tab key).
- Test a product without specifications and verify the specifications section is hidden or not rendered (no empty table visible).
- Verify that units of measurement in specifications (e.g., "kg", "cm", "W") are displayed correctly and consistently.

### Product variants (size, color, material, etc.)

- Verify all available variants (sizes, colors, materials) are displayed as selectable options on the product page.
- Verify unavailable or out-of-stock variants are visually distinguished (e.g., grayed out, crossed out) and cannot be added to the cart.
- Select each variant in turn and verify the page updates correctly (price, image, availability, delivery time).
- Verify that the default state of the page (on first load) either shows no variant selected with a prompt to select one, or pre-selects a sensible default.
- Attempt to add a configurable product to the cart without selecting a required variant and verify an error or prompt is shown.
- Verify variant option labels are clearly readable at all viewport sizes and do not overflow their containers.
- Test a product with 20+ variants and verify all options are accessible (scrollable list, dropdown, or similar) without breaking the layout.

### Variant change updates price

- Select a variant known to have a higher price surcharge and verify the displayed price increases immediately.
- Select a variant with a lower price and verify the price decreases immediately.
- Switch between multiple variants rapidly and verify the price always reflects the currently selected variant (no race condition showing a stale price).
- Verify the updated price in the product page matches the price in the cart after adding with that variant selected.
- Verify the VAT notice and base price also update when the variant price changes.

### Variant change updates image

- Select a color or style variant and verify the main product image changes to show the correct image for that variant.
- Verify the thumbnail strip also updates to show the images associated with the selected variant.
- Switch back to the original variant and verify the original images are restored.
- Test a variant that has no dedicated image and verify the main product image either shows a sensible fallback or does not break.
- Select a variant, open the lightbox, and verify the lightbox images correspond to the selected variant's images.

### Variant change updates availability

- Select a variant that is in stock and verify the availability indicator shows "In Stock" and the add-to-cart button is enabled.
- Select a variant that is out of stock and verify the availability indicator changes to "Out of Stock" and the add-to-cart button is disabled.
- Select a variant with low stock and verify the "Only X left" warning appears with the correct quantity.
- Rapidly switch between in-stock and out-of-stock variants and verify the availability status always reflects the currently selected variant.
- Verify the delivery time display also updates when switching between variants with different stock situations.

### Quantity selector

- Verify the quantity selector is present on the product page and defaults to a value of 1.
- Type a valid quantity (e.g., 3) directly into the quantity input field and verify the field accepts the value.
- Use the increment (+) and decrement (−) buttons on the quantity selector and verify the quantity changes by 1 each click.
- Verify the quantity field accepts only numeric input and rejects letters, symbols, or negative numbers.
- Set the quantity to 0 and attempt to add to the cart; verify this is prevented with an appropriate error or the field resets to 1.
- Set a very large quantity (e.g., 9999) and verify the field either caps at the maximum allowed or allows it to be corrected before adding to the cart.

### Quantity limits (min/max)

- Verify that if a minimum order quantity (e.g., min 2) is configured, the quantity selector starts at that minimum and cannot be set below it.
- Verify that if a maximum order quantity is configured, the quantity selector cannot be increased beyond that maximum (via button or direct input).
- Attempt to manually type a quantity below the minimum and verify the value is corrected or an error is shown on add-to-cart.
- Attempt to manually type a quantity above the maximum and verify the value is corrected or an error is shown.
- Verify that quantity limit messages are clearly communicated to the user (e.g., "Maximum 10 per order").
- Add a product multiple times to the cart and verify the combined quantity in the cart does not silently exceed the maximum.

### Add to cart button

- Click the "Add to Cart" button on an in-stock product page and verify the product is added to the cart.
- Verify a visual confirmation is shown after adding (e.g., cart counter increments, mini-cart slides in, success toast/notification).
- Verify the cart item count in the header updates immediately after adding.
- Verify the added product, variant, and quantity in the cart exactly match what was selected on the product page.
- Click the "Add to Cart" button multiple times in quick succession and verify the product is not duplicated beyond the intended quantity.
- Test adding to cart while unauthenticated and verify the product is added to the guest cart without requiring login.
- Test adding a product when the session has expired and verify an appropriate error or redirect is shown.

### Buy now button (if applicable)

- If a "Buy Now" button is present, click it and verify the user is taken directly to checkout with the product pre-loaded.
- Verify the cart contents are preserved when using "Buy Now" (the existing cart is not silently cleared, or if it is, a warning is shown).
- Test "Buy Now" without selecting a required variant and verify an error is shown rather than proceeding to checkout.
- Verify "Buy Now" respects the quantity selector value (e.g., buying 3 units goes to checkout with quantity 3).
- Test "Buy Now" as a guest user and verify the checkout flow starts correctly without requiring prior login.

### Wishlist button

- Click the "Add to Wishlist" button and verify the product is saved to the user's wishlist.
- Verify the wishlist button changes state (e.g., icon fills in, label changes to "Saved") after adding.
- Click the button again and verify the product is removed from the wishlist (toggle behavior).
- Test the wishlist button as a guest user and verify the system either saves to a cookie-based wishlist or prompts the user to log in.
- Navigate to the wishlist page and verify the product just added appears there with the correct name, image, and price.
- Add a product with a specific variant (e.g., size L, red color) to the wishlist and verify the variant is saved alongside the product.

### Product comparison

- If a comparison feature is available, add two products to the comparison and verify the comparison page opens showing both products side by side.
- Verify the comparison table displays the same attributes for all compared products in aligned rows.
- Add the maximum allowed number of products to comparison and verify adding one more shows an appropriate error or replaces the oldest entry.
- Remove a product from the comparison and verify the comparison updates correctly.
- Verify the comparison feature is accessible and functional on mobile viewports.
- Navigate away from the page and return; verify the comparison selection is persisted (e.g., via cookie or session).

### Reviews display

- Verify the reviews section is displayed on the product page (in a tab, accordion, or inline section).
- Verify each review shows: star rating, review title (if applicable), review text, reviewer name (or anonymized), and submission date.
- Verify reviews are displayed in a readable, correctly formatted layout with no raw HTML or broken characters.
- Test a product with no reviews and verify a message such as "No reviews yet. Be the first to review!" is shown rather than an empty or broken section.
- Test a product with 50+ reviews and verify pagination or a "Load more" mechanism is present and functional.
- Verify that the reviews displayed on the page are sorted by the default sort order (e.g., newest first) and that sorting controls are present.

### Submit a review

- Navigate to a product page and find the "Write a Review" form or button; verify it is accessible.
- Submit a review with all required fields filled (rating, review text) and verify a success message is shown.
- Attempt to submit a review without selecting a star rating and verify an error prevents submission.
- Attempt to submit a review with only whitespace in the text field and verify this is rejected.
- Submit a review with a very long text (1000+ characters) and verify the form handles it without error.
- Submit a review with special characters (umlauts, emoji, HTML tags) and verify the content is stored and displayed safely (no XSS).
- Verify that an unauthenticated user is either prompted to log in before reviewing or can submit a guest review.
- If reviews require moderation, verify the reviewer sees a "Your review is awaiting approval" message rather than immediate publication.

### Review with images

- If image uploads are supported in reviews, attach a valid image (JPG, PNG) and submit the review; verify the image is displayed with the review.
- Attempt to attach a file that is not an image (e.g., a PDF or executable) and verify the upload is rejected.
- Attempt to attach an oversized image (e.g., 20 MB) and verify an appropriate file size error is shown.
- Verify uploaded review images are optimized and do not significantly slow down the product page load.
- Verify review images open in a lightbox or larger view when clicked.

### Average rating / stars

- Verify the average star rating displayed on the product page matches the arithmetic mean of all individual review ratings.
- Verify the average rating is displayed as both a numeric value (e.g., "4.3") and a visual star representation.
- Verify the total number of reviews contributing to the average is displayed (e.g., "4.3 (87 reviews)").
- Submit a new review and verify the average rating and review count update accordingly.
- Test a product with a single review and verify the average equals that single review's rating.
- Test a product with no reviews and verify the average rating section shows a neutral/empty state rather than "0 stars".

### Ask a question about the product

- If a "Ask a Question" feature is present, click it and verify a form or modal opens.
- Submit a question with valid input and verify a success message or confirmation is shown.
- Attempt to submit an empty question and verify the form is not submitted and an error is displayed.
- Attempt to submit a question with only whitespace and verify it is rejected.
- Verify submitted questions (if publicly displayed with answers) appear in the Q&A section of the product page.
- Test submitting a question as a guest vs. a logged-in user and verify both flows work correctly.

### Social sharing buttons

- Verify social sharing buttons are present on the product page (e.g., Facebook, Pinterest, WhatsApp, copy-link).
- Click each sharing button and verify it opens the correct sharing dialog or URL with the product page URL pre-populated.
- Verify the shared URL resolves to the correct product page (not a homepage or 404).
- Verify the page's Open Graph meta tags (og:title, og:image, og:description) are correctly populated for social sharing previews.
- On mobile, verify the WhatsApp or native share button opens the device's share sheet correctly.
- Verify that sharing buttons do not load third-party scripts until the user has given cookie consent.

### Tabs (description, reviews, FAQ, shipping)

- Verify all tabs (Description, Reviews, FAQ, Shipping, etc.) are displayed and clickable.
- Click each tab in turn and verify the corresponding content panel becomes visible and the previously active panel is hidden.
- Verify the active tab is visually distinguished (e.g., underline, different background).
- Test tab navigation via keyboard (Tab to focus, Enter/Space to activate) and verify it works correctly.
- Verify that anchor links to specific tabs (e.g., `#reviews`) in the URL correctly open and scroll to the relevant tab.
- Test tabs on mobile and verify they are accessible without horizontal scrolling if they are in a row, or presented as a dropdown/accordion.
- Verify tab content does not collapse or overlap with other page elements when the viewport is resized.

### Similar products / cross-selling

- Verify a "Similar products" or "You may also like" section is displayed on the product page.
- Verify each product in the cross-sell section shows at minimum: product image, product name, and price.
- Click on a recommended product and verify navigating to that product's page works correctly.
- Verify the cross-sell section does not display the currently viewed product as a recommendation.
- On mobile, verify the cross-sell section is scrollable or displayed in a readable layout without items being cut off.
- Verify that cross-sell items link to products that are published and not out of stock (or clearly marked if unavailable).

### Accessories / upselling

- Verify an "Accessories" or "Complete the look" / upsell section is displayed where applicable.
- Verify each upsell product shows an image, name, and price.
- Test adding an accessory to the cart directly from the upsell section and verify the product is added correctly.
- Verify the upsell section is not shown for products that have no configured upsells.
- Verify upsell items shown on a product page are genuinely related to the current product (no clearly irrelevant suggestions).

### Recently viewed products

- View three different product pages in sequence, then return to any product page and verify the "Recently Viewed" section shows the previously visited products.
- Verify the current product is not shown in its own "Recently Viewed" section.
- Verify recently viewed products persist after closing and reopening the browser (for logged-in users or via cookie).
- Clear browser cookies and verify the recently viewed section is empty on next visit.
- Verify recently viewed products display the correct image, name, and price for each item.

### Breadcrumbs

- Verify breadcrumbs are present on the product page in the format: Home > Category > (Subcategory >) Product Name.
- Click each breadcrumb link and verify it navigates to the correct page (Home, Category, Subcategory).
- Verify the last breadcrumb item (the current product name) is not a hyperlink.
- Verify breadcrumbs are present and correct when a product is accessed directly via URL rather than through category navigation.
- Test a product in multiple categories and verify the breadcrumb reflects the navigation path used to arrive at the page (or a defined primary category).
- On mobile, verify the breadcrumb does not overflow its container and is either truncated gracefully or wraps to a second line.

### Structured data (Product Schema, reviews)

- Use Google's Rich Results Test (or validate the page source) and verify `Product` schema markup is present on the product page.
- Verify the Product schema includes at minimum: `name`, `image`, `description`, `sku` or `mpn`, `offers` (with `price`, `priceCurrency`, `availability`).
- Verify the `offers.availability` property in the schema matches the actual displayed availability status.
- Verify that if reviews are present, `AggregateRating` schema is included with correct `ratingValue` and `reviewCount`.
- Verify individual `Review` schema entries are present if individual reviews are shown on the page.
- Verify there are no schema validation errors or warnings in Google's Rich Results Test.
- Verify the structured data is not duplicated on the page (e.g., two separate `Product` schema blocks).

### Canonical URL

- View the product page source and verify a `<link rel="canonical">` tag is present in the `<head>`.
- Verify the canonical URL matches the primary/preferred URL for that product (no trailing query parameters like `?ref=`, `?sort=`, or session IDs).
- Access the product page via a URL with a UTM parameter (e.g., `?utm_source=email`) and verify the canonical URL still points to the clean product URL.
- Verify no product page has a canonical URL pointing to a different product or to the homepage.
- Verify the canonical URL is absolute (starts with `https://`) and not relative.

### SEO meta tags

- Verify the product page has a unique, descriptive `<title>` tag containing the product name and optionally the brand or shop name.
- Verify the `<meta name="description">` tag is present, unique for this product, and between 120–160 characters.
- Verify the `og:title`, `og:description`, and `og:image` Open Graph tags are populated with product-specific content.
- Verify `og:image` points to a valid, accessible product image URL.
- Verify the `<meta name="robots">` tag does not contain `noindex` for published, in-stock products.
- Test a product marked as a draft or hidden in the backend and verify its page either returns 404 or contains `noindex`.
- Verify no two product pages share identical title tags or meta descriptions.
