# 1. Homepage

### Hero banner / slider display

- Verify the hero banner is visible immediately on page load without a noticeable delay or flash of unstyled content
- Verify all slider images render correctly (no broken image icons, no placeholder boxes)
- Verify image aspect ratios are consistent across all slides (no distortion or cropping artifacts)
- Verify the slider auto-advances to the next slide after the configured interval (e.g., 5 seconds)
- Verify the slider pauses auto-advance when the user hovers over it
- Verify the slider resumes auto-advance when the user moves the cursor away
- Verify clicking the next arrow advances to the next slide
- Verify clicking the previous arrow returns to the previous slide
- Verify clicking dot indicators navigates directly to the corresponding slide
- Verify the active dot indicator is highlighted to reflect the current slide
- Verify the slider loops back to the first slide after the last slide
- Verify each banner image links to the correct destination URL when clicked
- Verify CTA buttons on banners are visible, legible, and respond to clicks
- Verify CTA button links navigate to the correct page (e.g., sale category, product page)
- Verify the slider adapts correctly at mobile breakpoints (full width, no overflow)
- Verify the slider adapts correctly at tablet breakpoints
- Verify swipe-left and swipe-right touch gestures advance/retreat slides on mobile devices
- Verify keyboard navigation (arrow keys) works when the slider is focused
- Verify slide transitions are smooth and free of visual glitches

### Category overview / navigation

- Verify all category tiles/cards are displayed on the homepage
- Verify each category tile shows the correct category image
- Verify each category tile displays the correct category name/label
- Verify clicking a category tile navigates to the correct category page
- Verify category images are not broken or missing
- Verify the category overview section is fully visible without horizontal scrolling on desktop
- Verify the layout adjusts correctly to a grid or carousel on mobile
- Verify hover states on category tiles are visually consistent (e.g., overlay, border)
- Verify the number of categories shown matches the expected/configured count
- Verify no duplicate categories are displayed

### Product highlights / bestsellers

- Verify the product highlights section is present and renders on page load
- Verify the correct products are displayed (matches the configured bestseller/featured list)
- Verify each product card shows the product image
- Verify each product card shows the correct product name
- Verify each product card shows the correct price
- Verify sale prices and strikethrough original prices display correctly where applicable
- Verify clicking a product card navigates to the correct product detail page
- Verify the "Add to cart" button (if present on the card) adds the product to the cart
- Verify the wishlist button (if present on the card) adds the product to the wishlist
- Verify product images are not broken or missing
- Verify the section title (e.g., "Bestsellers") is displayed
- Verify the layout is responsive and adapts correctly on mobile and tablet
- Verify the number of displayed products matches the configured limit

### Current offers / sale banners

- Verify sale banners are displayed when active promotions exist
- Verify the banner text accurately reflects the current offer (e.g., "20% off all orders")
- Verify sale banner images are not broken
- Verify clicking the sale banner navigates to the correct sale/promotion page or category
- Verify countdown timers (if present) display the correct remaining time
- Verify countdown timers count down in real time
- Verify expired offers are no longer shown
- Verify the sale banner is visually prominent and not hidden below the fold on desktop
- Verify the banner is legible and fully visible on mobile
- Verify CTA buttons within sale banners are clickable and link correctly

### Search field display + functionality

- Verify the search field is visible on the homepage
- Verify the search field displays placeholder text (e.g., "Search for products…")
- Verify clicking/tapping the search field focuses it and shows a cursor
- Verify typing into the search field inputs text correctly
- Verify pressing Enter submits the search and navigates to the results page
- Verify clicking the search icon/button submits the search
- Verify the search field shows autocomplete suggestions while typing
- Verify autocomplete suggestions are relevant to the typed query
- Verify clicking an autocomplete suggestion navigates to the correct result or product
- Verify submitting an empty search query shows an appropriate message or does not crash
- Verify the search field is accessible via keyboard (Tab to focus, Enter to submit)
- Verify the search field is usable on mobile (correct keyboard type, no overlapping elements)

### Newsletter signup

