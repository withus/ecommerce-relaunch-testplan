# 42. Social Commerce & Sharing

### Social media share buttons on product pages

- Verify that share buttons are visible on all product detail pages (desktop and mobile).
- Confirm that the set of share buttons matches the platforms defined in requirements (e.g., Facebook, Twitter/X, Pinterest, WhatsApp, Email).
- Click each share button and verify it opens the correct platform's share dialog or native share sheet (mobile).
- Verify that share buttons on mobile trigger the OS native share sheet (Web Share API) where supported.
- Verify that share buttons do not load third-party social tracking scripts until the user has given consent for marketing/social cookies in the consent management platform.
- Confirm that share buttons are accessible via keyboard (Tab to focus, Enter to activate) and have descriptive ARIA labels (e.g., `aria-label="Share on Facebook"`).
- Verify that share buttons do not cover other critical UI elements (e.g., Add to Cart button) on any viewport size.
- Confirm that the URL passed to each share button is the canonical product URL, not a session-specific or tracking-polluted URL.
- Verify that share button click events are captured correctly in analytics (GA4 custom event `share` with `method` and `content_type` parameters).
- Verify that UTM parameters appended to shared URLs are correct and consistent for each platform (e.g., `utm_source=facebook&utm_medium=social`).

### Open Graph tags (og:title, og:image, og:description) correctly populated

- Open the page source (or use a meta tag inspector tool) on a product page and confirm that `og:title` matches the product name exactly.
- Confirm that `og:description` is populated with a meaningful product description (not empty, not truncated mid-word, not longer than ~200 characters).
- Confirm that `og:image` points to a valid, publicly accessible image URL (not a relative path, not a localhost/staging URL in production).
- Verify that the `og:image` meets Facebook's minimum dimension requirements (at least 1200×630 px recommended; minimum 600×315 px).
- Confirm that `og:url` matches the canonical URL of the product page (no query strings, no trailing variation parameters).
- Verify that `og:type` is set to `product` on product pages and `website` on other page types (homepage, category).
- Confirm that `og:site_name` is consistently set to the shop name across all pages.
- Confirm that `og:locale` is correctly set to the page's language (e.g., `de_DE`, `en_US`) and that `og:locale:alternate` is present for each additional language if the shop is multilingual.
- Verify that product pages include `product:price:amount` and `product:price:currency` OG tags so Facebook Shops can read price information.
- Verify that OG tags on category pages and CMS pages (about, blog posts) are also populated correctly, not defaulting to blank or homepage values.
- Confirm that OG tags are rendered server-side (present in the initial HTML response), not injected only by JavaScript after page load, so crawlers and share scrapers can read them.
- Verify that when a product variant is selected (e.g., a different color), the OG image does not change in the HTML head unless the variant has its own dedicated URL.

### Twitter Card tags correct

- Confirm that `twitter:card` is set to `summary_large_image` on product pages so the preview shows the full-width image.
- Verify that `twitter:title` is present and matches (or is a trimmed version of) `og:title`, within Twitter's 70-character display limit.
- Confirm that `twitter:description` is present, meaningful, and within Twitter's 200-character display limit.
- Verify that `twitter:image` points to a publicly accessible image URL and meets Twitter's dimension requirements (minimum 300×157 px, recommended 1200×628 px, max file size 5 MB).
- Confirm that `twitter:site` is set to the shop's official Twitter/X handle (e.g., `@yourshop`).
- Validate all Twitter Card tags using the Twitter Card Validator tool and confirm the preview renders correctly without errors.
- Confirm there are no duplicate `twitter:*` meta tags in the page source.
- Confirm Twitter Card tags are present on blog/content pages as well, not only product pages.

### Social login (Facebook, Google, Apple)

- Navigate to the registration page and verify that all configured social login buttons (Facebook, Google, Apple) are visible and correctly labeled.
- Navigate to the login page and verify the same social login buttons appear.
- Click "Continue with Google" and confirm the OAuth consent screen loads correctly with the correct app name and requested scopes (email, profile — no excessive permissions).
- Complete the Google OAuth flow with a new account and verify that a customer account is created in the shop, the email address is stored, and the user is redirected to the correct post-registration page.
- Complete the Google OAuth flow with an email address that already exists as a password-based account and verify the shop handles this gracefully (either links accounts with user confirmation or shows a clear error message — no silent duplicate account creation).
- Click "Continue with Facebook" and complete the OAuth flow; verify account creation and that the Facebook profile picture is optionally imported or ignored based on shop configuration.
- Click "Continue with Apple" and complete the Sign in with Apple flow, including the "Hide My Email" relay option; verify the shop accepts the anonymized email and creates the account correctly.
- Verify that on first social login, the user is presented with the privacy policy and terms acceptance step (consent must be active, not pre-checked).
- After social login, verify the user can navigate to My Account and that no password is required to be set unless they choose to add one.
- Verify that a user who registered via social login cannot log in with email + password until they explicitly set a password (no blank-password bypass).
- Simulate an OAuth state mismatch (tampered `state` parameter) and confirm the shop returns an error and does not create or log in any account.
- Simulate a social login provider outage (disable network to the OAuth endpoint) and verify the shop shows a user-friendly error message and does not crash.
- After social login, verify that the session cookie is set with Secure, HttpOnly, and SameSite=Lax flags, identical to a standard password login session.
- Verify that revoking the app permission on the provider side (Google account settings → remove app) and then attempting to log in shows a clear error rather than broken behavior.

