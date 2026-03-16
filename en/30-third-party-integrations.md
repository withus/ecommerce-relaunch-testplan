# 30. Third-Party Integrations

### Google Maps (store locator, contact page)

- Verify the Google Maps widget renders correctly on the contact page and store locator page across all supported browsers (Chrome, Firefox, Safari, Edge).
- Confirm the map loads at the correct zoom level and centers on the correct location(s) without manual interaction.
- Verify that map markers, info windows, and address details are accurate and match the business data.
- Click each map marker and confirm the info window displays the correct store name, address, and opening hours.
- Test "Get directions" link opens the correct destination in Google Maps (new tab or native maps app on mobile).
- Verify the map is interactive: panning, zooming, and switching between map/satellite view all work.
- Simulate Google Maps API outage (block the API endpoint via browser DevTools / network throttling): confirm a graceful fallback is shown (e.g., static image, text address, or error notice) instead of a broken widget or blank space.
- Verify the Google Maps embed only loads after the user has given consent for "external media" or equivalent consent category — no map tiles or tracking requests before consent.
- After consent is rejected, confirm no requests to `maps.googleapis.com` appear in the network tab.
- After consent is granted and then revoked, confirm the map widget disappears or is replaced by the consent prompt on next page load.
- Test the map on mobile devices: confirm touch interactions (pinch-to-zoom, drag) work correctly.
- Verify the Maps API key is not exposed in plain text in the page source beyond what is necessary (no server-side keys in frontend).
- Confirm the map widget does not cause layout shift (CLS) during page load.

---

### reCAPTCHA / spam protection

- Verify reCAPTCHA (v2 checkbox, v2 invisible, or v3 score-based) appears or is active on all intended forms: contact form, registration, login, checkout, review submission.
- Submit each protected form without completing the CAPTCHA challenge (v2): confirm submission is blocked and an appropriate error message is shown.
- Complete a valid reCAPTCHA challenge and confirm the form submits successfully.
- Test reCAPTCHA v3 (if used): verify that a score is evaluated server-side and low-score submissions are blocked or flagged without disrupting real users.
- Simulate Google reCAPTCHA API unavailability (block `www.google.com/recaptcha`): verify the fallback behavior — either the form uses a server-side honeypot fallback, or it displays an explicit error message; the form must not silently skip protection.
- Confirm reCAPTCHA is only loaded after consent for "functional" or "necessary" cookies is given, or that it falls under strictly necessary and is documented accordingly in the privacy policy.
- Verify that reCAPTCHA does not load (no requests to `google.com/recaptcha`) before the appropriate consent category is accepted.
- Check that the reCAPTCHA widget does not obscure critical UI elements (especially the checkout button or form submit) on mobile viewports.
- Validate that the reCAPTCHA badge or checkbox is visible and accessible via keyboard on all targeted forms.
- Test with an ad blocker active: confirm the form either degrades gracefully (fallback spam protection) or informs the user that the CAPTCHA cannot load.
- Confirm CAPTCHA errors are conveyed to screen readers via ARIA live regions or meaningful error text.

---

### Trusted Shops / buyer protection badges

- Verify the Trusted Shops badge (trustbadge) renders correctly in the footer and/or checkout area on all page types.
- Click the Trusted Shops badge and confirm it opens the correct overlay or redirect to the Trusted Shops profile page.
- Confirm the badge shows the current rating score, review count, and validity date pulled live from the Trusted Shops API.
- Simulate the Trusted Shops widget script being unavailable (block `widgets.trustedshops.com`): confirm no JavaScript errors are thrown, no layout is broken, and the missing badge area does not cause noticeable layout shift.
- Verify the Trusted Shops trustbadge script is loaded in a consent-aware manner: it must not set cookies or make tracking requests before the user has consented to the appropriate category (typically "marketing" or "functional").
- After consent is withdrawn, confirm Trusted Shops cookies are not set on subsequent page loads.
- Check the buyer protection opt-in overlay in the order confirmation flow (if applicable): verify it appears, can be accepted or declined, and the choice is correctly recorded.
- Verify the Trusted Shops review widget (if embedded on product or homepage) displays current reviews and star ratings correctly.
- Confirm the badge is visible and correctly positioned on mobile without overlapping other elements (e.g., sticky chat widget or add-to-cart bar).
- Validate that the Trusted Shops integration is documented in the privacy policy and cookie declaration.

---

### Review widgets (Trustpilot, Google Reviews)

