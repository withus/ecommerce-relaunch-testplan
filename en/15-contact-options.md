# 15. Contact Options

### Contact form validation

- Submit the form with all fields empty — verify that each required field shows an individual, descriptive error message
- Submit with only whitespace in required fields (spaces, tabs) — verify the form does not accept blank-looking inputs as valid
- Enter an email address without `@` (e.g. `userdomain.com`) — verify an inline validation error appears
- Enter an email address with `@` but no domain (e.g. `user@`) — verify rejection
- Enter a syntactically valid but unreachable email (e.g. `test@localhost`) — verify the form accepts or rejects it according to spec (usually accept at frontend, bounce handling at backend)
- Exceed the maximum character limit on the message textarea — verify the field is capped or an error is shown, and the character counter updates in real time if present
- Enter a subject or name field with only numbers or special characters — verify no crash occurs and the entry is stored/rejected according to business rules
- Paste HTML tags into a text field (e.g. `<b>test</b>`) — verify output is escaped when displayed in the confirmation or admin panel (XSS protection)
- Submit the form via keyboard only (Tab to each field, Enter to submit) — verify full keyboard operability and that focus order is logical
- Verify that required fields are marked visually (asterisk or label) and that their `required` attribute is present in the HTML
- Test the phone number field (if present) with letters, special characters, and international formats (e.g. `+49 30 123456`) — verify only valid formats are accepted or that the field is free-text with server-side sanitisation
- Verify that validation errors are announced to screen readers via ARIA live regions or `aria-describedby` associations

### Contact form submission

- Fill in all fields correctly and submit — verify a success message is displayed immediately on the same page or on a confirmation page
- Verify the success message clearly states what happens next (e.g. "We will respond within 24 hours")
- Click the submit button twice rapidly — verify the form is not submitted twice (double-submit protection via button disabling or server-side deduplication)
- Submit the form and then press the browser back button — verify resubmission is prevented (PRG pattern or equivalent)
- Submit while on a slow/throttled network connection — verify a loading indicator is shown and the user is not left without feedback
- Verify that after successful submission the form is cleared or the user is taken to a confirmation state, preventing accidental reuse
- Submit the form with JavaScript disabled — verify the form still submits via native HTML POST and the user receives a confirmation (graceful degradation)
- Simulate a server-side error during submission (5xx response) — verify the user sees a user-friendly error message and is invited to try again, and that no sensitive technical detail (stack trace, file path) is exposed
- Verify all submitted data appears correctly in the admin backend or is forwarded to the configured inbox
- Submit from a mobile device using the on-screen keyboard — verify no layout shift or modal overlap obscures the submit button

### Contact form confirmation email

- Complete a valid submission and check the customer's inbox — verify the auto-reply arrives within an expected timeframe (e.g. under 2 minutes)
- Verify the confirmation email contains: sender name, reply-to address pointing to the support inbox, and a copy of the submitted message
- Verify the "From" address matches the shop domain (no `noreply@thirdparty.com`) and is SPF/DKIM authenticated
- Check that the confirmation email renders correctly on desktop clients (Gmail, Outlook) and mobile clients (iOS Mail, Android Gmail)
- Check the email in plain-text view — verify it is readable without HTML rendering
- Verify the email does not end up in the spam folder for major providers (Gmail, Outlook, GMX)
- Submit with an address that uses subaddressing (e.g. `user+test@example.com`) — verify the confirmation is still delivered
- Verify that the internal notification email sent to the shop team contains all form fields including the customer's reply-to address
- Verify that HTML injected into form fields (e.g. `Subject: Free money\r\nBcc: spam@example.com`) does not result in email header injection
- If the form includes a file attachment field, verify that the attachment is included in the notification email and that only allowed file types (e.g. PDF, JPEG, PNG) are accepted

### Phone number clickable (mobile)

- Open the contact page on a real iOS device and tap the phone number — verify the native dialer opens with the number pre-filled
- Open the contact page on a real Android device and tap the phone number — verify the dialer opens correctly
- Inspect the HTML — verify the phone number uses a `tel:` href (e.g. `<a href="tel:+4930123456">`) rather than plain text
- Verify the displayed number format matches the `tel:` href value (no formatting characters that break the link, e.g. extra spaces without them being stripped in the href)
- Verify that on desktop browsers the click either opens a registered call application or provides a sensible fallback (e.g. a tooltip or copy-to-clipboard)
- Verify the tap target is at least 44×44 px on mobile (WCAG 2.5.5 / Apple HIG guideline)
- If the phone number appears in the footer, header, and contact page, verify all instances are clickable and use the same canonical number
- Verify the number is not auto-detected and reformatted by the browser in a way that breaks the `tel:` link (check `<meta name="format-detection" content="telephone=no">` strategy if manual links are used)

### Email address clickable

- Click the email address on the contact page on desktop — verify the default mail client opens with the address pre-filled in the "To" field
- Tap the email address on mobile — verify the mail app opens correctly
- Inspect the HTML — verify a `mailto:` href is used
- Verify that the email address is not plain text (which would require copy-pasting) and not obfuscated in a way that breaks the link for users (only obfuscation that remains functional is acceptable)
- Verify the email address is protected against straightforward harvesting bots — check whether the shop uses a rendering technique (CSS reversal, JS injection) or accepts the trade-off of plain `mailto:` visibility
- If a subject line is pre-filled in the `mailto:` (e.g. `mailto:support@shop.com?subject=Inquiry`), verify the subject appears correctly in the mail client
- Verify the displayed email address matches the `mailto:` href destination
- Verify the email address resolves to an actively monitored inbox (send a test message and confirm receipt)