### Share preview in social networks (image + text correct?)

- Use the Facebook Sharing Debugger (developers.facebook.com/tools/debug) on the product URL and confirm the preview shows the correct title, description, and image with no scraping errors.
- Use the Facebook Sharing Debugger's "Scrape Again" function after a product update and confirm the cached OG data is refreshed within a reasonable time (or cache-busting is in place).
- Use the LinkedIn Post Inspector (linkedin.com/post-inspector) on the product URL and confirm the preview renders correctly.
- Use the Twitter Card Validator on the product URL and confirm the card preview shows the large image, title, description, and site handle.
- Use WhatsApp on a mobile device to share a product URL and confirm the link preview (thumbnail, title, description) displays correctly in the chat.
- Use Telegram to share a product URL and confirm the preview renders correctly (Telegram uses its own OG scraper).
- Share a product URL on Pinterest and confirm the pin image corresponds to `og:image` and the description is populated from `og:description`.
- Verify that the shared image is not the shop logo or a placeholder when OG image tags are missing — test with a product that intentionally has no image to confirm a fallback OG image is served.
- Verify that the preview URL shown in social shares uses HTTPS and the production domain, not a staging or development URL.
- Confirm that product pages with very long titles display truncated titles gracefully in previews (no broken layout in the preview card).

### Social proof: display "X people are viewing this product right now"

- Navigate to a product page and verify the "X people are viewing this product" message appears (if the feature is active).
- Confirm the number displayed is within a plausible range and is not hardcoded (e.g., always "17") — verify that the value changes over time or between page loads.
- Verify that the feature uses a real-time or near-real-time data source (WebSocket, SSE, or polling) and does not display stale data from a day-old cache.
- Confirm the message disappears or shows "0" when no other users are viewing (or that the component is hidden below a minimum threshold to avoid showing "1 person viewing" to the only user).
- Verify that the social proof counter does not count bots or automated health-check requests (e.g., Googlebot, uptime monitors).
- Verify that the feature degrades gracefully when the real-time service is unavailable — the component should either hide itself or show nothing, not show an error or a broken UI element.
- Confirm that the social proof component does not violate GDPR by tracking individual user sessions without consent — verify that the underlying mechanism (e.g., anonymous session counter) does not store personally identifiable data.
- Verify that the counter is not inflatable via simple browser tab manipulation (e.g., opening 50 tabs should not show "50 people viewing") — confirm session deduplication is in place.
- Verify that the counter display is accessible (sufficient color contrast, not conveyed by color alone) and does not shift layout as the number updates (CLS impact check).

### Instagram Shopping / Facebook Shop integration

- Verify that the product catalog feed (XML or API) is correctly configured and submitted to Meta Commerce Manager.
- Confirm that the product feed is updated regularly (at minimum daily) and reflects current prices, stock levels, and product names.
- Verify that each product in the feed has a valid `id`, `title`, `description`, `availability`, `condition`, `price`, `link`, and `image_link` field — validate using the Meta Product Feed Debug tool.
- Confirm that the `link` field in the feed points to the correct canonical product URL on the shop, and that the URL is accessible without login.
- Confirm that the `image_link` field points to a high-resolution image (minimum 500×500 px, recommended 1024×1024 px) that is publicly accessible.
- Verify that out-of-stock products are marked with `availability: out of stock` in the feed and are not shown as purchasable in Instagram Shopping.
- Confirm that product variants (size, color) are submitted as separate items with `item_group_id` linking them, so Instagram Shopping displays the variant selector correctly.
- Verify that the Facebook Pixel (or Meta Conversions API) fires `ViewContent`, `AddToCart`, and `Purchase` events correctly when a user navigates from an Instagram Shopping tag to the shop — confirm event parameters (content_id, content_type, value, currency) are correct.
- Simulate a purchase that originates from an Instagram Shopping tag and confirm the purchase is attributed correctly in Meta Ads Manager (not assigned to "direct" traffic).
- Verify that the Facebook Shop product catalog is reviewed and approved by Meta without policy violations — confirm no products in the feed violate Meta's Commerce Policies (e.g., regulated goods, misleading claims).
- Confirm that the shop's Facebook Page is correctly linked to the Instagram Business account and that the catalog is connected to both.
- Verify that price changes in the shop are reflected in the Facebook/Instagram catalog within the feed update interval and that users clicking through from Instagram see the same price as in the feed.