- Verify the newsletter signup section/form is displayed on the homepage
- Verify the email input field is present and accepts text input
- Verify submitting a valid email address shows a success confirmation message
- Verify submitting an invalid email format (e.g., "user@") shows a validation error
- Verify submitting an empty email field shows a required-field validation error
- Verify a double opt-in confirmation email is sent after successful signup
- Verify submitting a previously registered email shows an appropriate message (e.g., "already subscribed")
- Verify the signup form includes a privacy policy checkbox or notice (if required)
- Verify submitting without checking the required consent checkbox shows a validation error
- Verify the submit button is disabled or shows a loading state while the request is processing
- Verify the newsletter form is usable on mobile (fields and button fully accessible)

### Trust elements (badges, reviews, USPs)

- Verify trust badges (e.g., SSL seal, payment provider logos, Trusted Shops) are displayed
- Verify all trust badge images load correctly (no broken images)
- Verify trust badges link to their respective verification/certificate pages where applicable
- Verify USP elements (e.g., "Free shipping", "30-day returns") are visible
- Verify USP icons/images render correctly
- Verify USP text is accurate and up to date
- Verify aggregate customer review score and count are displayed correctly
- Verify clicking a review score element navigates to the reviews section or page
- Verify trust elements are visible above or near the fold without excessive scrolling
- Verify trust elements display correctly on mobile (no text overflow, no overlapping icons)

### Footer (links, contact, social media, payment method icons)

- Verify the footer is present and fully rendered at the bottom of the homepage
- Verify all footer navigation links are present (e.g., Imprint, Privacy Policy, Terms, Contact)
- Verify each footer link navigates to the correct page
- Verify no footer links return a 404 or redirect error
- Verify the contact information in the footer is correct (phone number, email address, address)
- Verify the phone number is a clickable `tel:` link on mobile
- Verify the email address is a clickable `mailto:` link
- Verify social media icons (Facebook, Instagram, etc.) are displayed
- Verify each social media icon links to the correct social media profile and opens in a new tab
- Verify payment method icons (Visa, Mastercard, PayPal, etc.) are displayed
- Verify payment method icons load without broken images
- Verify the footer copyright notice shows the correct year and company name
- Verify the footer layout is responsive and readable on mobile (no horizontal overflow)
- Verify footer columns stack correctly on small screens

### Cookie banner / consent management

- Verify the cookie consent banner appears on the first visit to the homepage
- Verify the banner is visible and not hidden behind other elements
- Verify the banner displays a clear description of cookie usage
- Verify an "Accept all" button is present and functional
- Verify clicking "Accept all" dismisses the banner and sets consent cookies
- Verify a "Reject all" or "Necessary only" button is present and functional
- Verify clicking "Reject" dismisses the banner and sets only necessary cookies
- Verify a "Manage settings" or "Customize" option is available to configure cookie categories individually
- Verify marketing/analytics cookies are NOT set before the user accepts them
- Verify the consent choice is remembered on subsequent visits (banner does not reappear)
- Verify the user can change their consent settings via a link in the footer
- Verify the banner is accessible via keyboard and screen reader
- Verify the banner is fully usable on mobile (buttons not cut off, text readable)

### Load time / performance

- Verify the homepage fully loads within an acceptable time (target: under 3 seconds on a standard connection)
- Verify the Time to First Byte (TTFB) is under 600ms
- Verify the Largest Contentful Paint (LCP) occurs within 2.5 seconds
- Verify Cumulative Layout Shift (CLS) score is below 0.1 (no elements jumping during load)
- Verify First Input Delay (FID) / Interaction to Next Paint (INP) is below 200ms
- Verify images below the fold use lazy loading (not loaded until scrolled into view)
- Verify images above the fold (hero banner) are NOT lazy loaded and appear immediately
- Verify CSS and JavaScript files are minified and compressed
- Verify Gzip or Brotli compression is active (check response headers)
- Verify browser caching headers are set correctly for static assets
- Verify no unnecessary blocking render resources delay page display
- Measure and document the homepage load time using Google PageSpeed Insights (target: score 75+ on mobile, 90+ on desktop)

### Above-the-fold content correct

