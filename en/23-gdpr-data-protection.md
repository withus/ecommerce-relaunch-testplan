# 23. GDPR / Data Protection / Legal

### Cookie consent banner

- **First-visit trigger:** Open the shop in an incognito/private browsing window with all cookies cleared. Verify the consent banner appears before any non-essential cookie is set (GDPR Art. 7; ePrivacy Directive Art. 5(3)).
- **No pre-tracking:** Using browser DevTools (Application > Cookies) or a cookie scanner (e.g., Cookiebot scan, CookieYes audit), confirm that zero analytics, advertising, or functional third-party cookies exist in storage before the user interacts with the banner.
- **Banner blocks scripts:** Open the Network tab in DevTools and reload the page. Verify no requests to Google Analytics, Meta Pixel, TikTok Pixel, Hotjar, or any other tracking domain are fired before consent is given.
- **Banner content requirements:** Verify the banner includes: (a) a clear description of cookie categories, (b) a link to the full privacy policy, (c) an "Accept All" button, (d) a "Reject All" or equivalent button, and (e) a "Settings / Manage Preferences" option — all equally prominent (no dark patterns).
- **Reject All button parity:** Confirm "Reject All" is at least as visible and equally accessible as "Accept All" (same size, same prominence, not hidden behind extra clicks) — required by EDPB Guidelines 05/2020 and national DPAs.
- **Banner reappears after clearing cookies:** Clear all cookies and storage via DevTools, reload — the banner must reappear immediately.
- **Banner does not reappear after valid consent:** Accept all cookies, reload the page multiple times across a session — the banner must not re-show until the consent record expires.
- **Consent record expiry:** Check that consent records have a maximum validity of 12 months (best practice; some CMPs use 6 months). After expiry, the banner must reappear on the next visit.
- **Consent granularity:** Verify the banner offers at least three separate categories: Necessary, Analytics/Statistics, and Marketing/Advertising — each independently toggleable.
- **Necessary cookies always on:** The "Necessary" toggle must be pre-checked and non-interactive (greyed out) since these do not require consent (ePrivacy Directive Art. 5(3), recital 66).
- **Banner on all entry points:** Navigate directly to a product page, a category page, and the checkout — confirm the banner appears on each, not just the homepage.
- **Mobile banner usability:** On a 375px viewport, verify the banner does not obscure the full viewport content, all buttons are tappable (min. 44x44px), and the banner is scrollable if text overflows.

---

### Cookie settings changeable

- **Consent Management Platform (CMP) reopenable:** Locate the "Cookie Settings" or "Privacy Settings" link in the footer. Verify it opens the full CMP preference center without requiring a page reload.
- **Per-category toggle works:** In the preference center, enable only Analytics, leave Marketing disabled, save, and use a cookie scanner or DevTools to confirm only analytics cookies (e.g., `_ga`, `_gid`) are set and no advertising cookies (e.g., `_fbp`, `_gcl_au`) are present.
- **Consent withdrawal works (Art. 7(3)):** Accept all categories, then reopen settings and disable Marketing. Save. Verify marketing cookies are deleted immediately (not just blocked going forward) — check `document.cookie` and Application > Cookies.
- **Settings persist across sessions:** Set a custom preference (e.g., Analytics only), close the browser, reopen — the preference center must reflect the saved state, not default back to "all accepted."
- **Settings persist across pages:** Accept Analytics only on the homepage, navigate to a product page, reopen settings — the saved preference must still show "Analytics: on, Marketing: off."
- **Timestamp and proof of consent stored:** Inspect the CMP consent cookie (e.g., `CookieConsent`, `euconsent-v2`) and verify it contains: a timestamp, the consent version, and per-category decision — required for accountability under Art. 5(2).
- **Version bump triggers re-consent:** Change the CMP configuration version (simulate a new cookie policy version). Verify existing users see the banner again and are asked to re-confirm consent.
- **Settings accessible without JavaScript:** Verify a fallback privacy settings page or static opt-out page exists for users with JavaScript disabled (graceful degradation).

---

### Only necessary cookies without consent