- Verify the Trustpilot or Google Reviews widget loads and displays the correct aggregate star rating and review count on the homepage, product pages, and/or footer.
- Confirm individual reviews shown in the widget are current, correctly attributed, and match what is visible on the review platform directly.
- Test that clicking "Read all reviews" or the widget CTA opens the correct platform page (Trustpilot profile or Google Business profile) in a new tab.
- Simulate the review widget script being unavailable (block `widget.trustpilot.com` or the Google Reviews embed endpoint): confirm no errors are thrown, the page layout remains intact, and a graceful fallback (e.g., hidden widget, static fallback text) is shown.
- Verify the review widget only loads after consent for the corresponding category has been granted (typically "marketing" or "statistics"). Before consent, the widget area must show a consent prompt or remain hidden.
- Confirm no cookies or tracking beacons from the review platform are set before consent.
- After consent is rejected, verify the widget does not load and no network requests to the review platform are made.
- Check that the structured data (Schema.org `AggregateRating`) on product and category pages reflects the same data as the widget — no inconsistency between visible score and schema.
- Test the widget rendering on mobile: confirm it does not overflow its container or cause horizontal scrolling.
- Confirm the widget is accessible: star ratings have text equivalents for screen readers.
- Verify the review platform integration is listed in the privacy policy with correct data processing description.

---

### Social media embeds

- Verify all social media embeds (Twitter/X posts, Instagram posts, Facebook posts, TikTok, LinkedIn) load correctly and display the intended content.
- Confirm that social media embeds are blocked by default until the user grants consent for the relevant category (typically "marketing" or "external media"). Before consent, a placeholder with a consent prompt must be shown.
- Click the consent prompt within the embed placeholder: confirm consent is recorded and the embed loads immediately without a page reload.
- After consent is given once, verify embeds load on subsequent page visits without showing the prompt again (consent is persisted).
- After consent is revoked, verify embeds are replaced by the consent placeholder on the next page load.
- Simulate the social platform script being unavailable: confirm the placeholder or a fallback message is shown without JavaScript errors.
- Verify that no third-party cookies from the social platform are set in the browser before consent is granted.
- Test that embedded posts are responsive and do not cause horizontal overflow on mobile viewports.
- Confirm embedded videos (e.g., embedded tweets with video) respect autoplay policies and do not autoplay with sound.
- Check that embeds do not cause significant layout shift (CLS) while loading.
- Verify the correct post is embedded (no outdated or deleted posts causing broken embeds).

---

### YouTube/Vimeo videos

- Verify all embedded YouTube and Vimeo videos load correctly and the correct video is displayed (matching title/thumbnail).
- Confirm video playback works: play, pause, seek, volume control, full-screen, and quality selection all function correctly.
- Verify YouTube embeds use the privacy-enhanced mode (`youtube-nocookie.com`) by default. If not, confirm the embed is blocked until marketing consent is granted.
- Vimeo embeds: confirm DNT (Do Not Track) or equivalent privacy settings are applied where supported.
- Test that video embeds are blocked before consent and a clickable placeholder (thumbnail + consent prompt) is shown instead. After clicking, consent is recorded and the video loads in place.
- After consent is revoked, the video placeholder with the consent prompt is restored on the next page load.
- Simulate the YouTube/Vimeo CDN being unavailable: confirm the placeholder remains, no JavaScript errors are thrown, and the page layout is not broken.
- Verify videos are responsive: they resize correctly on all viewport widths without horizontal scroll or overflow.
- Test autoplay behavior: videos must not autoplay with sound on page load (violates browser policies and GDPR consent requirements).
- Confirm the video embed does not block the main thread or negatively impact the page's LCP or INP scores beyond acceptable thresholds.
- Verify captions/subtitles are available and togglable for accessibility.
- Check that the video embed area is keyboard accessible (can be focused and activated via keyboard).

---

### Chat widget (Zendesk, Intercom, Tidio)

- Verify the chat widget icon appears in the correct position (typically bottom-right) on all page types without overlapping critical UI elements (add-to-cart button, checkout button, cookie banner).
- Click the chat widget icon and confirm the chat window opens correctly on desktop and mobile.
- Send a test message and confirm the message is delivered and a response (bot or agent) is received within the expected time.
- Confirm the chat widget is only loaded after consent for the relevant category (typically "functional" or "marketing") has been granted. Before consent, no widget script requests or cookies should appear.
- After consent is rejected, verify the chat widget does not load and no network requests to the chat platform are made.
- After consent is withdrawn, verify the widget is removed on the next page load.
- Simulate the chat platform being unavailable (block the widget script endpoint): confirm the widget fails silently, no JavaScript errors are thrown, and no layout space is reserved for the missing widget.
- Test the offline/away state: when agents are not available, confirm the chat widget shows an offline message or switches to a contact form fallback automatically.
- Verify chat history persistence: navigate between pages while a chat is open and confirm the conversation is maintained without interruption.
- Test the widget on mobile: confirm it does not cover the page content when minimized and the chat window is usable on small screens.
- Test widget accessibility: confirm the chat button has an accessible label and the chat window can be navigated via keyboard.
- Verify that conversations containing personal data are handled according to the privacy policy.
- Confirm the chat widget does not significantly impact page load performance (lazy-loaded, non-render-blocking).