- Verify the hero banner/primary image is fully visible without scrolling on a 1920×1080 desktop display
- Verify the main navigation header is visible above the fold
- Verify the primary CTA button is visible above the fold
- Verify the page headline/value proposition is visible above the fold
- Verify no critical content is hidden behind the cookie banner or other overlays
- Verify above-the-fold content is correct and complete on a 768px tablet viewport
- Verify above-the-fold content is correct and complete on a 375px mobile viewport
- Verify no layout shift pushes key content below the fold during or after page load
- Verify the logo is visible in the header above the fold

### Responsive layout (mobile, tablet, desktop)

- Verify the homepage renders without horizontal scrollbar at 1920px desktop width
- Verify the homepage renders without horizontal scrollbar at 1280px laptop width
- Verify the homepage renders without horizontal scrollbar at 768px tablet width
- Verify the homepage renders without horizontal scrollbar at 375px mobile width (iPhone SE)
- Verify the homepage renders correctly at 390px mobile width (iPhone 14)
- Verify font sizes remain legible at all breakpoints (no text smaller than 14px)
- Verify all images scale proportionally and do not overflow their containers
- Verify grid layouts (category tiles, product cards) reflow to fewer columns on smaller screens
- Verify the navigation switches to a hamburger menu on mobile
- Verify no UI elements overlap each other on any tested viewport
- Verify buttons and links have sufficient tap target size on mobile (minimum 44×44px)
- Verify the layout in landscape orientation on mobile is correct and usable
- Verify the layout on tablet in both portrait and landscape orientations

### SEO meta tags (title, description, OG tags)

- Verify the page `<title>` tag is present and contains the expected homepage title text
- Verify the title tag length is within recommended limits (50–60 characters)
- Verify the meta description tag is present and contains meaningful content
- Verify the meta description length is within recommended limits (120–160 characters)
- Verify the `og:title` Open Graph tag is present and contains the correct title
- Verify the `og:description` Open Graph tag is present and contains the correct description
- Verify the `og:image` Open Graph tag is present and points to a valid, accessible image URL
- Verify the `og:url` Open Graph tag matches the canonical URL of the homepage
- Verify the `og:type` tag is set to `website` for the homepage
- Verify Twitter Card tags (`twitter:card`, `twitter:title`, `twitter:description`, `twitter:image`) are present
- Verify no duplicate title or meta description tags exist in the HTML
- Verify the meta viewport tag is present: `<meta name="viewport" content="width=device-width, initial-scale=1">`

### Structured data (Schema.org)

- Verify the homepage contains a `WebSite` Schema.org structured data block
- Verify the `WebSite` schema includes a `SearchAction` for sitelinks searchbox (if applicable)
- Verify an `Organization` schema block is present with correct name, URL, logo, and contact info
- Verify structured data is valid JSON-LD (no syntax errors)
- Validate the structured data using Google's Rich Results Test tool — zero errors expected
- Verify no deprecated or unsupported schema properties are used
- Verify the logo URL in the `Organization` schema points to an accessible image
- Verify the `@context` is set to `https://schema.org`

### Breadcrumbs

- Verify that breadcrumbs are NOT displayed on the homepage (homepage is the root — no breadcrumb needed)
- If breadcrumbs are shown on the homepage, verify that only "Home" is displayed with no additional levels
- Verify breadcrumb structured data (BreadcrumbList schema) is present on sub-pages linked from the homepage
- Verify no breadcrumb navigation causes a broken link when clicked on other pages reached from the homepage

### Language switcher (if multilingual)

- Verify the language switcher is visible in the header on the homepage
- Verify the current active language is clearly indicated (highlighted, flag, label)
- Verify clicking an alternative language option reloads the page in the selected language
- Verify all homepage content (header, body, footer) is translated after switching language
- Verify the URL structure changes correctly on language switch (e.g., `/en/`, `/de/`)
- Verify the `hreflang` tags in the HTML head reference all available language variants
- Verify switching language does not log out the user or clear the cart
- Verify the selected language preference is persisted on subsequent visits (cookie or session)
- Verify the language switcher is accessible on mobile and does not overflow the header
- Verify currency and date formats adapt to the selected language/locale where applicable
