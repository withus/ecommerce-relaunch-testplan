# 48. Live Chat & Customer Support Integration

### Chat widget loads and opens correctly

- Verify the chat widget script loads after the page's critical rendering path completes (not render-blocking).
- Confirm the widget initializes within 3 seconds on a standard broadband connection; set a clear failure threshold.
- Confirm the widget is absent on the page when the user has not yet given consent (or has rejected marketing/functional cookies), and loads only after consent is granted.
- Test that the widget loads correctly when cookies are fully disabled in the browser — widget must either degrade gracefully or display a static fallback link.
- Test widget load with an active ad blocker (e.g., uBlock Origin): confirm the page does not throw uncaught JS errors and that a fallback contact method is still accessible.
- Test widget initialization on a cold page load (no cache) vs. warm load (primed cache) — the widget must appear in both cases.
- Verify the widget opens to a chat panel on click with no visible delay exceeding 500 ms.
- Confirm that clicking the widget trigger multiple times in quick succession does not open duplicate panels or produce JS errors.
- Test widget open/close toggle: opening, then closing, then reopening must restore the chat panel to its previous state (not reset the conversation).
- Simulate a slow network (Chrome DevTools throttle to "Slow 3G") and confirm the widget either loads eventually or fails silently without breaking page interactivity.
- Test on all supported browsers (Chrome, Firefox, Safari, Edge) and confirm the widget renders and opens identically.

### Chat widget does not cover critical UI elements (CTA buttons!)

- Identify all primary CTA buttons shop-wide (Add to Cart, Buy Now, Proceed to Checkout, Place Order) and verify none are obscured by the default widget position in any viewport width (320 px, 375 px, 768 px, 1024 px, 1440 px).
- Inspect the widget's CSS `z-index` value; confirm it is lower than any modal, cookie banner, and checkout overlay, but high enough to remain clickable above regular content.
- Test the checkout page specifically: with the chat widget visible, confirm all checkout form fields, the address form, payment selectors, and the Place Order button are fully tappable/clickable with no overlap.
- Test on product pages with a sticky Add-to-Cart bar at the bottom: verify the widget icon does not sit on top of the sticky bar or shift its position.
- Scroll to the footer and confirm the widget does not overlap the legal links (imprint, privacy policy) which must remain fully clickable for compliance reasons.
- Resize the browser window to simulate tablet landscape (1024×768) and portrait (768×1024) and confirm the widget repositions or scales without covering any form element.
- Open the cookie consent banner simultaneously with the chat widget: confirm the two elements do not overlap and both remain independently operable.
- Open a product image lightbox/fullscreen gallery while the chat widget is visible: confirm the lightbox has a higher `z-index` and the chat widget does not bleed through.
- Test with the mobile sticky header present: the widget must not clip under the header or sit behind it.
- Verify that any auto-triggered chat invitation pop-up (proactive message bubble) does not obscure text content or CTAs for longer than a user-dismissible timeout.

### Chatbot answers to standard questions correct

- Define a baseline list of FAQ topics (shipping times, return policy, payment methods, product availability, discount codes) and verify the chatbot returns accurate, up-to-date answers for each.
- Test that the chatbot's answers match the live content of the corresponding FAQ/CMS pages — run this check again after any content update.
- Enter a question with a minor spelling error (e.g., "retrun policy") and confirm the chatbot either normalizes it or prompts for clarification rather than returning a "not understood" error.
- Test fully ambiguous input ("I have a problem") and confirm the chatbot presents categorized follow-up options rather than a generic failure message.
- Verify the chatbot does not hallucinate prices, dates, or product names — all factual responses must be sourced from the shop's actual data.
- Test intentionally malicious input in the chat field (e.g., `<script>alert(1)</script>`, SQL fragments): confirm the chat UI escapes all output and no script executes.
- Test a question in a language other than the default shop language; confirm the chatbot either responds correctly in that language or gracefully flags the language limitation.
- Confirm that answers to legally sensitive questions (cancellation rights, warranty law) contain accurate wording that aligns with the published terms and conditions.
- Verify the chatbot response time is under 2 seconds for scripted/FAQ-type answers.
- After a chatbot session, confirm no personal data (email, address, order number typed during the session) is stored in browser localStorage or sessionStorage without consent.

### Handoff chatbot → human agent works

- Trigger a handoff request explicitly (e.g., typing "speak to a human" or clicking "Connect to agent") and confirm the transition message appears within 5 seconds.
- Verify the full conversation transcript from the chatbot phase is visible to the human agent immediately upon handoff — no context loss.
- Test handoff during business hours: confirm a human agent actually receives and accepts the chat within the defined SLA window.
- Test handoff outside business hours: confirm the user receives an honest "no agents available" message and is offered an alternative (email, callback, contact form) rather than an indefinite waiting spinner.
- During the handoff waiting queue, confirm the user sees an estimated wait time or queue position, and that this counter updates in real time.
- If the user navigates to a different page during the handoff queue, confirm the chat session and queue position are preserved (tied to session, not page).
- Verify that the human agent can send attachments (e.g., return label PDF) and that the file appears inline in the customer chat window without requiring a download link click.
- Confirm that closing the chat panel during a human agent session presents a confirmation dialog ("Are you sure you want to end the chat?") to prevent accidental disconnection.
- Test what happens if the human agent disconnects unexpectedly: verify the customer receives an automatic notification and is re-queued or offered a fallback option.
- Verify the complete chat transcript (including the human phase) is sent to the customer's email address at session end (if the feature is advertised).

### Chat history persists across page changes