---

### Push notifications

- Verify the push notification permission prompt appears at the appropriate moment (not immediately on page load — ideally triggered by a user action or after engagement).
- Allow push notifications in the browser and confirm a subscription is successfully created (check in the notification platform's dashboard).
- Trigger a test push notification from the admin/platform and confirm it is delivered to subscribed browsers within the expected time.
- Verify the notification content (title, body, icon, URL) is correct and clicking the notification opens the correct page.
- Decline the push notification permission prompt and confirm no subsequent prompt appears on the same visit (browsers block repeated prompts, but the site should respect this).
- Verify the push notification opt-in is tied to the cookie consent mechanism: the permission prompt must not be shown before the user has given consent for marketing/notifications.
- Simulate the push notification service (e.g., Firebase Cloud Messaging, OneSignal) being unavailable: confirm the site functions normally with no errors and the missing subscription silently fails.
- Test unsubscribe flow: if the shop provides a "manage notifications" page, verify that unsubscribing removes the subscription from the platform correctly.
- Verify push notifications work across supported browsers (Chrome, Edge, Firefox) — note that Safari on iOS has separate requirements (iOS 16.4+ PWA).
- Confirm push notification service workers do not interfere with other service workers (e.g., PWA cache worker).
- Verify the push notification integration is documented in the privacy policy.

---

### Affiliate tracking

- Verify that affiliate links with tracking parameters (e.g., `?ref=`, `?aff=`, `?utm_source=affiliate&...`) correctly set the affiliate cookie upon page visit.
- Confirm the affiliate cookie is set with the correct expiry duration (as documented and disclosed in the privacy policy).
- Complete a purchase after arriving via an affiliate link and verify the order is attributed to the correct affiliate in the tracking platform.
- Test that affiliate attribution survives navigation across multiple pages within the same session.
- Test cookie persistence: close the browser, reopen it, and complete a purchase within the cookie validity window — confirm the affiliate attribution is still recorded.
- Verify affiliate tracking cookies are only set after marketing consent has been granted. Before consent, no affiliate tracking cookies should be set even if the affiliate link was followed.
- After consent is rejected, confirm clicking an affiliate link does not set the tracking cookie.
- Simulate the affiliate tracking script being unavailable: confirm the user can still complete the purchase without errors and the shop functions normally (tracking fails silently).
- Test session attribution logic with multiple affiliate links in sequence: confirm the correct "last-click" or "first-click" attribution model is applied as configured.
- Verify that affiliate tracking parameters are not passed to third-party URLs in a way that leaks user data.
- Confirm the affiliate cookie is listed in the cookie declaration with correct name, purpose, and expiry.

---

### Retargeting pixels

- Verify all configured retargeting pixels (Meta/Facebook Pixel, Google Ads Remarketing, Pinterest Tag, TikTok Pixel, etc.) fire on the correct pages and events (page view, product view, add to cart, purchase).
- Confirm retargeting pixels are completely blocked before the user grants marketing consent. Use browser DevTools network tab to verify zero requests to pixel endpoints before consent.
- After granting marketing consent, confirm all configured pixels fire correctly and are visible in the respective platform's event debugger (Meta Pixel Helper, Google Tag Assistant).
- After revoking marketing consent, verify pixel scripts are not loaded and no pixel requests are made on subsequent page loads.
- Verify that Consent Mode v2 signals (`ad_storage: denied`, `analytics_storage: denied`) are correctly sent to Google Ads and GA4 before consent and updated to `granted` after consent.
- Confirm the purchase pixel event fires exactly once per confirmed order — not on payment failure pages or when the user navigates back to the confirmation page.
- Verify purchase pixel events contain correct data: order value (without VAT if configured), currency, order ID, and product IDs.
- Test pixel firing with an active ad blocker: confirm the shop functions normally, the purchase completes, and no JavaScript errors are thrown (pixels fail silently).
- Simulate the pixel script CDN being unavailable: confirm no errors are surfaced to the user and checkout completes normally.
- Verify that the `purchase` event is not fired for test/sandbox orders.
- Confirm all retargeting pixels are listed in the cookie consent banner and privacy policy with correct descriptions and data recipients.
- Test server-side event tracking (Conversions API / server-side tagging) if implemented: confirm events are correctly deduplicated against browser-side pixel events.