- **Baseline cookie audit (no interaction):** Using an automated scanner (Cookiebot, OneTrust Scanner, or browser extension "Cookie AutoDelete"), load the homepage without clicking anything. Export the list of all cookies set. Verify every cookie in that list falls into the "strictly necessary" category.
- **Strictly necessary definition check:** Review each cookie set without consent against the criteria: session management (`PHPSESSID`, `CSRF`), shopping cart persistence, authentication — none of these should include analytics, A/B testing, or advertising identifiers.
- **`_ga` / `_gid` not set before consent:** In DevTools > Application > Cookies, confirm `_ga`, `_gid`, `_gat`, or `GA_SESSID` cookies are absent before the user interacts with the banner.
- **`_fbp` / Meta Pixel not firing before consent:** In the Network tab filter by `facebook.net` — verify no pixel requests occur before consent.
- **Hotjar / Clarity not loading before consent:** Filter Network tab for `hotjar.com`, `clarity.ms` — no requests before consent.
- **Third-party fonts and CDNs:** Verify that Google Fonts, Google Maps embeds, or YouTube iFrames are either self-hosted or not loaded before consent (these transfer the visitor's IP to third-party servers = data processing under GDPR Art. 4(1)).
- **LocalStorage / SessionStorage audit:** Open Application > Local Storage in DevTools. Confirm no analytics or advertising data (e.g., `amplitude_unsent`, `mp_*`, `_hjid`) is written before consent.
- **Cookie on subdomain restriction:** Verify session and CSRF cookies use the `Secure`, `HttpOnly`, and `SameSite=Strict` or `SameSite=Lax` flags and are scoped appropriately (not set on `*.example.com` when only `shop.example.com` is needed).

---

### Analytics only after consent

- **GA4 fires only post-consent:** Give "Analytics" consent, then open the Network tab and filter by `google-analytics.com` or `analytics.google.com`. Confirm the `page_view` hit fires within 1–2 seconds of consent being given.
- **GA4 does not fire in reject state:** Reject all cookies, navigate through 5+ pages, and confirm no GA4 requests appear in the Network tab.
- **Consent Mode v2 signals correct:** If Google Consent Mode v2 is implemented, verify the `gtag('consent', 'default', {...})` call sets `analytics_storage: 'denied'` and `ad_storage: 'denied'` before the banner interaction. After consent, the update call must fire with the user's choices.
- **GA4 anonymize_ip / no PII in hits:** After accepting analytics consent, inspect GA4 hits in Network tab and confirm no email addresses, full names, or order IDs are transmitted as event parameters (GDPR Art. 5(1)(c) — data minimization).
- **Session recording tools (Hotjar/Clarity):** Accept Analytics consent, verify Hotjar or Clarity starts recording. Reject consent (or only give Marketing), verify recording does not start.
- **Server-side analytics if applicable:** If server-side GTM is used, verify the server-side container only activates analytics tags after a consent signal is received from the client (check GTM trigger conditions).
- **Analytics cookie duration matches documentation:** Accept analytics consent and check the `_ga` cookie expiry (should be 2 years, or whatever is documented in the privacy policy). Mismatch is a GDPR transparency violation (Art. 13(2)(a)).
- **Matomo / self-hosted analytics:** If a self-hosted analytics tool is used, verify it is configured with IP anonymization enabled, no cross-site tracking, and that data is stored on EU infrastructure (Art. 44ff. — transfers to third countries).

---

### Marketing cookies only after consent

- **Meta Pixel fires only post-marketing-consent:** Accept Marketing cookies. Filter Network tab by `connect.facebook.net` and `facebook.com/tr` — the pixel `PageView` event must fire. Reject marketing consent, repeat — no pixel requests must appear.
- **Google Ads remarketing tag:** Accept Marketing, verify `googleadservices.com` and `doubleclick.net` requests appear. Reject, verify they are absent.
- **TikTok / Pinterest / LinkedIn pixels:** For each applicable tracking pixel, repeat the accept/reject test and confirm none fire without marketing consent.
- **Retargeting cookie duration:** Accept marketing consent and check cookies like `_fbp` (90 days), `_gcl_au` (90 days), `IDE` (DoubleClick, ~13 months). Verify durations match those documented in the cookie policy.
- **Affiliate tracking cookies:** Identify any affiliate or referral tracking cookies (e.g., from Awin, Tradedoubler, CJ Affiliate). Confirm they are categorized under Marketing and not set without consent.
- **Email newsletter tracking pixels:** Inspect transactional email HTML for tracking pixels (e.g., `mailchimp.com/track/open`). These open-tracking pixels are fired server-side and do not require cookie consent, but verify the privacy policy discloses email open tracking (Art. 13 transparency).
- **A/B testing tools:** If Optimizely, VWO, or Google Optimize is used, verify the experiment assignment does not use persistent tracking cookies before consent. Verify the tool is gated behind the appropriate consent category.
- **Cookie scan report comparison:** Run a full cookie scan (e.g., via Cookiebot's free scan at cookiebot.com/en/cookie-checker/) and compare the list of detected cookies against the cookie policy table. Every cookie found must be listed in the policy with: name, provider, purpose, type, duration.

---

### Privacy policy complete + up to date

- **GDPR Art. 13 mandatory information present:** Verify the privacy policy includes: (1) identity and contact details of the controller, (2) contact details of the DPO (if applicable), (3) purposes and legal basis for each processing activity (Art. 13(1)(c)), (4) legitimate interests (if Art. 6(1)(f) is relied on), (5) recipients or categories of recipients, (6) third-country transfers and safeguards (Art. 46), (7) retention periods (Art. 13(2)(a)), (8) all eight data subject rights (Art. 13(2)(b)), (9) right to withdraw consent (Art. 7(3)), (10) right to lodge a complaint with the supervisory authority (Art. 13(2)(d)).
- **Legal basis for each processing activity:** Check that each processing activity (order processing, newsletter, analytics, fraud prevention, customer account) has a stated legal basis — Art. 6(1)(b) for contract performance, Art. 6(1)(a) for consent, Art. 6(1)(c) for legal obligation, Art. 6(1)(f) for legitimate interests.
- **Third-party service list complete:** Verify all third-party services used (Stripe, PayPal, Google Analytics, Mailchimp, Zendesk, Trustpilot, etc.) are listed in the privacy policy with their role (processor vs. controller), country of processing, and transfer safeguard (e.g., Standard Contractual Clauses, EU-US Data Privacy Framework).
- **Last updated date visible:** The privacy policy must show a "last updated" or "effective date" timestamp. Verify it reflects recent changes (e.g., if a new analytics tool was added in the last 12 months).
- **Privacy policy reachable from all pages:** Every page (homepage, product page, checkout, contact form, newsletter signup) must have a direct link to the privacy policy in the footer or within the relevant form.
- **Accessibility of privacy policy:** Verify the privacy policy is written in plain language, structured with headings, and not buried in a downloadable PDF as the only format (EDPB recommends layered notice approach).
- **Data retention periods documented:** For each data category (order data, customer accounts, server logs, analytics data), a specific retention period (e.g., "10 years for invoices per HGB §257") must be stated.
- **Supervisory authority contact listed:** Verify the competent supervisory authority (e.g., for Germany: Landesbeauftragter für Datenschutz of the respective Bundesland) is named with contact details.

---

### Legal notice / imprint complete

- **German DDG §5 / Austrian ECG §5 requirements:** Verify the imprint contains: full legal name of the company, complete postal address (no PO box), contact email address, phone number, registration court and registration number (if GmbH/AG/etc.), VAT ID (Umsatzsteuer-ID), responsible person for content (§55 RStV if applicable).
- **Imprint reachable within 2 clicks:** From any page on the website, the imprint must be accessible within at most 2 clicks. Verify it is linked in the footer and not hidden behind login or popups.
- **Imprint link in emails:** Check transactional email templates — the imprint or a link to it must be present in the footer of every commercial email.
- **Imprint on mobile:** Verify the imprint link is visible and tappable on a 375px viewport (not hidden in a collapsed footer section that doesn't expand).
- **No broken imprint link:** Run a broken link check on all footer links. The imprint link must return HTTP 200 with actual content (not redirect loop or 404).

---

### Terms & conditions current

- **T&C version date visible:** The T&C document must display a version number or "last updated" date. Verify it matches any recent business changes (e.g., new payment methods, new return policy, new shipping regions).
- **T&C linked at checkout:** In the order summary step of checkout, there must be a checkbox linking to the T&C. Clicking the link must open the T&C (new tab or modal). The checkbox label must be clearly worded (not vague like "I agree to everything").
- **T&C acceptance mandatory:** Attempt to place an order without checking the T&C checkbox. Verify an error prevents order completion.
- **T&C downloadable / printable:** The T&C must be presented in a format the customer can save (printable HTML, PDF download, or copy-pasteable text) — required for persistent access under BGB §312i.
- **No unfair clauses check (BGB §305ff.):** Review the T&C for common invalid clauses: blanket liability exclusions, unlimited right to change prices, unreasonable contract terms. Flag any clauses that likely violate AGB-Recht for manual legal review.
- **T&C for B2B vs. B2C:** If the shop serves both segments, verify separate or clearly differentiated T&C sections exist, since consumer protection rules (Widerrufsrecht, §13 BGB) do not apply to B2B.

---

### Cancellation policy + sample form

- **14-day right of withdrawal displayed (§355 BGB / EU Directive 2011/83/EU):** Verify the cancellation policy clearly states the 14-day withdrawal period, starting from the day of goods receipt.
- **Cancellation policy in order confirmation email:** The full cancellation policy (or a link to it with the option to save/print) must be included in the order confirmation email — this is required for the withdrawal period to start (§356 BGB).
- **Cancellation policy on checkout:** Link to the cancellation policy must be present and clickable on the checkout page (order summary step), adjacent to or within the legal checkbox area.
- **Sample cancellation form downloadable:** Per §356 BGB Anlage 2, a standardized cancellation form must be available for download (PDF preferred). Verify the PDF is functional, contains all required fields, and is not corrupt.
- **Sample form content correct:** The sample form must contain: addressee (company name, address, fax/email), statement of withdrawal, order date, receipt date, customer name and address, customer signature field and date.
- **Cancellation policy not applicable to exceptions:** Verify the policy notes statutory exceptions to the right of withdrawal (custom-made products, sealed hygiene goods, digital content if download started) — Art. 16 Consumer Rights Directive.
- **Cancellation period extended if policy missing:** Test: if the cancellation policy is not provided at order time (simulate by removing from email), the withdrawal period extends to 12 months + 14 days (§356(3) BGB). Document this risk for the development team.

---

### Right to data deletion (delete account)

- **Delete account option in customer account:** Navigate to Account Settings. Verify a "Delete Account" or "Request Account Deletion" option exists (GDPR Art. 17 — right to erasure).
- **Deletion confirmation step:** Clicking delete must require a confirmation step (e.g., "Type your email to confirm" or "Enter your password") — to prevent accidental deletion.
- **Confirmation email on deletion request:** After initiating deletion, the user must receive a confirmation email stating that the request was received and will be processed within the legally required timeframe (Art. 12(3) — within 1 month).
- **Actual data erasure within 30 days:** 30 days after a test deletion request, verify in the admin backend that the customer record has been anonymized or deleted (PII: name, email, address, phone — not order data required for tax/legal retention).
- **Order data retention despite deletion:** Verify that order records are retained for the legally required period (10 years under HGB §257 / AO §147 in Germany) even after account deletion — but pseudonymized (name/email replaced with "Deleted User" or similar).
- **Right to erasure not applicable where retention required:** Test that the system does not delete invoice data, order IDs, or payment records that are subject to legal retention obligations (Art. 17(3)(b) GDPR).
- **Erasure of newsletter consent:** After account deletion, verify the newsletter subscription is also cancelled and the email address removed from the mailing list (or anonymized) within the same timeframe.
- **Partial erasure request via email/contact form:** Submit a data deletion request via the contact form (for users who cannot log in). Verify the shop has a documented process to handle this within the Art. 12(3) deadline, and that the request triggers the same anonymization workflow.

---

### Data export (GDPR Art. 20)

- **Data portability feature present:** In the customer account, verify a "Download my data" or "Export my data" feature exists (GDPR Art. 20 — right to data portability).
- **Export format machine-readable:** Verify the export is provided in a structured, commonly used, machine-readable format — JSON or CSV are acceptable; a PDF printout alone is NOT sufficient for Art. 20 compliance.
- **Export content complete:** The export must include all personal data provided by the user: name, email, address(es), order history, reviews submitted, wishlist, newsletter preferences, saved payment methods (tokenized only — no full card numbers).
- **No sensitive data in export:** Verify the export does NOT include password hashes, internal admin notes, fraud flags, or system-internal fields not related to the data subject.
- **Export delivered securely:** The download link must be authenticated (only accessible when logged in) and must not be accessible via a guessable URL. If delivered by email, the link must expire after 24–48 hours.
- **Response time compliance:** Submit an Art. 20 request and verify the export is available within 30 days (Art. 12(3)). Automated self-service exports should be near-instant.
- **Export request for non-logged-in users:** Submit a data export request via the contact form as a non-registered user (or a user who deleted their account). Verify the team has a process to fulfill this and the response is sent within 30 days.

---

### Newsletter double opt-in

- **Signup form submits to confirmation stage:** Enter an email in the newsletter form and click Subscribe. Verify the page/component shows a "Please check your email to confirm" message — not "You are now subscribed."
- **Confirmation email sent immediately:** Check the inbox (and spam folder) within 2 minutes of submission. The confirmation email must arrive promptly.
- **Confirmation email content (Art. 7 + TDDDG):** The email must contain: (a) what the subscriber signed up for, (b) the sender identity, (c) a single confirmation link, (d) a statement that if they did not request this, they can ignore the email — no marketing content in this email.
- **Confirmation link works:** Click the confirmation link. Verify it activates the subscription and redirects to a success page (not a 404 or generic homepage).
- **Confirmation link expiry:** Wait 48–72 hours without clicking the link. Attempt to click it — verify it has expired and the user is prompted to re-subscribe. Expired tokens protect against harvested email confirmation URLs.
- **No newsletters before confirmation:** Trigger a newsletter send to pending subscribers. Verify unconfirmed addresses do NOT receive the newsletter.
- **Consent timestamp logged:** In the admin backend (email service provider), verify each subscriber record shows: IP address at signup, timestamp of signup, timestamp of confirmation, and source URL — required as proof of consent under Art. 7(1) GDPR.
- **Double opt-in also at checkout newsletter checkbox:** In the checkout flow, if a "Subscribe to newsletter" checkbox is shown, verify that checking it also triggers a double opt-in confirmation email — not an immediate subscription.
- **Unsubscribe works from every email:** Every newsletter email must contain an unsubscribe link. Clicking it must unsubscribe the user within 10 business days (best practice: immediately). After unsubscribing, verify no further newsletters are sent.
- **Re-subscribe after unsubscribe:** Attempt to re-subscribe a previously unsubscribed email. Verify the double opt-in process is triggered again (the original consent is invalidated after unsubscribe).

---

### Checkbox texts correctly worded

- **T&C / cancellation policy checkbox wording:** The order confirmation checkbox must read something equivalent to: "I have read and agree to the [Terms & Conditions] and acknowledge the [Right of Withdrawal]" — with hyperlinks on the named documents. Generic "I agree to everything" wording is legally insufficient.
- **Newsletter consent checkbox wording:** The newsletter opt-in checkbox text must specify: (a) what the subscriber will receive, (b) how often (if known), (c) who the sender is, (d) that consent can be withdrawn at any time with a reference to the unsubscribe option — per Art. 7(2) GDPR and §7 UWG.
- **Privacy policy consent wording (if used):** For data processing requiring explicit consent (e.g., "Contact form data is processed per our [Privacy Policy]"), verify the checkbox text clearly identifies which data is processed and for what purpose — no bundled consent for unrelated processing activities (Art. 7(2) GDPR).
- **Separate checkboxes for separate purposes:** Verify that T&C acceptance, cancellation policy acknowledgment, and newsletter signup are NOT bundled into a single checkbox. Each requires a separate action (EDPB Guidelines 05/2020 on consent).
- **Checkbox labels are adjacent to checkboxes:** Visually verify that the label text is directly associated with its checkbox (not ambiguous about which checkbox it refers to), both visually and in the HTML (`<label for="...">` or wrapping `<label>`).
- **No pre-ticked checkboxes:** Load the checkout page and programmatically inspect all checkbox inputs in the form: `document.querySelectorAll('input[type="checkbox"]')` — verify `.checked` is `false` for all optional checkboxes (T&C consent is mandatory but must start unchecked; newsletter is optional and must start unchecked).

---

### No pre-checked checkboxes (opt-in)

- **Checkout — all optional checkboxes unchecked by default:** Inspect the checkout form HTML source (not rendered DOM) and verify no `checked` attribute is present on optional checkboxes (newsletter, marketing consent, third-party offers).
- **Registration form — newsletter unchecked:** On the registration form, verify the newsletter / marketing communications checkbox is unchecked by default.
- **Pre-selected consent = invalid consent:** Per GDPR Art. 4(11) and Art. 7, and confirmed by CJEU ruling (Planet49, C-673-17), pre-ticked opt-in boxes do not constitute valid consent. Document any failing checkbox as a critical GDPR violation.
- **JavaScript-based pre-checking:** Check if any JavaScript sets `checkbox.checked = true` after page load (e.g., via a CMP configuration or A/B test). Use the browser console: `document.querySelectorAll('input[type="checkbox"][checked]')` and also verify post-JS-execution state.
- **Cookie banner pre-checked categories:** In the cookie preference center, verify that Analytics and Marketing toggles are OFF (not ON) by default. The "Accept All" button is permitted, but individual toggles must default to off (EDPB Guidelines 05/2020, §64).
- **Opt-out vs. opt-in verification:** Verify that subscribing to the newsletter requires a positive action (checking a box), not opting out (unchecking a pre-selected box). Test the form's behavior: submit without touching the newsletter checkbox — verify the user is NOT subscribed.

---

### Data processing agreements (third-party)

- **DPA inventory list:** Verify the shop operator can produce a list of all data processors (companies processing personal data on behalf of the shop) with signed DPAs — required under Art. 28(3) GDPR. Typical processors: hosting provider, email service, analytics, payment processor, shipping label provider, CRM, CDN.
- **Hosting provider DPA:** Verify a signed DPA (Auftragsverarbeitungsvertrag, AVV) with the hosting provider is in place. For cloud providers (AWS, Google Cloud, Azure), check their DPA portal for a signed agreement.
- **Payment processor DPA / independent controller:** Clarify whether Stripe, PayPal, Klarna are data processors (requiring a DPA under Art. 28) or independent controllers (requiring a reference in the privacy policy under Art. 26 or Art. 13(1)(e)). Stripe generally acts as an independent controller; document this correctly in the privacy policy.
- **Analytics tool DPA:** Verify a signed DPA with the analytics provider (Google Analytics, Matomo, etc.) is in place. For Google Analytics, the DPA is accepted via the Google Ads / Analytics settings — verify it is signed in the account.
- **Email marketing provider DPA:** Verify a signed DPA with Mailchimp, Klaviyo, Brevo, or whatever ESP is used. Verify the ESP processes data within the EU or under approved transfer mechanisms (EU-US DPF, SCCs).
- **Third-country transfers documented:** For each processor located outside the EU/EEA (or using US-based infrastructure), verify an adequacy decision (Art. 45), SCCs (Art. 46(2)(c)), or binding corporate rules are in place and documented in the privacy policy.
- **Record of Processing Activities (Art. 30 ROPA):** Verify the shop operator maintains an up-to-date ROPA documenting all processing activities, legal bases, data categories, retention periods, and processors — this is mandatory for companies with 250+ employees, and recommended for all e-commerce operators.

---

### Cookie durations documented

- **Cookie policy / cookie table present:** Verify a dedicated cookie table exists (either in the privacy policy or a separate cookie policy page) listing every cookie used on the site.
- **Required columns present:** Each entry in the cookie table must include: (1) Cookie name, (2) Provider / Domain, (3) Purpose / Category, (4) Type (session vs. persistent), (5) Duration / Expiry, (6) Whether it is set before or after consent.
- **Duration accuracy — session cookies:** Verify cookies declared as "session cookies" in the policy actually expire when the browser session ends (no `Expires` or `Max-Age` attribute, or `Max-Age=0`). Use DevTools > Application > Cookies to check.
- **Duration accuracy — persistent cookies:** For each persistent cookie, compare the documented expiry in the cookie table against the actual `Expires` date shown in DevTools. Discrepancies (e.g., policy says "1 year" but cookie is set for 2 years) constitute a transparency violation under Art. 13(2)(a).
- **Third-party cookie durations:** For cookies set by third parties (Google, Meta, Hotjar), verify the durations in the cookie table match the actual cookies set. Third-party cookies are often updated by the providers — schedule quarterly reviews.
- **Automated cookie scan to detect undocumented cookies:** Run the site through Cookiebot's free scanner (cookiebot.com/en/cookie-checker/) or a similar tool. Compare the scan results with the cookie policy table. Any cookie found by the scanner that is absent from the policy table must be added.
- **Cookie policy update process:** Verify a documented process exists for updating the cookie table when new scripts or tags are added (e.g., via a GTM tag audit before publishing new tags). An outdated cookie policy with undeclared cookies is a DPA enforcement risk.
- **Link to cookie policy in consent banner:** Verify the consent banner contains a direct, working link to the cookie policy or the relevant section of the privacy policy — users must be able to review the full cookie list before deciding to consent (Art. 13(1) transparency).