### Live chat widget

- Open the shop in a supported browser — verify the chat widget loads within an acceptable time (under 3 seconds) without blocking page rendering
- Verify the widget does not overlap the "Add to Cart" button, checkout CTA, or cookie consent banner on any viewport width (320 px through 1920 px)
- Click the chat icon — verify the chat window opens and a greeting or bot prompt is displayed within 2 seconds
- Send a message — verify it is received on the operator side and a response can be sent back
- Resize the browser or rotate a mobile device while the chat is open — verify the widget remains usable and does not cover critical content
- Close the chat window and navigate to a different page — verify the chat session state is preserved (chat history visible when reopened)
- Leave the chat idle for the configured timeout period — verify a "Are you still there?" prompt or graceful session end is shown
- Test the offline/out-of-hours state — verify the widget shows a contact form or a message indicating when agents are next available, rather than showing a misleading "Chat now" button
- Verify the chat widget is keyboard accessible: focusable via Tab, openable with Enter/Space, and closable with Escape
- Verify that the chat widget script does not fire tracking events before cookie consent has been given (GDPR)
- Block the chat CDN domain via the browser's network throttle — verify the page remains fully functional and no JavaScript errors are thrown
- Test the chatbot's response to common support questions (order status, return policy) — verify answers are accurate and not outdated

### WhatsApp button

- Tap the WhatsApp button on a mobile device with WhatsApp installed — verify the app opens with a pre-filled message and the correct business phone number
- Tap the button on a mobile device without WhatsApp installed — verify the user is redirected to the WhatsApp download page or the WhatsApp Web version, not to a broken link
- Click the button on desktop — verify it opens WhatsApp Web (`https://web.whatsapp.com/send?phone=...`) in a new tab
- Inspect the href — verify the URL uses the correct format: `https://wa.me/[international number without + or spaces]` or `https://api.whatsapp.com/send?phone=...`
- Verify the pre-filled message text (if any) is URL-encoded and renders correctly in WhatsApp without garbled characters
- Verify the button is visible without excessive scrolling on the contact page and on mobile at common viewport heights
- Verify the WhatsApp button does not appear during out-of-business hours without a caveat, or alternatively that a response-time disclaimer is shown
- Verify the button has an accessible label (`aria-label="Contact us via WhatsApp"` or equivalent) for screen readers
- Verify the WhatsApp business number is identical across all pages and matches the officially registered business number
- Verify that the WhatsApp link/button does not load the WhatsApp Business API or any tracking pixel before cookie consent is given

### Social media links

- Click each social media icon (Facebook, Instagram, Twitter/X, YouTube, Pinterest, LinkedIn, TikTok — whichever apply) from the contact page and footer — verify each opens the correct, active profile in a new tab
- Verify all links use `target="_blank"` combined with `rel="noopener noreferrer"` to prevent tab-napping security issues
- Verify that none of the social media URLs redirect through a tracking redirect that could become a broken link if the redirect service changes
- Check all social profile URLs for correctness: no typos in handle names, no links to deleted or placeholder profiles
- Verify the social media icons have accessible labels (alt text on `<img>` or `aria-label` on `<a>`) for screen readers
- Verify the icon images load correctly and are not broken or replaced by generic broken-image placeholders
- Verify the link to Twitter/X reflects any handle change (many shops still link to the old `twitter.com` username which may have changed)
- Verify consistent social media links across the footer, contact page, and any "Follow us" banners — no conflicting or outdated handles on different pages
- Check that social media links are excluded from the sitemap or marked `nofollow`/`noindex` as appropriate for SEO

### FAQ / self-service

- Open the FAQ page and verify all questions expand/collapse correctly (accordion behaviour) — test both click and keyboard (Enter/Space)
- Verify that deep-linking to a specific FAQ item via URL hash (e.g. `/faq#returns`) scrolls to and opens the correct item
- Search for a common keyword within the FAQ search field — verify relevant results are returned and irrelevant ones are filtered out
- Search for a term that yields no results — verify a user-friendly "no results" message is shown with a suggestion to contact support
- Verify FAQ content is up to date: check shipping times, return window, and payment methods against the current shop settings
- Verify that answers containing links (e.g. "see our returns page") use correct, non-broken internal links
- Verify the FAQ page has proper structured data (`FAQPage` schema) so Google can display rich results
- Verify the FAQ is accessible: all toggle controls are `<button>` elements (or equivalent with `role="button"`), expanded/collapsed state is communicated via `aria-expanded`, and answer panels use `aria-hidden` when collapsed
- Verify that the FAQ page loads in a reasonable time even when many questions are present (no eager rendering of all hidden panels causing layout thrash)
- Verify the FAQ self-service path reduces contact form load: confirm there is a clear escalation path ("Didn't find your answer? Contact us") from every FAQ dead-end
- On mobile, verify the FAQ accordion is fully operable with touch and that tapping an item does not require precise targeting on small text
- Verify that FAQ items indexed by search engines do not include outdated promotional details (old prices, discontinued products) that could mislead customers or create legal issues