- Start a chat conversation, navigate to a different product page (via internal link), and confirm the chat panel reopens with the full message history intact.
- Start a chat conversation, navigate using the browser Back button, and confirm the history is not lost or duplicated.
- Start a chat conversation as a guest and refresh the page (F5): confirm history persists via session cookie or `sessionStorage` without requiring re-identification.
- Start a chat conversation, navigate to checkout, complete a purchase, land on the thank-you page: confirm the chat history is still accessible and the conversation is not reset.
- Test history persistence across a soft navigation (SPA-style) vs. a full-page navigation (traditional page load) — behavior must be consistent.
- Log in mid-conversation (opening the login modal without leaving the page): verify the chat history merges with any existing account-level history rather than being discarded.
- Log out during an active chat session and confirm the history is either cleared (for privacy) or retained under a guest session, with clear UI indication of the state.
- Open the shop in two different browser tabs: confirm each tab maintains its own independent chat session, with no cross-tab interference or duplication.
- Test on mobile: switch to a different app and return to the browser tab — confirm the chat session is still alive and history is intact (no timeout for short app-switches).
- Confirm that chat history displayed in the widget matches, line-for-line, what is stored in the support platform's backend transcript.

### Chat widget usable on mobile

- On a physical iOS device (Safari) and Android device (Chrome), confirm the chat widget icon is visible, at least 44×44 px in tap target size, and does not require precise tapping to open.
- Open the chat panel on mobile and confirm the keyboard does not push the chat input field off-screen (the panel must scroll or resize to keep the input field above the virtual keyboard).
- Test that the chat panel does not cover the browser's back/navigation buttons or the address bar in any scroll position.
- In landscape orientation, confirm the chat panel remains fully usable and does not clip at the sides of a narrow viewport.
- Test swipe-to-dismiss or swipe-to-scroll within the chat panel: swipe up/down inside the message list must scroll messages, not trigger the page behind the panel.
- Verify that tapping outside the chat panel (on the shop page area) on mobile either has no effect or triggers the close action with a clear affordance — it must not accidentally submit forms on the page behind.
- Confirm the chat panel on mobile is not accidentally triggering iOS Safari's "pull-to-refresh" when scrolling to the top of the message list.
- Test the chat widget alongside the mobile sticky Add-to-Cart bar: both must be simultaneously visible and independently tappable without one obscuring the other.
- On low-memory mobile devices or with many browser tabs open, confirm the chat widget does not cause noticeable JS jank or page scroll lag.
- Test voice dictation input into the chat field on both iOS and Android — the text must be correctly inserted and submitted.
- Confirm the chat widget is excluded from the mobile viewport's print stylesheet so it does not appear on printed/PDF pages.

### Offline mode of chat (contact form instead of live chat)

- Disable live chat availability in the support platform backend and reload the shop: confirm the widget automatically switches to a static contact form or a "leave a message" form, not a broken chat panel.
- Verify the offline contact form requests at minimum: name, email, subject, message — and that all fields include client-side validation with descriptive error messages.
- Submit the offline contact form with a valid input and confirm a server-side submission confirmation is displayed immediately and a confirmation email is sent to the customer.
- Submit the offline contact form with an invalid email address and confirm the form is not submitted and the field is highlighted with an actionable error message.
- Submit the offline contact form with all required fields empty and confirm each empty field is individually flagged, not just the first one encountered.
- Test the offline form for XSS: enter `<img src=x onerror=alert(1)>` in the message field and confirm the submitted content is sanitized in the confirmation email and in the support backend.
- Confirm the offline form submission is protected against CSRF (form includes a CSRF token or equivalent).
- Test the rate-limiting behavior: submit the offline form 10 times in 2 minutes from the same IP and confirm a rate-limit response is returned and the submissions are not all forwarded to the support inbox.
- Simulate a network error during offline form submission (DevTools → Network → Offline) mid-submit: confirm the form content is preserved and the user is shown a retry option — not a blank form.
- Once live chat comes back online (re-enable in backend), reload the shop and confirm the widget seamlessly transitions back to live chat mode without requiring a hard page refresh from the user.
- Verify the offline state is reflected in real time (without page reload) if the support team goes offline mid-session — the existing chat panel should display an in-chat notice, not just silently stop responding.

### Help center / FAQ pages searchable

- Navigate to the help center and enter a search query for a known topic: confirm relevant results appear within 2 seconds.
- Enter a search query using natural language ("how do I return my order") and confirm the results include the returns FAQ article, even if the article title uses different phrasing.
- Enter a query that returns zero results and confirm a helpful fallback is shown — suggested categories, a link to the contact form, or a prompt to start a chat — not just an empty results list.
- Enter special characters in the search field (`<`, `>`, `"`, `%20`, `--`): confirm no JS errors occur, results are safely rendered, and no XSS or SQL injection vector is exposed.
- Test the FAQ search from within the chat widget (if the widget offers inline FAQ search before connecting to an agent): confirm results link to the correct help center articles.
- Verify that help center articles linked from the chat widget open in a new tab (not hijacking the current shop page), preserving the shopping session.
- Confirm that the help center search is accessible via keyboard (Tab to the search field, Enter to submit, arrow keys to navigate results).
- On mobile, confirm the help center search field receives keyboard focus on tap without the keyboard obscuring the input or results list.
- After updating an FAQ article, confirm the search index is refreshed within the documented cache window (e.g., within 24 hours) so stale content does not appear in results.
- Test that the FAQ/help center search is excluded from the main shop's product search — a query entered in the shop header search must not surface FAQ articles, and vice versa, unless intentionally designed to do so.
