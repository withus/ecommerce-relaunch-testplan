# 51. Legal Compliance (detailed)

> **Scope:** German/EU legal requirements for e-commerce shops — DSGVO (GDPR), TDDDG, PAngV (Price Indication Regulation), Buttonloesung (Button Solution), Widerrufsbelehrung (Cancellation Policy), Impressumspflicht, AGB, Cookie Consent.
>
> **Applicable Law:** BGB, DDG, UWG, PAngV, GDPR, TDDDG, JuSchG, BGB-InfoV, MStV, UKlaG.
>
> **Test Environment:** All tests must be executed in Chrome, Firefox, Safari, and Edge on both desktop and mobile unless stated otherwise. Use incognito/private browsing for fresh-state tests.

---

## 51.1 Cookie Consent Banner Appears on First Visit

### TC-51.1.1 — Banner displays on first visit (fresh session)

| Field | Detail |
|---|---|
| **Test ID** | TC-51.1.1 |
| **Priority** | Critical |
| **Precondition** | Browser with no cookies, cache, or local storage for the shop domain. Use incognito/private window. |
| **Steps** | 1. Open the shop homepage in a fresh incognito window. 2. Wait for the page to fully load (DOMContentLoaded + all visible content rendered). 3. Observe the viewport. |
| **Expected Result** | A cookie consent banner is displayed prominently, overlaying or positioned above/below the page content, before the user interacts with anything. The banner is immediately visible without scrolling. |

### TC-51.1.2 — Banner displays before any non-essential scripts execute

| Field | Detail |
|---|---|
| **Test ID** | TC-51.1.2 |
| **Priority** | Critical |
| **Precondition** | Fresh incognito browser. DevTools Network tab open with "Preserve log" enabled. |
| **Steps** | 1. Open DevTools > Network tab. 2. Enable "Preserve log" and clear all existing entries. 3. Navigate to the shop homepage. 4. Before interacting with the consent banner, review all network requests. 5. Filter for known tracking domains: `google-analytics.com`, `analytics.google.com`, `gtag/js`, `connect.facebook.net`, `facebook.com/tr`, `hotjar.com`, `clarity.ms`, `doubleclick.net`, `googleadservices.com`. |
| **Expected Result** | Zero network requests to any tracking or analytics domain appear before the consent banner is interacted with. Only first-party requests and technically necessary third-party resources (e.g., payment SDK, font CDN if classified as necessary) are present. |

### TC-51.1.3 — Banner appears on all entry points, not just homepage

| Field | Detail |
|---|---|
| **Test ID** | TC-51.1.3 |
| **Priority** | High |
| **Precondition** | Fresh incognito browser for each test URL. |
| **Steps** | 1. Open a product detail page URL directly (not via navigation from homepage). 2. Check if the cookie banner appears. 3. Close the browser. Repeat with: a category page, a blog/content page, the contact page, the imprint page, a search results page, the cart page (if accessible without items), and the login/registration page. |
| **Expected Result** | The cookie consent banner appears on every entry point tested. It is not limited to the homepage. |

### TC-51.1.4 — Banner reappears after consent cookie is deleted

| Field | Detail |
|---|---|
| **Test ID** | TC-51.1.4 |
| **Priority** | High |
| **Precondition** | Visit the shop and accept cookies (consent is stored). |
| **Steps** | 1. Open DevTools > Application > Cookies. 2. Identify the consent cookie (common names: `cookieconsent_status`, `CookieConsent`, `cc_cookie`, or CMP-specific). 3. Delete this cookie. 4. Reload the page. |
| **Expected Result** | The cookie consent banner reappears as if it were a first visit. No non-essential tracking fires before re-consent. |

### TC-51.1.5 — Banner reappears after consent expiry period

| Field | Detail |
|---|---|
| **Test ID** | TC-51.1.5 |
| **Priority** | Medium |
| **Precondition** | Consent was previously given. Know the configured consent expiry period (typically 6 or 12 months). |
| **Steps** | 1. Open DevTools > Application > Cookies. 2. Locate the consent cookie. 3. Manually modify its expiry date to a past date (or delete it to simulate expiry). 4. Reload the page. |
| **Expected Result** | The consent banner reappears. Tracking scripts do not fire until new consent is given. The consent cookie is regenerated upon new consent. |

### TC-51.1.6 — Banner does NOT reappear if consent was already given

| Field | Detail |
|---|---|
| **Test ID** | TC-51.1.6 |
| **Priority** | High |
| **Precondition** | Visit the shop and accept all cookies. |
| **Steps** | 1. Close the browser tab. 2. Open a new tab. 3. Navigate to the shop homepage. 4. Navigate to 3-4 internal pages. |
| **Expected Result** | The cookie consent banner does not appear on any subsequent page load. The consent cookie is present and valid. |

### TC-51.1.7 — Banner content includes required legal information

| Field | Detail |
|---|---|
| **Test ID** | TC-51.1.7 |
| **Priority** | High |
| **Precondition** | Fresh incognito browser. |
| **Steps** | 1. Open the homepage. 2. Read the consent banner text. 3. Check for: (a) identification of the data controller (company name), (b) explanation of cookie categories/purposes, (c) link to the full Privacy Policy, (d) option to accept all, reject all (or at minimum accept only necessary), and granular settings. |
| **Expected Result** | All four elements are present. The privacy policy link navigates to the correct page. The text is in the shop's primary language (German for DE market). |

### TC-51.1.8 — Banner does not block entire viewport (dark pattern check)

| Field | Detail |
|---|---|
| **Test ID** | TC-51.1.8 |
| **Priority** | Medium |
| **Precondition** | Fresh incognito browser, mobile and desktop viewports. |
| **Steps** | 1. Open the shop homepage. 2. Before interacting with the banner, attempt to scroll the page content. 3. Check if the imprint/privacy policy links in the footer are accessible without accepting cookies. |
| **Expected Result** | The banner does not cover 100% of the viewport in a way that forces acceptance before any content can be read. Legal pages (imprint, privacy policy) are accessible without consenting to cookies (GDPR requirement: informed consent means the user must be able to read the policy before deciding). |

### TC-51.1.9 — Banner is keyboard-navigable (accessibility)

| Field | Detail |
|---|---|
| **Test ID** | TC-51.1.9 |
| **Priority** | High |
| **Precondition** | Fresh incognito browser, desktop. |
| **Steps** | 1. Open the homepage. 2. Press Tab repeatedly. 3. Verify focus moves to each interactive element in the banner (Accept, Reject/Necessary only, Settings link, Privacy Policy link). 4. Press Enter on each focused button to confirm it activates. 5. Press Escape to check if the banner can be dismissed (and what that means for consent). |
| **Expected Result** | All banner buttons and links are reachable via Tab. Focus order is logical. Focus is visually indicated. Enter activates the focused button. The banner is fully operable without a mouse. |

### TC-51.1.10 — Banner is screen-reader compatible

| Field | Detail |
|---|---|
| **Test ID** | TC-51.1.10 |
| **Priority** | Medium |
| **Precondition** | Screen reader enabled (NVDA on Windows, VoiceOver on macOS/iOS). Fresh incognito browser. |
| **Steps** | 1. Open the homepage with screen reader active. 2. Navigate through the banner content. 3. Verify: (a) banner has an appropriate ARIA role (e.g., `role="dialog"` or `role="alertdialog"`), (b) buttons have accessible labels, (c) banner text is read aloud in logical order. |
| **Expected Result** | The screen reader announces the banner, reads its content, and allows interaction with all buttons. No content is skipped or inaccessible. |

### TC-51.1.11 — Banner is fully functional on mobile devices

| Field | Detail |
|---|---|
| **Test ID** | TC-51.1.11 |
| **Priority** | High |
| **Precondition** | Mobile device or responsive emulation in DevTools (iPhone SE, iPhone 14, Samsung Galaxy S21, iPad). |
| **Steps** | 1. Open the homepage on each device viewport. 2. Check: (a) banner text is not truncated or overflowing, (b) all buttons are visible and tappable (min 44x44px touch target), (c) banner does not cause horizontal scroll, (d) settings/preferences UI (if expandable) is usable on small screens. |
| **Expected Result** | The banner renders correctly on all tested mobile viewports. All interactive elements are usable. No layout issues. |

### TC-51.1.12 — Cookie scanner verification (zero non-essential cookies before consent)

| Field | Detail |
|---|---|
| **Test ID** | TC-51.1.12 |
| **Priority** | Critical |
| **Precondition** | Fresh incognito browser. Access to a cookie scanning tool (CookieYes scanner, Cookiebot, or manual inspection). |
| **Steps** | 1. Open DevTools > Application > Cookies before navigating. 2. Navigate to the homepage. 3. Without interacting with the consent banner, list all cookies set. 4. Cross-reference each cookie against the CMP's cookie declaration. 5. Classify each as necessary or non-essential. |
| **Expected Result** | Only technically necessary cookies are present (session ID, CSRF token, language preference, consent cookie itself in "not yet decided" state). Zero analytics, marketing, or preference cookies exist before consent. |

---

## 51.2 Cookies Set ONLY After Consent (No Pre-Tracking)

### TC-51.2.1 — No analytics cookies/scripts before consent

| Field | Detail |
|---|---|
| **Test ID** | TC-51.2.1 |
| **Priority** | Critical |
| **Precondition** | Fresh incognito browser. DevTools open with Network tab recording. |
| **Steps** | 1. Navigate to the shop homepage. 2. Do NOT interact with the consent banner. 3. In the Network tab, filter for: `google-analytics.com`, `analytics.google.com`, `gtag/js?id=G-`, `matomo`, `plausible`. 4. In the Cookies tab, look for `_ga`, `_gid`, `_gat`, `_pk_*`, or any analytics-related cookies. |
| **Expected Result** | Zero analytics-related network requests. Zero analytics cookies. Google Analytics / GA4 must not fire any measurement protocol hits. |

### TC-51.2.2 — No marketing/advertising scripts before consent

| Field | Detail |
|---|---|
| **Test ID** | TC-51.2.2 |
| **Priority** | Critical |
| **Precondition** | Fresh incognito browser. DevTools open. |
| **Steps** | 1. Navigate to the shop homepage. 2. Do NOT interact with the consent banner. 3. Filter the Network tab for: `connect.facebook.net`, `facebook.com/tr`, `doubleclick.net`, `googleadservices.com`, `ads.linkedin.com`, `analytics.tiktok.com`, `pintrk`, `twitter.com/i/adsct`. 4. Check cookies for `_fbp`, `_fbc`, `IDE`, `test_cookie`, `fr`. |
| **Expected Result** | Zero marketing/advertising network requests. Zero marketing cookies. No pixels fire. |

### TC-51.2.3 — No heatmap/session recording tools before consent

| Field | Detail |
|---|---|
| **Test ID** | TC-51.2.3 |
| **Priority** | High |
| **Precondition** | Fresh incognito browser. DevTools open. |
| **Steps** | 1. Navigate to the homepage. 2. Do NOT interact with the consent banner. 3. Filter for: `hotjar.com`, `static.hotjar.com`, `clarity.ms`, `mouseflow.com`, `luckyorange.com`. 4. Check cookies for `_hj*`, `_clck`, `_clsk`. |
| **Expected Result** | Zero heatmap or session recording scripts load. No associated cookies are set. |

### TC-51.2.4 — Accept "Necessary only" keeps tracking scripts blocked

| Field | Detail |
|---|---|
| **Test ID** | TC-51.2.4 |
| **Priority** | Critical |
| **Precondition** | Fresh incognito browser. DevTools open. |
| **Steps** | 1. Navigate to the homepage. 2. On the consent banner, click "Necessary only" / "Reject all" / decline non-essential cookies. 3. Navigate through 3-4 pages (homepage, category, product, cart). 4. Check the Network tab for any analytics or marketing requests on each page load. 5. Check cookies for any non-essential cookies. |
| **Expected Result** | After rejecting non-essential cookies, zero analytics or marketing scripts fire across all page navigations. Only necessary cookies exist. |

### TC-51.2.5 — Accept "All" activates tracking scripts correctly

| Field | Detail |
|---|---|
| **Test ID** | TC-51.2.5 |
| **Priority** | High |
| **Precondition** | Fresh incognito browser. DevTools open. |
| **Steps** | 1. Navigate to the homepage. 2. Accept all cookies on the consent banner. 3. Check Network tab for analytics and marketing requests. 4. Navigate to a product page and add a product to the cart. 5. Check that e-commerce events (e.g., `view_item`, `add_to_cart`) fire correctly in GA4. |
| **Expected Result** | After accepting all cookies, analytics scripts load and fire correctly. Marketing pixels activate. E-commerce tracking events are recorded with correct parameters. |

### TC-51.2.6 — Consent Mode v2 signals sent to Google Tag Manager

| Field | Detail |
|---|---|
| **Test ID** | TC-51.2.6 |
| **Priority** | High |
| **Precondition** | Google Tag Manager is implemented. Fresh incognito browser. GTM debug mode or Tag Assistant active. |
| **Steps** | 1. Navigate to the homepage without consenting. 2. Open Tag Assistant or GTM Preview mode. 3. Check the consent state: `ad_storage`, `analytics_storage`, `ad_user_data`, `ad_personalization`. 4. Accept all cookies. 5. Re-check the consent state. |
| **Expected Result** | Before consent: all consent signals show `denied`. After accepting all: signals switch to `granted`. Consent Mode v2 is correctly implemented with all four required parameters. |

### TC-51.2.7 — Reject all cookies and verify no third-party cookies remain

| Field | Detail |
|---|---|
| **Test ID** | TC-51.2.7 |
| **Priority** | Critical |
| **Precondition** | Fresh incognito browser. |
| **Steps** | 1. Navigate to the homepage. 2. Click "Reject all" on the consent banner. 3. Browse 5+ pages. 4. Open DevTools > Application > Cookies. 5. List all cookies for all domains. |
| **Expected Result** | Only first-party necessary cookies are present. No third-party cookies from analytics, marketing, or tracking domains exist. The consent cookie itself reflects the "rejected" state. |

### TC-51.2.8 — Consent audit via network waterfall analysis

| Field | Detail |
|---|---|
| **Test ID** | TC-51.2.8 |
| **Priority** | Medium |
| **Precondition** | Fresh incognito browser. |
| **Steps** | 1. Open DevTools > Network tab with "Preserve log" enabled. 2. Navigate to the homepage. 3. Wait 10 seconds without interacting. 4. Export the network log (HAR file). 5. Accept all cookies. 6. Wait 5 seconds. 7. Export the network log again. 8. Compare the two HAR files for third-party requests. |
| **Expected Result** | The first HAR file (pre-consent) contains zero third-party analytics/marketing requests. The second HAR file (post-consent) contains the expected analytics and marketing requests, confirming scripts fire only after consent. |

### TC-51.2.9 — Local storage and IndexedDB audit before consent

| Field | Detail |
|---|---|
| **Test ID** | TC-51.2.9 |
| **Priority** | Medium |
| **Precondition** | Fresh incognito browser. |
| **Steps** | 1. Navigate to the homepage without consenting. 2. Open DevTools > Application > Local Storage, Session Storage, and IndexedDB. 3. List all entries. |
| **Expected Result** | No tracking-related data is stored in Local Storage, Session Storage, or IndexedDB before consent. Only technically necessary entries (e.g., cart data, CSRF tokens) are present. |

---

## 51.3 Cookie Preferences Changeable and Saved

### TC-51.3.1 — Cookie settings link accessible from every page

| Field | Detail |
|---|---|
| **Test ID** | TC-51.3.1 |
| **Priority** | Critical |
| **Precondition** | Consent was previously given (banner dismissed). |
| **Steps** | 1. Navigate to the homepage. 2. Scroll to the footer. 3. Look for a "Cookie Settings" / "Cookie Preferences" / "Cookie-Einstellungen" link. 4. Navigate to 4-5 different pages (category, product, cart, imprint, contact). 5. On each page, verify the cookie settings link is present in the footer. |
| **Expected Result** | A clearly labeled cookie settings link is present in the footer on every page of the shop. The link is visible and clickable. |

### TC-51.3.2 — Cookie settings UI opens without losing page context

| Field | Detail |
|---|---|
| **Test ID** | TC-51.3.2 |
| **Priority** | High |
| **Precondition** | On a product detail page with items in the cart. |
| **Steps** | 1. Click the "Cookie Settings" link in the footer. 2. Observe how the UI opens (modal/overlay vs. new page). 3. If modal: check if the underlying page remains intact. 4. Close the settings UI. 5. Verify the page state (scroll position, cart contents, form data) is unchanged. |
| **Expected Result** | The cookie settings UI opens as a modal/overlay or inline panel without a full page reload. Closing it returns the user to their previous state. No data is lost. |

### TC-51.3.3 — Granular category toggles displayed

| Field | Detail |
|---|---|
| **Test ID** | TC-51.3.3 |
| **Priority** | High |
| **Precondition** | Cookie settings UI is open. |
| **Steps** | 1. Open the cookie settings UI. 2. Verify the following categories are listed with individual toggles: (a) Necessary / Essential (always on, non-toggleable), (b) Statistics / Analytics, (c) Marketing / Advertising, (d) Preferences / Functional (if applicable). 3. Verify each category has a description of its purpose. |
| **Expected Result** | All cookie categories are listed with individual on/off toggles. The "Necessary" category is clearly labeled as required and its toggle is disabled (always on) or has no toggle at all. Each category has a brief, understandable description. |

### TC-51.3.4 — Toggling categories updates consent cookie immediately

| Field | Detail |
|---|---|
| **Test ID** | TC-51.3.4 |
| **Priority** | High |
| **Precondition** | Cookie settings UI is open. DevTools > Application > Cookies is visible. |
| **Steps** | 1. Note the current state of the consent cookie. 2. Toggle "Statistics" from on to off. 3. Click "Save preferences" / "Save". 4. Check the consent cookie value. 5. Reload the page. 6. Re-open cookie settings. |
| **Expected Result** | The consent cookie is updated immediately after saving. The cookie value reflects the changed preference. After reload, the cookie settings UI shows the saved preferences correctly. Analytics scripts do not fire after statistics consent is withdrawn. |

### TC-51.3.5 — Preferences persist across browser sessions

| Field | Detail |
|---|---|
| **Test ID** | TC-51.3.5 |
| **Priority** | High |
| **Precondition** | Custom cookie preferences saved (e.g., Statistics=ON, Marketing=OFF). |
| **Steps** | 1. Close the browser entirely (not just the tab). 2. Reopen the browser. 3. Navigate to the shop. 4. Open cookie settings. 5. Verify the saved preferences. |
| **Expected Result** | Preferences are preserved exactly as configured. The consent banner does not reappear. Marketing scripts remain blocked. Statistics scripts load correctly. |

### TC-51.3.6 — Withdrawing consent immediately stops associated tracking

| Field | Detail |
|---|---|
| **Test ID** | TC-51.3.6 |
| **Priority** | Critical |
| **Precondition** | All cookies were previously accepted. DevTools Network tab is recording. |
| **Steps** | 1. Open cookie settings. 2. Disable "Marketing" and "Statistics" categories. 3. Save preferences. 4. Navigate to 3 new pages. 5. Check the Network tab for analytics and marketing requests on those pages. 6. Check cookies for analytics/marketing cookies. |
| **Expected Result** | After withdrawing consent, zero analytics or marketing requests fire on subsequent page navigations. Previously set analytics/marketing cookies are deleted or not renewed. Consent Mode signals update to `denied`. |

### TC-51.3.7 — Consent record includes timestamp

| Field | Detail |
|---|---|
| **Test ID** | TC-51.3.7 |
| **Priority** | Medium |
| **Precondition** | Consent given. |
| **Steps** | 1. Open DevTools > Application > Cookies. 2. Inspect the consent cookie value (decode if JSON-encoded or base64-encoded). 3. Look for a timestamp field (e.g., `consentDate`, `timestamp`, `datetime`). 4. If the CMP stores consent server-side, check the CMP admin dashboard for consent records. |
| **Expected Result** | The consent record contains a timestamp of when consent was given. This is necessary for GDPR accountability (Art. 7(1)). The timestamp is in a standard format (ISO 8601 or Unix timestamp). |

### TC-51.3.8 — Cookie declaration lists all cookies by name, purpose, provider, and duration

| Field | Detail |
|---|---|
| **Test ID** | TC-51.3.8 |
| **Priority** | High |
| **Precondition** | Cookie settings UI is open with expanded category details. |
| **Steps** | 1. Expand each cookie category. 2. For each cookie listed, verify: (a) cookie name, (b) provider/domain, (c) purpose description, (d) expiry duration. 3. Cross-reference with actual cookies set after accepting all categories. |
| **Expected Result** | Every cookie that is set by the shop is listed in the declaration with name, provider, purpose, and duration. No undeclared cookies exist. The information matches what is actually set in the browser. Required by GDPR Art. 13 and TDDDG. |

### TC-51.3.9 — Cookie settings available in all supported languages

| Field | Detail |
|---|---|
| **Test ID** | TC-51.3.9 |
| **Priority** | Medium |
| **Precondition** | Shop supports multiple languages (e.g., DE, EN). |
| **Steps** | 1. Switch the shop language to German. Open cookie settings. Verify text is in German. 2. Switch to English. Open cookie settings. Verify text is in English. 3. Repeat for all supported languages. |
| **Expected Result** | Cookie settings UI, category names, descriptions, and cookie declarations are properly translated in all supported languages. No untranslated strings or placeholder text. |

### TC-51.3.10 — Re-accepting cookies after prior rejection activates scripts

| Field | Detail |
|---|---|
| **Test ID** | TC-51.3.10 |
| **Priority** | Medium |
| **Precondition** | All non-essential cookies were previously rejected. DevTools Network tab recording. |
| **Steps** | 1. Open cookie settings. 2. Toggle "Statistics" and "Marketing" to ON. 3. Save preferences. 4. Navigate to a new page. 5. Check Network tab for analytics/marketing requests. |
| **Expected Result** | After re-accepting, analytics and marketing scripts fire correctly on the next page load. Corresponding cookies are set. Consent Mode signals update to `granted`. |

---

## 51.4 Legal Notice / Imprint Complete and Accessible

### TC-51.4.1 — Imprint reachable within two clicks from any page

| Field | Detail |
|---|---|
| **Test ID** | TC-51.4.1 |
| **Priority** | Critical |
| **Precondition** | Shop fully loaded. |
| **Steps** | 1. Navigate to the homepage. Verify the imprint link is in the footer (1 click). 2. Navigate to a product page. Check for the imprint link (1 click from footer). 3. Navigate to a deep category page. Check. 4. Navigate to the checkout page. Check. 5. Navigate to the customer account area. Check. |
| **Expected Result** | The imprint is accessible within a maximum of two clicks from every page. Per DDG section 5, it must be "easily recognizable, directly accessible, and constantly available." A footer link that is always visible satisfies this. |

### TC-51.4.2 — Imprint link present in footer on all pages

| Field | Detail |
|---|---|
| **Test ID** | TC-51.4.2 |
| **Priority** | Critical |
| **Precondition** | None. |
| **Steps** | 1. Visit 10+ different page types: homepage, category, product, cart, checkout, login, registration, contact, blog, FAQ, search results. 2. On each page, scroll to the footer. 3. Verify an "Impressum" / "Legal Notice" / "Imprint" link is present and visible. |
| **Expected Result** | The imprint link is present in the footer on every page without exception. It is not hidden behind a dropdown or accordion. |

### TC-51.4.3 — Imprint contains all mandatory information (DDG section 5)

| Field | Detail |
|---|---|
| **Test ID** | TC-51.4.3 |
| **Priority** | Critical |
| **Precondition** | Imprint page is open. |
| **Steps** | Verify the following information is present: 1. Full company name including legal form (e.g., "Muster GmbH"). 2. Registered postal address (street, postal code, city — no PO box). 3. Telephone number. 4. Email address. 5. Managing director / authorized representative name(s). 6. Commercial register entry: court, registration number (e.g., "Amtsgericht Koeln, HRB 12345"). 7. VAT identification number (USt-IdNr.) per section 27a UStG. 8. (If applicable) Professional regulatory information, professional body, trade license. |
| **Expected Result** | All mandatory fields are present, accurate, and up to date. No placeholder text. No missing fields. |

### TC-51.4.4 — Imprint contains responsible person for editorial content

| Field | Detail |
|---|---|
| **Test ID** | TC-51.4.4 |
| **Priority** | Medium |
| **Precondition** | Shop publishes editorial content (blog, articles, guides). |
| **Steps** | 1. Open the imprint page. 2. Look for "Verantwortlich fuer den Inhalt nach section 18 Abs. 2 MStV" or equivalent. 3. Verify a named individual is listed with address. |
| **Expected Result** | If the shop has editorial content, the responsible person per MStV section 18(2) is named. If no editorial content exists, this field may be omitted. |

### TC-51.4.5 — Imprint rendered as HTML text (not image)

| Field | Detail |
|---|---|
| **Test ID** | TC-51.4.5 |
| **Priority** | High |
| **Precondition** | Imprint page is open. |
| **Steps** | 1. Right-click on the imprint text. 2. Verify "Select All" / text selection works. 3. Verify text is not rendered as an image (`<img>` tag). 4. Use "View Page Source" to confirm the content is in HTML text nodes. 5. Use a screen reader to verify the text is read aloud correctly. |
| **Expected Result** | All imprint content is rendered as selectable HTML text. It is accessible to screen readers and search engine crawlers. No content is embedded as images. |

### TC-51.4.6 — Imprint not blocked by robots.txt or noindex

| Field | Detail |
|---|---|
| **Test ID** | TC-51.4.6 |
| **Priority** | High |
| **Precondition** | Access to the shop's robots.txt and page source. |
| **Steps** | 1. Navigate to `[shop-domain]/robots.txt`. 2. Check for any Disallow rules that match the imprint URL. 3. Open the imprint page source. 4. Check the `<meta name="robots">` tag. 5. Check the HTTP response headers for `X-Robots-Tag`. |
| **Expected Result** | The imprint page is not disallowed in robots.txt. No `noindex` directive is set in meta tags or HTTP headers. The page is crawlable and indexable. |

### TC-51.4.7 — Email address is a clickable mailto link on mobile

| Field | Detail |
|---|---|
| **Test ID** | TC-51.4.7 |
| **Priority** | Medium |
| **Precondition** | Mobile device or mobile emulation. |
| **Steps** | 1. Open the imprint page on a mobile device. 2. Tap the email address. 3. Verify the device's email client opens with the correct recipient pre-filled. |
| **Expected Result** | The email address is wrapped in a `mailto:` link. Tapping it opens the default email client with the correct address. |

### TC-51.4.8 — Imprint is present in all language versions

| Field | Detail |
|---|---|
| **Test ID** | TC-51.4.8 |
| **Priority** | Medium |
| **Precondition** | Shop supports multiple languages. |
| **Steps** | 1. Switch to each supported language. 2. Navigate to the imprint page. 3. Verify the content is present and accurate in each language version. 4. Verify legal entity details (address, registration) remain the same across languages (they are factual, not translated). |
| **Expected Result** | The imprint exists and is accessible in every language version of the shop. Factual information is consistent. Surrounding text (labels, headings) is translated. |

### TC-51.4.9 — Imprint page loads within acceptable performance thresholds

| Field | Detail |
|---|---|
| **Test ID** | TC-51.4.9 |
| **Priority** | Low |
| **Precondition** | None. |
| **Steps** | 1. Open DevTools > Network tab. 2. Navigate to the imprint page. 3. Measure total load time and Time to First Contentful Paint. 4. Verify no broken resources block rendering. |
| **Expected Result** | The imprint page loads within 3 seconds on a standard connection. All text is rendered promptly. No broken CSS/JS dependencies delay content visibility. |

---

## 51.5 Terms and Conditions Linked and Acceptable in Checkout

### TC-51.5.1 — T&C link in checkout opens the full document

| Field | Detail |
|---|---|
| **Test ID** | TC-51.5.1 |
| **Priority** | Critical |
| **Precondition** | Items in cart, user is on the checkout order summary/review step. |
| **Steps** | 1. Locate the T&C link near the T&C checkbox. 2. Click/tap the link. 3. Verify the full T&C page or document loads. 4. Check it is not a 404, blank page, or error page. |
| **Expected Result** | The T&C link navigates to the complete, current Terms and Conditions document. The page loads correctly with all content visible. |

### TC-51.5.2 — T&C link opens in new tab or modal (checkout session preserved)

| Field | Detail |
|---|---|
| **Test ID** | TC-51.5.2 |
| **Priority** | High |
| **Precondition** | User is on the checkout page with filled forms and items in cart. |
| **Steps** | 1. Click the T&C link. 2. Observe whether it opens in a new tab/window or in a modal overlay. 3. Close the T&C tab/modal. 4. Verify the checkout page is intact: form data preserved, cart contents unchanged, no session expiry. |
| **Expected Result** | The T&C document opens in a new tab or modal, preserving the checkout session. Returning to the checkout does not require re-entering any data. |

### TC-51.5.3 — T&C checkbox is NOT pre-checked

| Field | Detail |
|---|---|
| **Test ID** | TC-51.5.3 |
| **Priority** | Critical |
| **Precondition** | User is on the checkout review step. |
| **Steps** | 1. Arrive at the order summary step with the T&C checkbox visible. 2. Verify the checkbox is unchecked by default. 3. Check the page source or inspect the element to confirm no `checked` attribute is set. |
| **Expected Result** | The T&C checkbox is unchecked by default. German law (BGH case law, UKlaG) requires active opt-in. Pre-checking is non-compliant. |

### TC-51.5.4 — Order cannot be placed without accepting T&C

| Field | Detail |
|---|---|
| **Test ID** | TC-51.5.4 |
| **Priority** | Critical |
| **Precondition** | Items in cart, all checkout fields filled, T&C checkbox unchecked. |
| **Steps** | 1. Leave the T&C checkbox unchecked. 2. Click the "Place Order" button. 3. Observe the result. |
| **Expected Result** | The order is NOT submitted. A clear, user-friendly error message is displayed indicating that the Terms and Conditions must be accepted before the order can be placed. The button is either disabled or server-side validation blocks the submission. |

### TC-51.5.5 — Error message displayed when T&C not accepted

| Field | Detail |
|---|---|
| **Test ID** | TC-51.5.5 |
| **Priority** | High |
| **Precondition** | Checkout review step, T&C unchecked. |
| **Steps** | 1. Attempt to place the order without checking T&C. 2. Read the validation error message. 3. Verify it appears near the checkbox (not only at the top of the page). 4. Verify the message is in the shop's language (German for DE). |
| **Expected Result** | A clear validation error is displayed near the T&C checkbox. The message explicitly states that accepting the Terms and Conditions is required. The error is in the correct language. |

### TC-51.5.6 — T&C document contains all required legal sections

| Field | Detail |
|---|---|
| **Test ID** | TC-51.5.6 |
| **Priority** | High |
| **Precondition** | T&C page is open. |
| **Steps** | Verify the document contains sections covering: 1. Scope of application. 2. Conclusion of contract. 3. Prices and payment terms. 4. Delivery and shipping. 5. Right of withdrawal / cancellation. 6. Warranty / liability. 7. Applicable law and jurisdiction. 8. Data protection reference. |
| **Expected Result** | All legally required sections are present. The document is complete and current. |

### TC-51.5.7 — T&C are versioned with a current date

| Field | Detail |
|---|---|
| **Test ID** | TC-51.5.7 |
| **Priority** | Medium |
| **Precondition** | T&C page is open. |
| **Steps** | 1. Look for a "Version" or "Stand" (as of) date on the T&C page. 2. Verify the date is recent and reflects the current version. 3. Check if previous versions are accessible (link to archive or version history). |
| **Expected Result** | The T&C document shows a clear version date. For contracts concluded under earlier versions, prior T&C versions should be accessible per good practice. |

### TC-51.5.8 — T&C available as PDF download

| Field | Detail |
|---|---|
| **Test ID** | TC-51.5.8 |
| **Priority** | High |
| **Precondition** | T&C page is open. |
| **Steps** | 1. Look for a PDF download link on the T&C page. 2. Click the download link. 3. Verify the PDF downloads successfully. 4. Open the PDF and verify content matches the online version. 5. Check for correct rendering of German characters (ae, oe, ue, ss). |
| **Expected Result** | The T&C are available as a downloadable PDF. The file is not corrupt or blank. Content matches the online version. German characters render correctly. Required by section 312f BGB for distance contracts. |

### TC-51.5.9 — T&C included in order confirmation email

| Field | Detail |
|---|---|
| **Test ID** | TC-51.5.9 |
| **Priority** | High |
| **Precondition** | Complete a test order. |
| **Steps** | 1. Place an order successfully. 2. Check the order confirmation email. 3. Verify the T&C are either: (a) attached as a PDF, or (b) linked with a permanent URL to the version valid at time of purchase. |
| **Expected Result** | The order confirmation email includes the T&C (as PDF attachment or permanent link). The customer can save them for their records as required by German distance selling law. |

### TC-51.5.10 — T&C do not contain void clauses under German AGB law

| Field | Detail |
|---|---|
| **Test ID** | TC-51.5.10 |
| **Priority** | High |
| **Precondition** | T&C document is available for legal review. |
| **Steps** | 1. Review T&C for common void clauses under sections 305-310 BGB: (a) exclusion of statutory warranty rights for consumers, (b) unreasonable limitation of liability, (c) unilateral right to change prices after contract conclusion, (d) excessive contractual penalties, (e) shifting burden of proof to consumer. 2. Flag any potentially non-compliant clauses. |
| **Expected Result** | No clauses that are void under German AGB law (sections 305-310 BGB) are present. If flagged clauses exist, they must be reviewed by legal counsel before go-live. |

### TC-51.5.11 — T&C checkbox works across all checkout paths

| Field | Detail |
|---|---|
| **Test ID** | TC-51.5.11 |
| **Priority** | High |
| **Precondition** | Items in cart. |
| **Steps** | 1. Test T&C acceptance in standard checkout (logged-in user). 2. Test in guest checkout. 3. Test in express checkout flows (PayPal Express, Apple Pay, Google Pay) — verify the T&C are acknowledged before order completion. |
| **Expected Result** | T&C acceptance is enforced in every checkout path. Express checkout flows either display the T&C acceptance inline or ensure the user has acknowledged them before payment authorization. |

---

## 51.6 Cancellation Policy Correctly Displayed

### TC-51.6.1 — Cancellation policy displayed in checkout before order placement

| Field | Detail |
|---|---|
| **Test ID** | TC-51.6.1 |
| **Priority** | Critical |
| **Precondition** | Items in cart, user is on the checkout order review step. |
| **Steps** | 1. Navigate to the final checkout step (order review/summary). 2. Look for a cancellation policy link or text. 3. Verify it is displayed prominently (not only in the footer). |
| **Expected Result** | The cancellation policy is linked or displayed in the checkout flow before the order button, not buried in the footer only. The user is informed of their withdrawal rights before committing to the purchase. |

### TC-51.6.2 — Cancellation policy link works (no 404)

| Field | Detail |
|---|---|
| **Test ID** | TC-51.6.2 |
| **Priority** | Critical |
| **Precondition** | Checkout page is displayed. |
| **Steps** | 1. Click the cancellation policy link in the checkout. 2. Verify the full cancellation policy document loads. 3. Also check the cancellation policy link from the footer on non-checkout pages. |
| **Expected Result** | The link navigates to a complete cancellation policy page. No 404 errors. No blank pages. Content is fully rendered. |

### TC-51.6.3 — Cancellation policy checkbox (if present) is NOT pre-checked

| Field | Detail |
|---|---|
| **Test ID** | TC-51.6.3 |
| **Priority** | Critical |
| **Precondition** | Checkout page with cancellation policy checkbox visible. |
| **Steps** | 1. Arrive at the order review step. 2. Inspect the cancellation policy checkbox. 3. Verify it is unchecked by default. |
| **Expected Result** | If a cancellation policy checkbox exists, it is unchecked by default. Active opt-in is required. |

### TC-51.6.4 — Policy complies with EU model cancellation notice

| Field | Detail |
|---|---|
| **Test ID** | TC-51.6.4 |
| **Priority** | Critical |
| **Precondition** | Cancellation policy document is open. |
| **Steps** | Verify the document contains per BGB-InfoV Anlage 1: 1. Statement of the 14-day withdrawal period. 2. The period starts from the date of delivery (not order date) per section 355 BGB. 3. Instructions on how to exercise the right (written declaration to the trader). 4. Name and full address of the trader to whom the withdrawal must be sent. 5. Reference to the model withdrawal form. 6. Consequences of withdrawal (refund timing, return of goods, return shipping cost allocation). |
| **Expected Result** | All mandatory elements of the EU model cancellation notice are present and correctly stated. The 14-day period is calculated from delivery, not order date. |

### TC-51.6.5 — Exceptions to withdrawal right are documented

| Field | Detail |
|---|---|
| **Test ID** | TC-51.6.5 |
| **Priority** | High |
| **Precondition** | Cancellation policy document is open. Shop sells products that may be exempt from withdrawal. |
| **Steps** | 1. Review the exceptions listed in the cancellation policy. 2. Verify they match section 312g Abs. 2 BGB: (a) custom/personalized products, (b) sealed goods not suitable for return due to health/hygiene if seal is broken, (c) perishable goods, (d) digital content already downloaded (with consent and acknowledgment of loss of withdrawal right), (e) sealed audio/video/software if seal is broken. 3. Cross-reference with the shop's actual product catalog. |
| **Expected Result** | All applicable exceptions are correctly listed. Only legally valid exceptions are cited. Products that fall under exceptions are clearly identified in the shop (e.g., "This product is custom-made. Withdrawal right does not apply."). |

### TC-51.6.6 — Model withdrawal form is included or linked

| Field | Detail |
|---|---|
| **Test ID** | TC-51.6.6 |
| **Priority** | High |
| **Precondition** | Cancellation policy page is open. |
| **Steps** | 1. Scroll through the cancellation policy. 2. Look for the model withdrawal form (Muster-Widerrufsformular). 3. Verify it is either embedded in the page or linked as a separate document/PDF. |
| **Expected Result** | The model withdrawal form is present per section 356 BGB. It can be accessed from the cancellation policy page. |

### TC-51.6.7 — Cancellation policy included in order confirmation email

| Field | Detail |
|---|---|
| **Test ID** | TC-51.6.7 |
| **Priority** | High |
| **Precondition** | Complete a test order. |
| **Steps** | 1. Place an order. 2. Check the confirmation email. 3. Verify the cancellation policy is included (inline text, PDF attachment, or permanent link). 4. Verify the model withdrawal form is also included. |
| **Expected Result** | Both the cancellation policy and the model withdrawal form are provided in the order confirmation email. The customer receives all required withdrawal information at the point of contract conclusion. |

### TC-51.6.8 — Cancellation policy is versioned and dated

| Field | Detail |
|---|---|
| **Test ID** | TC-51.6.8 |
| **Priority** | Medium |
| **Precondition** | Cancellation policy page is open. |
| **Steps** | 1. Check for a version date on the cancellation policy. 2. Verify the date is current. |
| **Expected Result** | The cancellation policy displays a clear version date. The date reflects the most recent update. |

### TC-51.6.9 — Consequences of withdrawal clearly stated

| Field | Detail |
|---|---|
| **Test ID** | TC-51.6.9 |
| **Priority** | High |
| **Precondition** | Cancellation policy document is open. |
| **Steps** | 1. Locate the section on consequences of withdrawal. 2. Verify it states: (a) refund will be issued within 14 days of receiving the withdrawal notice, (b) the original payment method will be used for the refund, (c) the consumer must return goods within 14 days, (d) who bears the return shipping costs. |
| **Expected Result** | All consequences are clearly documented per sections 355-357 BGB. No ambiguity about refund timeline, method, or return shipping cost responsibility. |

### TC-51.6.10 — Cancellation policy available as PDF

| Field | Detail |
|---|---|
| **Test ID** | TC-51.6.10 |
| **Priority** | Medium |
| **Precondition** | Cancellation policy page is open. |
| **Steps** | 1. Look for a PDF download link. 2. Download the PDF. 3. Verify it opens correctly and matches the online version. 4. Verify German characters render correctly. |
| **Expected Result** | The cancellation policy is downloadable as a correctly formatted PDF. Characters are correct. Content matches the web version. |

---

## 51.7 Privacy Policy Linked and Complete

### TC-51.7.1 — Privacy policy link in footer on every page

| Field | Detail |
|---|---|
| **Test ID** | TC-51.7.1 |
| **Priority** | Critical |
| **Precondition** | None. |
| **Steps** | 1. Visit 10+ different page types (homepage, category, product, cart, checkout, login, registration, contact, blog, FAQ, search results). 2. On each page, scroll to the footer. 3. Verify a "Datenschutzerklaerung" / "Privacy Policy" link is present. |
| **Expected Result** | The privacy policy link is present in the footer on every page without exception. |

### TC-51.7.2 — Privacy policy linked on all data collection forms

| Field | Detail |
|---|---|
| **Test ID** | TC-51.7.2 |
| **Priority** | Critical |
| **Precondition** | None. |
| **Steps** | 1. Navigate to the registration form. Verify a privacy policy link is present near/below the form. 2. Navigate to the newsletter subscription form. Verify. 3. Navigate to the contact form. Verify. 4. Navigate to the checkout (address entry). Verify. 5. Navigate to the review submission form. Verify. |
| **Expected Result** | Every form that collects personal data includes a link to the privacy policy at or near the point of data collection, as required by GDPR Art. 13. |

### TC-51.7.3 — Privacy policy linked in the cookie consent banner

| Field | Detail |
|---|---|
| **Test ID** | TC-51.7.3 |
| **Priority** | High |
| **Precondition** | Fresh incognito browser. |
| **Steps** | 1. Open the shop. 2. On the cookie consent banner, look for a privacy policy link. 3. Click it. 4. Verify it navigates to the correct privacy policy page. |
| **Expected Result** | The cookie consent banner contains a working link to the privacy policy. Users can read the policy before deciding on consent. |

### TC-51.7.4 — Privacy policy written in plain language and in German

| Field | Detail |
|---|---|
| **Test ID** | TC-51.7.4 |
| **Priority** | High |
| **Precondition** | Privacy policy page is open. |
| **Steps** | 1. Read through the privacy policy. 2. Assess readability: is it written in plain, understandable language (not solely dense legalese)? 3. For a German-market shop, verify the primary version is in German. |
| **Expected Result** | The privacy policy is understandable to an average consumer. It is available in German for German-speaking users. GDPR Art. 12 requires information to be provided in "concise, transparent, intelligible and easily accessible form, using clear and plain language." |

### TC-51.7.5 — Privacy policy names the data controller and DPO

| Field | Detail |
|---|---|
| **Test ID** | TC-51.7.5 |
| **Priority** | Critical |
| **Precondition** | Privacy policy page is open. |
| **Steps** | 1. Find the "Data Controller" / "Verantwortlicher" section. 2. Verify: full company name, address, contact details. 3. Check if a Data Protection Officer (DPO) is named (required if the company employs 20+ people regularly involved in automated data processing, or processes special categories of data). 4. If a DPO is named, verify contact details are provided. |
| **Expected Result** | The data controller is clearly identified with full contact details. If a DPO is required and appointed, their contact details are listed. Required by GDPR Art. 13(1)(a) and (b). |

### TC-51.7.6 — All data processing activities listed with legal basis

| Field | Detail |
|---|---|
| **Test ID** | TC-51.7.6 |
| **Priority** | Critical |
| **Precondition** | Privacy policy page is open. |
| **Steps** | For each data processing activity, verify the following is documented: 1. Purpose (why the data is collected). 2. Legal basis (GDPR Art. 6 reference: consent, contract performance, legitimate interest, legal obligation). 3. Categories of personal data collected. 4. Retention period or criteria for determining retention. 5. Third-party recipients (if data is shared). Check coverage of at least: (a) order processing, (b) customer account, (c) newsletter, (d) cookies/tracking, (e) payment processing, (f) shipping/logistics, (g) customer support. |
| **Expected Result** | Every data processing activity is documented with all five required elements. No processing activity is omitted. Legal bases are correctly cited (e.g., contract performance for order processing, consent for newsletter, legitimate interest for fraud prevention). |

### TC-51.7.7 — Third-party integrations disclosed

| Field | Detail |
|---|---|
| **Test ID** | TC-51.7.7 |
| **Priority** | Critical |
| **Precondition** | Privacy policy page is open. Knowledge of all third-party services used by the shop. |
| **Steps** | 1. List all third-party services the shop uses (from technical audit or CMP declaration): Google Analytics, Meta Pixel, Hotjar, Stripe, PayPal, Klarna, DHL, DPD, Trustpilot, Mailchimp, etc. 2. For each service, verify it is mentioned in the privacy policy. 3. Verify each entry includes: service name, provider, purpose, link to provider's privacy policy. |
| **Expected Result** | Every third-party service that processes personal data is disclosed in the privacy policy. No undisclosed integrations. Each disclosure includes provider details and a link to their privacy policy. |

### TC-51.7.8 — Cross-border data transfers documented

| Field | Detail |
|---|---|
| **Test ID** | TC-51.7.8 |
| **Priority** | High |
| **Precondition** | Privacy policy page is open. Shop uses US-based services (Google, Meta, Stripe, etc.). |
| **Steps** | 1. Search the privacy policy for references to data transfers outside the EU/EEA. 2. Verify the policy documents: (a) which services transfer data outside the EU, (b) the destination countries, (c) the safeguards used (Standard Contractual Clauses, adequacy decisions, EU-US Data Privacy Framework). |
| **Expected Result** | Cross-border data transfers are documented with the safeguards in place. If US services are used, the EU-US Data Privacy Framework certification or SCCs are referenced. Required by GDPR Art. 13(1)(f) and Art. 44-49. |

### TC-51.7.9 — Data subject rights documented

| Field | Detail |
|---|---|
| **Test ID** | TC-51.7.9 |
| **Priority** | Critical |
| **Precondition** | Privacy policy page is open. |
| **Steps** | 1. Locate the "Your Rights" / "Ihre Rechte" section. 2. Verify the following rights are listed: (a) Right of access (Art. 15), (b) Right to rectification (Art. 16), (c) Right to erasure (Art. 17), (d) Right to restriction of processing (Art. 18), (e) Right to data portability (Art. 20), (f) Right to object (Art. 21), (g) Right to withdraw consent (Art. 7(3)), (h) Right to lodge a complaint with a supervisory authority (Art. 77). 3. Verify the supervisory authority is named with address. |
| **Expected Result** | All eight data subject rights are listed and explained. The competent supervisory authority (Datenschutzbehoerde) is identified by name and address. Contact details for exercising rights are provided. |

### TC-51.7.10 — Privacy policy is current and not blocked from indexing

| Field | Detail |
|---|---|
| **Test ID** | TC-51.7.10 |
| **Priority** | Medium |
| **Precondition** | Privacy policy page is open. |
| **Steps** | 1. Check the version date on the privacy policy. 2. Verify it reflects any recent changes (new integrations, updated legal basis). 3. Check robots.txt for Disallow rules affecting the privacy policy URL. 4. Check for `noindex` meta tags or HTTP headers. |
| **Expected Result** | The privacy policy has a current version date. It is not blocked from search engine indexing (it should be accessible to anyone). |

### TC-51.7.11 — Privacy policy covers cookie consent mechanism

| Field | Detail |
|---|---|
| **Test ID** | TC-51.7.11 |
| **Priority** | High |
| **Precondition** | Privacy policy page is open. |
| **Steps** | 1. Search for a section about cookies and tracking. 2. Verify it references the cookie consent mechanism (CMP name, how to manage preferences). 3. Verify it explains the difference between necessary and non-essential cookies. 4. Verify it references the TDDDG (German Telecommunications-Telemedia Data Protection Act). |
| **Expected Result** | The privacy policy includes a comprehensive section on cookies that references the consent mechanism, cookie categories, and the TDDDG as the applicable German law for cookie consent. |

### TC-51.7.12 — Privacy policy covers newsletter with double opt-in reference

| Field | Detail |
|---|---|
| **Test ID** | TC-51.7.12 |
| **Priority** | High |
| **Precondition** | Privacy policy page is open. Shop offers newsletter subscription. |
| **Steps** | 1. Find the newsletter section in the privacy policy. 2. Verify it mentions: (a) double opt-in process, (b) email service provider (e.g., Mailchimp, CleverReach), (c) purpose of data processing, (d) legal basis (consent, Art. 6(1)(a)), (e) how to unsubscribe. |
| **Expected Result** | The newsletter processing is fully documented including the double opt-in mechanism and the email service provider. Unsubscribe process is described. |

---

## 51.8 Age Verification (If Age-Relevant Products)

### TC-51.8.1 — Age gate displayed before age-restricted products

| Field | Detail |
|---|---|
| **Test ID** | TC-51.8.1 |
| **Priority** | Critical |
| **Precondition** | Shop sells age-restricted products (alcohol, tobacco, knives, adult content). |
| **Steps** | 1. Navigate to a category or product page containing age-restricted items. 2. Observe whether an age verification gate is displayed before the product content is shown. |
| **Expected Result** | An age verification mechanism (date-of-birth entry, age declaration, or full AVS integration) is displayed before the user can view or purchase age-restricted products. |

### TC-51.8.2 — Age gate shown at product/category level, not only at checkout

| Field | Detail |
|---|---|
| **Test ID** | TC-51.8.2 |
| **Priority** | High |
| **Precondition** | Age-restricted products exist in the catalog. |
| **Steps** | 1. Navigate directly to an age-restricted product page via URL. 2. Check if the age gate appears before the product details are visible. 3. Navigate to an age-restricted category page. 4. Check if the age gate appears before the product listing is shown. |
| **Expected Result** | The age gate is displayed at the point of access (product or category level), not deferred to checkout only. The user cannot view restricted product details without passing the age check. |

### TC-51.8.3 — Underage users denied access with clear message

| Field | Detail |
|---|---|
| **Test ID** | TC-51.8.3 |
| **Priority** | Critical |
| **Precondition** | Age gate is displayed. |
| **Steps** | 1. Enter a date of birth that makes the user under 18 (or under the applicable age threshold). 2. Submit the age verification. |
| **Expected Result** | Access to the restricted product is denied. A clear, non-dismissible message explains why access is restricted (e.g., "You must be 18 or older to view this product"). The user cannot proceed past the gate. |

### TC-51.8.4 — Age gate cannot be bypassed via URL manipulation

| Field | Detail |
|---|---|
| **Test ID** | TC-51.8.4 |
| **Priority** | Critical |
| **Precondition** | Age gate is active. User has NOT passed verification. |
| **Steps** | 1. Copy the direct URL of an age-restricted product. 2. Open a new incognito window. 3. Paste the URL directly. 4. Try appending URL parameters (e.g., `?age_verified=true`, `?skip_age=1`). 5. Try navigating via browser back/forward buttons after being denied. |
| **Expected Result** | The age gate cannot be bypassed by any URL manipulation, direct navigation, or browser history navigation. Server-side verification is enforced. |

### TC-51.8.5 — Age verification is server-side, not solely cookie-based

| Field | Detail |
|---|---|
| **Test ID** | TC-51.8.5 |
| **Priority** | High |
| **Precondition** | User passed age verification. |
| **Steps** | 1. Pass the age verification. 2. Open DevTools > Application > Cookies. 3. Delete all cookies related to age verification. 4. Reload the age-restricted product page. |
| **Expected Result** | After deleting cookies, the age gate reappears. This confirms the shop properly handles the case. If server-side session stores the verification, this is acceptable. The key point is that age verification should not rely solely on a client-side cookie that can be trivially deleted. |

### TC-51.8.6 — Non-restricted products are NOT blocked by age gate

| Field | Detail |
|---|---|
| **Test ID** | TC-51.8.6 |
| **Priority** | High |
| **Precondition** | Shop has both restricted and non-restricted products. |
| **Steps** | 1. Navigate to a non-restricted product page. 2. Verify no age gate is displayed. 3. Add the product to cart and proceed through checkout. 4. Verify no age verification is required at any step. |
| **Expected Result** | Non-restricted products are fully accessible without any age verification prompt. The age gate is targeted only at restricted products. |

### TC-51.8.7 — Checkout prevents completion without passed age check for restricted items

| Field | Detail |
|---|---|
| **Test ID** | TC-51.8.7 |
| **Priority** | Critical |
| **Precondition** | Age-restricted product added to cart (somehow bypassing the product-level gate, e.g., via API manipulation). |
| **Steps** | 1. Add an age-restricted product to the cart (if possible without passing age gate, e.g., by direct API call). 2. Proceed to checkout. 3. Attempt to place the order. |
| **Expected Result** | The checkout process includes a secondary age verification check. Orders containing age-restricted products cannot be completed without verified age. |

### TC-51.8.8 — Delivery instructions for age-restricted products

| Field | Detail |
|---|---|
| **Test ID** | TC-51.8.8 |
| **Priority** | Medium |
| **Precondition** | Order placed for age-restricted product. |
| **Steps** | 1. Complete an order for an age-restricted product. 2. Check the order confirmation email for delivery instructions. 3. If integrated with shipping provider, check the shipping label data for age verification flags. |
| **Expected Result** | The order confirmation includes information about delivery requirements (e.g., "Recipient must be 18+ and may be asked for ID upon delivery"). Shipping label data includes the appropriate age verification flag for the carrier. |

### TC-51.8.9 — Robust AVS for high-risk products (JuSchG section 9)

| Field | Detail |
|---|---|
| **Test ID** | TC-51.8.9 |
| **Priority** | High |
| **Precondition** | Shop sells products requiring strict age verification (e.g., certain weapons, adult content per JuSchG/JMStV). |
| **Steps** | 1. Verify the age verification method: is it a simple self-declaration (date of birth) or a robust AVS (SCHUFA, Post-Ident, IDnow, video identification)? 2. For high-risk products, verify a robust AVS is integrated. 3. Test the AVS flow end-to-end. |
| **Expected Result** | For products requiring strict age verification under JuSchG section 9 or JMStV section 4, a robust online age verification system (not just a date-of-birth field) is in place and functional. |

---

## 51.9 Price Indication Regulation: Base Price, Total Price

### TC-51.9.1 — Base price displayed for products sold by unit of measure

| Field | Detail |
|---|---|
| **Test ID** | TC-51.9.1 |
| **Priority** | Critical |
| **Precondition** | Shop sells products by weight, volume, length, or area (e.g., food, beverages, cleaning products, fabric). |
| **Steps** | 1. Navigate to a product sold by weight/volume. 2. Verify the base price (Grundpreis) is displayed next to the selling price. 3. Verify the format: e.g., "1 kg = 12,50 EUR" or "100 ml = 3,99 EUR". 4. Verify the unit matches the product type (per PAngV section 4). |
| **Expected Result** | The base price is displayed in the legally required format. The unit of measure is correct. The price is accurate based on the product's net weight/volume and selling price. |

### TC-51.9.2 — Base price displayed on product list/category page

| Field | Detail |
|---|---|
| **Test ID** | TC-51.9.2 |
| **Priority** | Critical |
| **Precondition** | Products requiring base price exist in the catalog. |
| **Steps** | 1. Navigate to a category page listing products that require base prices. 2. Verify each product tile/card shows the base price alongside the selling price. |
| **Expected Result** | Base prices are visible on the category/product list page, not only on the product detail page. PAngV requires the base price wherever the selling price is shown. |

### TC-51.9.3 — Base price displayed in the cart

| Field | Detail |
|---|---|
| **Test ID** | TC-51.9.3 |
| **Priority** | High |
| **Precondition** | Product requiring base price is in the cart. |
| **Steps** | 1. Add a product requiring base price to the cart. 2. View the cart page. 3. Verify the base price is shown next to each applicable line item. |
| **Expected Result** | The base price is displayed for applicable products in the cart. |

### TC-51.9.4 — Base price updates dynamically on variant change

| Field | Detail |
|---|---|
| **Test ID** | TC-51.9.4 |
| **Priority** | High |
| **Precondition** | Product with multiple size/weight variants exists (e.g., 250g, 500g, 1kg). |
| **Steps** | 1. Open a product with weight variants. 2. Note the base price for the default variant. 3. Switch to a different variant (e.g., from 250g to 1kg). 4. Verify the base price updates to reflect the new variant's price per unit. |
| **Expected Result** | The base price recalculates dynamically when a different variant is selected. The calculation is accurate. |

### TC-51.9.5 — Total price includes VAT with "inkl. MwSt." notice

| Field | Detail |
|---|---|
| **Test ID** | TC-51.9.5 |
| **Priority** | Critical |
| **Precondition** | Any product page. |
| **Steps** | 1. Navigate to a product page. 2. Verify the displayed price is the total price (Gesamtpreis) including VAT. 3. Verify a notice reading "inkl. MwSt." (incl. VAT) or equivalent is displayed near the price. 4. Verify this notice is present per PAngV section 3. |
| **Expected Result** | The total price including VAT is shown. The "inkl. MwSt." notice is clearly visible near the price on every product. |

### TC-51.9.6 — Shipping cost notice displayed near the price

| Field | Detail |
|---|---|
| **Test ID** | TC-51.9.6 |
| **Priority** | Critical |
| **Precondition** | Any product page. |
| **Steps** | 1. Navigate to a product page. 2. Near the price, look for a shipping cost notice (e.g., "zzgl. Versandkosten" with a link, or "kostenloser Versand"). 3. Click the shipping cost link if present. 4. Verify it navigates to the shipping information page with cost details. |
| **Expected Result** | A shipping cost notice is displayed near the product price per PAngV section 6. If free shipping applies, this is stated instead. The link to the shipping information page works correctly. |

### TC-51.9.7 — German number format for prices

| Field | Detail |
|---|---|
| **Test ID** | TC-51.9.7 |
| **Priority** | High |
| **Precondition** | Shop targets German market. |
| **Steps** | 1. Check price formatting on: product page, category page, cart, checkout, order confirmation. 2. Verify: comma as decimal separator (e.g., 49,99), dot or non-breaking space as thousands separator (e.g., 1.299,00 or 1 299,00), EUR symbol or "EUR" after the number. |
| **Expected Result** | All prices are formatted according to German conventions: comma for decimals, period for thousands, and EUR symbol placement is consistent throughout the shop. |

### TC-51.9.8 — Strikethrough prices comply with Omnibus Directive (30-day lowest price)

| Field | Detail |
|---|---|
| **Test ID** | TC-51.9.8 |
| **Priority** | Critical |
| **Precondition** | Products with sale/discount pricing exist. |
| **Steps** | 1. Navigate to a product with a strikethrough price. 2. Verify the reference price (strikethrough) is clearly labeled. 3. Verify it represents the lowest price in the preceding 30 days (per section 11 UWG implementing the Omnibus Directive). 4. If the shop states "UVP" (RRP), verify it is clearly distinguished from the shop's own prior price. 5. Check if the 30-day lowest price annotation is present (e.g., "Niedrigster Preis der letzten 30 Tage: XX,XX EUR"). |
| **Expected Result** | Strikethrough prices comply with the Omnibus Directive. The reference price is the genuine lowest price of the last 30 days. It is clearly labeled. No fictitious or inflated original prices are used. |

### TC-51.9.9 — 30-day lowest price displayed near sale pricing

| Field | Detail |
|---|---|
| **Test ID** | TC-51.9.9 |
| **Priority** | High |
| **Precondition** | Product with active sale price. |
| **Steps** | 1. Navigate to a sale product. 2. Look for the 30-day lowest price disclosure near the current price. 3. Verify the wording is clear (e.g., "Letzter niedrigster Preis: XX,XX EUR"). |
| **Expected Result** | The 30-day lowest price is displayed near the sale price. Consumers can see the genuine prior price for comparison. Required since May 28, 2022 (Omnibus Directive implementation in German law). |

### TC-51.9.10 — No hidden charges added at final checkout step

| Field | Detail |
|---|---|
| **Test ID** | TC-51.9.10 |
| **Priority** | Critical |
| **Precondition** | Items in cart, proceed through checkout to the final review step. |
| **Steps** | 1. Note the total on the cart page. 2. Proceed through checkout (address, shipping, payment). 3. On the final order summary, compare the total to the cart total plus shipping. 4. Verify no additional fees, surcharges, or costs were added that were not disclosed on the product or cart page. |
| **Expected Result** | The final total matches the sum of item prices + shipping + any disclosed fees. No hidden charges appear at the last step. PAngV section 3(2) prohibits undisclosed costs. |

### TC-51.9.11 — Price consistency across all touchpoints

| Field | Detail |
|---|---|
| **Test ID** | TC-51.9.11 |
| **Priority** | Critical |
| **Precondition** | Product with known price. |
| **Steps** | 1. Note the price on the product detail page. 2. Add to cart and check the cart price. 3. Proceed to checkout and check the checkout price. 4. Complete the order and check the confirmation page price. 5. Check the confirmation email price. |
| **Expected Result** | The product price is identical across all touchpoints: product page, cart, checkout summary, confirmation page, and confirmation email. No discrepancies. |

### TC-51.9.12 — Base price and total price correct for configurable products

| Field | Detail |
|---|---|
| **Test ID** | TC-51.9.12 |
| **Priority** | High |
| **Precondition** | Configurable product with options that affect price. |
| **Steps** | 1. Open a configurable product. 2. Select various options that change the price. 3. Verify the total price updates in real time. 4. If the product requires a base price, verify the base price also updates. 5. Add to cart and verify the correct configured price is reflected. |
| **Expected Result** | Both the total price and base price (if applicable) update dynamically and correctly for configurable products. The price in the cart matches the configured price on the product page. |

---

## 51.10 Cancellation Form Downloadable as PDF

### TC-51.10.1 — PDF download link present on cancellation policy page

| Field | Detail |
|---|---|
| **Test ID** | TC-51.10.1 |
| **Priority** | Critical |
| **Precondition** | Cancellation policy page is open. |
| **Steps** | 1. Navigate to the cancellation policy page. 2. Look for a link to download the model withdrawal form (Muster-Widerrufsformular) as a PDF. 3. Verify the link is clearly labeled. |
| **Expected Result** | A clearly labeled PDF download link for the model withdrawal form is present on the cancellation policy page. |

### TC-51.10.2 — PDF downloads successfully across all browsers

| Field | Detail |
|---|---|
| **Test ID** | TC-51.10.2 |
| **Priority** | High |
| **Precondition** | Cancellation policy page with PDF link is open. |
| **Steps** | 1. Click the PDF download link in Chrome. Verify download completes. 2. Repeat in Firefox. 3. Repeat in Safari. 4. Repeat in Edge. 5. Repeat on a mobile browser (iOS Safari, Android Chrome). |
| **Expected Result** | The PDF downloads successfully in all tested browsers. No download errors, timeouts, or browser-specific issues. |

### TC-51.10.3 — PDF is not blank, corrupt, or password-protected

| Field | Detail |
|---|---|
| **Test ID** | TC-51.10.3 |
| **Priority** | Critical |
| **Precondition** | PDF has been downloaded. |
| **Steps** | 1. Open the downloaded PDF in a PDF viewer (Adobe Acrobat Reader, browser PDF viewer, Preview on macOS). 2. Verify content is visible and readable. 3. Verify the file is not password-protected. 4. Verify the file is not corrupt (opens without error). |
| **Expected Result** | The PDF opens successfully, displays all content, is not password-protected, and is not corrupt. |

### TC-51.10.4 — PDF contains all required fields per BGB-InfoV Anlage 2

| Field | Detail |
|---|---|
| **Test ID** | TC-51.10.4 |
| **Priority** | Critical |
| **Precondition** | PDF is open. |
| **Steps** | Verify the PDF contains: 1. Addressee fields (trader name and address pre-filled). 2. Withdrawal declaration text ("I/we hereby give notice that I/we withdraw from my/our contract of sale of the following goods / provision of the following service"). 3. Fields for: product name, order number/date. 4. Customer name and address fields. 5. Customer signature field. 6. Date field. |
| **Expected Result** | All required fields per BGB-InfoV Anlage 2 are present. The form is complete and usable. |

### TC-51.10.5 — PDF text is selectable and copyable (not a scanned image)

| Field | Detail |
|---|---|
| **Test ID** | TC-51.10.5 |
| **Priority** | High |
| **Precondition** | PDF is open. |
| **Steps** | 1. Try to select text in the PDF with the cursor. 2. Copy selected text and paste it into a text editor. 3. Verify the text is actual text, not a rasterized image. |
| **Expected Result** | PDF text is selectable and copyable. This allows customers to fill in the form digitally (e.g., using a PDF form filler). |

### TC-51.10.6 — German characters (umlauts) render correctly

| Field | Detail |
|---|---|
| **Test ID** | TC-51.10.6 |
| **Priority** | High |
| **Precondition** | PDF is open. |
| **Steps** | 1. Check for correct rendering of: ae (ä), oe (ö), ue (ü), ss (ß), Ae (Ä), Oe (Ö), Ue (Ü). 2. Check the Euro symbol (EUR). 3. Look for any garbled or replacement characters. |
| **Expected Result** | All German special characters and the Euro symbol render correctly. No encoding issues. |

### TC-51.10.7 — PDF contains correct shop contact details for form submission

| Field | Detail |
|---|---|
| **Test ID** | TC-51.10.7 |
| **Priority** | High |
| **Precondition** | PDF is open. |
| **Steps** | 1. Verify the PDF contains the shop's name, postal address, and email address for receiving withdrawal declarations. 2. If applicable, verify fax number is included. 3. Cross-reference these details with the imprint page. |
| **Expected Result** | The PDF contains accurate and current contact details matching the shop's imprint. Customers can use these to send their withdrawal. |

### TC-51.10.8 — PDF file is properly named

| Field | Detail |
|---|---|
| **Test ID** | TC-51.10.8 |
| **Priority** | Low |
| **Precondition** | PDF has been downloaded. |
| **Steps** | 1. Check the downloaded file name. 2. Verify it is descriptive (e.g., `widerrufsformular.pdf`, `withdrawal-form.pdf`). 3. Verify it does not contain random strings, UUIDs, or confusing names. |
| **Expected Result** | The PDF has a clear, descriptive filename that customers can easily identify in their downloads folder. |

### TC-51.10.9 — Withdrawal form included in order confirmation email

| Field | Detail |
|---|---|
| **Test ID** | TC-51.10.9 |
| **Priority** | High |
| **Precondition** | Complete a test order. |
| **Steps** | 1. Place an order. 2. Check the order confirmation email. 3. Verify the model withdrawal form PDF is attached or linked. |
| **Expected Result** | The model withdrawal form is included in the order confirmation email as a PDF attachment or a permanent download link. Required by section 356(1) BGB for distance contracts. |

### TC-51.10.10 — Online cancellation workflow as alternative to PDF form

| Field | Detail |
|---|---|
| **Test ID** | TC-51.10.10 |
| **Priority** | Medium |
| **Precondition** | Customer account with a completed order. |
| **Steps** | 1. Log in to the customer account. 2. Navigate to order history. 3. Open a recent order. 4. Look for a "Cancel order" or "Request return" / "Withdraw from contract" button. 5. If present, complete the online cancellation workflow. 6. Verify a confirmation is displayed and an email is sent acknowledging the withdrawal. |
| **Expected Result** | If an online cancellation workflow exists, it functions correctly as an alternative to the paper/PDF form. The system acknowledges receipt of the withdrawal and sends confirmation. The 14-day refund period is triggered from this point. |

---

## 51.11 Button Solution: "Order with Obligation to Pay" on Order Button

### TC-51.11.1 — Order button displays legally compliant text

| Field | Detail |
|---|---|
| **Test ID** | TC-51.11.1 |
| **Priority** | Critical |
| **Precondition** | Items in cart, user is on the final checkout step. |
| **Steps** | 1. Navigate to the final order review step in checkout. 2. Locate the "Place Order" button. 3. Read the button text. 4. Verify it is one of the legally compliant formulations per section 312j(3) BGB: "zahlungspflichtig bestellen", "kaufen", "kostenpflichtig bestellen", "jetzt kaufen", or "verbindlich bestellen". |
| **Expected Result** | The order button text unambiguously signals a financial obligation. It is NOT a generic label like "Continue", "Next", "Submit", "Complete", "Finish", or "Place Order" (English labels are non-compliant for German-market shops). |

### TC-51.11.2 — Non-compliant button text detection (negative test)

| Field | Detail |
|---|---|
| **Test ID** | TC-51.11.2 |
| **Priority** | Critical |
| **Precondition** | Checkout page is displayed. |
| **Steps** | 1. Inspect the order button text. 2. Verify it is NOT any of the following non-compliant examples: "Continue", "Next", "Submit", "Complete order", "Finish", "Confirm", "Send order", "Weiter", "Absenden", "Fertig", "Bestellung abschliessen" (without the payment obligation reference). |
| **Expected Result** | The button text clearly communicates a payment obligation. None of the non-compliant examples listed are used. |

### TC-51.11.3 — Button text is readable text, not just an icon or image

| Field | Detail |
|---|---|
| **Test ID** | TC-51.11.3 |
| **Priority** | High |
| **Precondition** | Checkout page is displayed. |
| **Steps** | 1. Inspect the button element in DevTools. 2. Verify the text is rendered as actual text content (not an `<img>` tag, not `background-image` CSS, not icon-only). 3. Verify a screen reader can read the button label. |
| **Expected Result** | The button text is actual HTML text content, readable by screen readers. It is not hidden behind an image or icon. |

### TC-51.11.4 — Button solution applies to all checkout paths

| Field | Detail |
|---|---|
| **Test ID** | TC-51.11.4 |
| **Priority** | Critical |
| **Precondition** | Items in cart. |
| **Steps** | 1. Test the order button text in standard checkout (logged-in user). 2. Test in guest checkout. 3. Test in express checkout flows: (a) PayPal Express — check the final confirmation step, (b) Apple Pay — check the payment sheet or confirmation step, (c) Google Pay — check the payment sheet or confirmation step. 4. For each express flow, verify the user is informed of the financial obligation before the final confirmation. |
| **Expected Result** | Every checkout path displays a compliant order button or equivalent confirmation that clearly communicates the payment obligation. Express checkout flows either display the compliant text or rely on the wallet provider's built-in payment confirmation (which inherently communicates financial obligation). |

### TC-51.11.5 — Order summary visible adjacent to the order button

| Field | Detail |
|---|---|
| **Test ID** | TC-51.11.5 |
| **Priority** | Critical |
| **Precondition** | Checkout final step is displayed. |
| **Steps** | 1. Look at the content immediately above or adjacent to the order button. 2. Per section 312j(2) BGB, verify the following are displayed: (a) essential characteristics of the products, (b) total price including all taxes and fees, (c) shipping costs, (d) for subscriptions: contract duration and minimum contract duration. 3. Verify this information is not hidden behind a "Show details" accordion or collapsed section that the user must actively expand. |
| **Expected Result** | The order summary is fully visible and legible immediately above or beside the order button. All required information per section 312j(2) BGB is displayed without user action to expand or reveal it. |

### TC-51.11.6 — Summary information is legible (font size, contrast)

| Field | Detail |
|---|---|
| **Test ID** | TC-51.11.6 |
| **Priority** | High |
| **Precondition** | Checkout final step on both desktop and mobile. |
| **Steps** | 1. Inspect the order summary text near the order button. 2. Verify font size is not unreasonably small (min 12px recommended for body text). 3. Check color contrast ratio against WCAG AA standards (minimum 4.5:1 for normal text). 4. On mobile, verify the summary is not truncated or cut off. |
| **Expected Result** | The order summary is clearly legible. Text has adequate font size and color contrast. On mobile, no critical information is cut off or requires horizontal scrolling. |

### TC-51.11.7 — Button text not cropped on mobile screens

| Field | Detail |
|---|---|
| **Test ID** | TC-51.11.7 |
| **Priority** | High |
| **Precondition** | Mobile device or mobile emulation (iPhone SE — smallest common viewport). |
| **Steps** | 1. Navigate to the checkout final step on a small mobile screen. 2. Inspect the order button. 3. Verify the full text is visible (not truncated with "..." or cut off by the button's width). |
| **Expected Result** | The full legally required button text is visible on all mobile screen sizes. It is not truncated, ellipsized, or clipped. |

### TC-51.11.8 — Button only active after required checkboxes are checked

| Field | Detail |
|---|---|
| **Test ID** | TC-51.11.8 |
| **Priority** | High |
| **Precondition** | Checkout final step, T&C and cancellation policy checkboxes visible. |
| **Steps** | 1. Leave all checkboxes unchecked. 2. Check if the order button is disabled or if clicking it triggers validation errors. 3. Check the T&C checkbox only. 4. Try to click the order button. 5. Check all required checkboxes. 6. Try to click the order button. |
| **Expected Result** | The order button is either disabled until all required checkboxes are checked (with clear visual indication of the disabled state), or clicking the button while checkboxes are unchecked produces clear validation error messages. The order is never submitted without all required consents. |

### TC-51.11.9 — Double-click protection on order button

| Field | Detail |
|---|---|
| **Test ID** | TC-51.11.9 |
| **Priority** | Critical |
| **Precondition** | Checkout final step, all fields and checkboxes completed. |
| **Steps** | 1. Quickly double-click the order button. 2. Check if two orders are created. 3. Check if the button is disabled after the first click (showing a loading spinner or "Processing..." state). |
| **Expected Result** | Only one order is created. The button is disabled after the first click to prevent duplicate submissions. A loading indicator is shown. The second click is silently ignored. |

### TC-51.11.10 — Button text preserved after JavaScript disabled

| Field | Detail |
|---|---|
| **Test ID** | TC-51.11.10 |
| **Priority** | Medium |
| **Precondition** | Desktop browser with ability to disable JavaScript. |
| **Steps** | 1. Disable JavaScript in the browser (Chrome DevTools > Settings > Debugger > Disable JavaScript). 2. Navigate to the checkout (note: the checkout may not function fully, but the form should still render). 3. Check if the order button text is still the compliant text. 4. Re-enable JavaScript. |
| **Expected Result** | The button text is not dynamically injected by JavaScript alone. The compliant text is present in the HTML source (server-rendered). If JavaScript is required for checkout functionality, the form should display a fallback message, but the button text must remain compliant. |

### TC-51.11.11 — Post-update regression check for button text

| Field | Detail |
|---|---|
| **Test ID** | TC-51.11.11 |
| **Priority** | High |
| **Precondition** | After any shop platform update, theme change, or checkout redesign. |
| **Steps** | 1. After a platform update (e.g., Shopware, Magento, WooCommerce, or custom framework update), navigate to the checkout. 2. Verify the order button text has not reverted to a non-compliant default (many platforms ship with generic defaults). 3. Document the button text found. |
| **Expected Result** | The legally compliant button text is preserved after any update. Platform defaults have not overwritten the customized, compliant button label. This should be part of the post-deployment regression checklist. |

### TC-51.11.12 — Button text in subscription/recurring order checkout

| Field | Detail |
|---|---|
| **Test ID** | TC-51.11.12 |
| **Priority** | High |
| **Precondition** | Shop offers subscription products. |
| **Steps** | 1. Add a subscription product to the cart. 2. Proceed to checkout. 3. Verify the order button text appropriately communicates the recurring payment obligation (e.g., "Zahlungspflichtiges Abo bestellen" or equivalent). 4. Verify the order summary shows subscription terms: interval, total per period, minimum duration. |
| **Expected Result** | For subscription orders, the button text and order summary clearly communicate the recurring financial obligation, not just a one-time payment. The subscription terms are fully disclosed per section 312j(2) BGB. |

---

## 51.12 Cross-Cutting Legal Compliance Tests

### TC-51.12.1 — All legal pages accessible without JavaScript

| Field | Detail |
|---|---|
| **Test ID** | TC-51.12.1 |
| **Priority** | Medium |
| **Precondition** | Desktop browser with JavaScript disabled. |
| **Steps** | 1. Disable JavaScript. 2. Navigate to: imprint, T&C, cancellation policy, privacy policy. 3. Verify all pages load and display their content. |
| **Expected Result** | All legal pages are accessible and readable without JavaScript. Legal information must be available to all users regardless of browser capabilities. |

### TC-51.12.2 — All legal page links in footer are not broken (link check)

| Field | Detail |
|---|---|
| **Test ID** | TC-51.12.2 |
| **Priority** | High |
| **Precondition** | None. |
| **Steps** | 1. Navigate to the homepage. 2. Click each legal link in the footer: Impressum, AGB, Datenschutz, Widerruf. 3. Verify each navigates to a valid page (no 404, no 500, no blank page). 4. Repeat from 3 other page types (product, category, checkout). |
| **Expected Result** | All legal page links in the footer work correctly from every page. No broken links. |

### TC-51.12.3 — Legal pages render correctly on mobile

| Field | Detail |
|---|---|
| **Test ID** | TC-51.12.3 |
| **Priority** | High |
| **Precondition** | Mobile device or responsive emulation. |
| **Steps** | 1. Open each legal page on a mobile device: imprint, T&C, cancellation policy, privacy policy. 2. Verify: (a) text is readable without horizontal scrolling, (b) no content overflow, (c) tables (if any) are responsive, (d) PDF download links are accessible and functional. |
| **Expected Result** | All legal pages are fully readable and functional on mobile devices. No layout issues. All interactive elements are usable. |

### TC-51.12.4 — Legal pages available in all shop languages

| Field | Detail |
|---|---|
| **Test ID** | TC-51.12.4 |
| **Priority** | High |
| **Precondition** | Shop supports multiple languages. |
| **Steps** | 1. Switch to each supported language. 2. Navigate to each legal page. 3. Verify the content is translated and accurate. 4. Verify legal specifics (e.g., German law references) remain correct even in translated versions. |
| **Expected Result** | Legal pages exist in all supported languages. Translations are accurate. Law references are correct for the applicable jurisdiction (German law for German version, potentially adapted for other markets). |

### TC-51.12.5 — No pre-checked opt-in checkboxes anywhere on the site

| Field | Detail |
|---|---|
| **Test ID** | TC-51.12.5 |
| **Priority** | Critical |
| **Precondition** | None. |
| **Steps** | 1. Visit every form on the site: registration, checkout, newsletter signup, contact form, review submission. 2. On each form, inspect all checkboxes. 3. Verify none are pre-checked. 4. Inspect the HTML source to confirm no `checked` attribute is set by default. |
| **Expected Result** | Zero pre-checked opt-in checkboxes exist anywhere on the site. GDPR requires freely given, specific, informed, and unambiguous consent — pre-checked boxes do not meet this standard (CJEU Planet49 ruling). |

### TC-51.12.6 — Checkout checkbox texts correctly worded

| Field | Detail |
|---|---|
| **Test ID** | TC-51.12.6 |
| **Priority** | High |
| **Precondition** | Checkout page with checkboxes visible. |
| **Steps** | 1. Read the text of each checkout checkbox: T&C, cancellation policy, privacy policy, newsletter (if present). 2. Verify: (a) T&C checkbox text references accepting the Terms and Conditions with a link, (b) cancellation policy checkbox acknowledges the withdrawal rights with a link, (c) newsletter checkbox (if present) is separate from required checkboxes and clearly optional, (d) no bundled consent (e.g., "I accept T&C and subscribe to the newsletter" in a single checkbox). |
| **Expected Result** | Each checkbox has clear, specific wording. Consents are not bundled. Links to referenced documents work. Newsletter consent is clearly separated from mandatory legal acknowledgments. |

### TC-51.12.7 — Legal compliance preserved after cache purge

| Field | Detail |
|---|---|
| **Test ID** | TC-51.12.7 |
| **Priority** | Medium |
| **Precondition** | CDN or server-side cache is in use. |
| **Steps** | 1. Purge the full site cache (CDN, Varnish, application cache). 2. Navigate to the checkout as a new user. 3. Verify: (a) cookie consent banner appears, (b) order button has compliant text, (c) all legal page links work, (d) T&C/cancellation policy checkboxes are unchecked by default. |
| **Expected Result** | Legal compliance features are not dependent on cached content. After a full cache purge, all legal elements render correctly. |

### TC-51.12.8 — DSGVO Art. 17 — Account deletion request

| Field | Detail |
|---|---|
| **Test ID** | TC-51.12.8 |
| **Priority** | High |
| **Precondition** | Active customer account with order history. |
| **Steps** | 1. Log in to the customer account. 2. Navigate to account settings. 3. Look for a "Delete account" / "Konto loeschen" option. 4. Request account deletion. 5. Verify a confirmation is required (not instant deletion on first click). 6. Confirm deletion. 7. Verify the account is deactivated/deleted. 8. Try to log in again. |
| **Expected Result** | The customer can request account deletion per GDPR Art. 17 (Right to Erasure). After confirmation, the account is deleted or anonymized. Login with the old credentials fails. Order data may be retained for legal retention periods (tax law: 10 years) but personal data is anonymized. |

### TC-51.12.9 — DSGVO Art. 20 — Data portability / data export

| Field | Detail |
|---|---|
| **Test ID** | TC-51.12.9 |
| **Priority** | Medium |
| **Precondition** | Active customer account with data (orders, addresses, profile). |
| **Steps** | 1. Log in to the customer account. 2. Look for a "Download my data" / "Daten exportieren" option (either in account settings or via a support request process). 3. Request data export. 4. Download the export file. 5. Verify it contains: personal data, order history, addresses, and other stored information. 6. Verify the format is machine-readable (JSON, CSV, or XML). |
| **Expected Result** | The customer can export their personal data in a structured, commonly used, machine-readable format per GDPR Art. 20. If no self-service option exists, a clearly documented process for requesting data export via email/support must be available. |

### TC-51.12.10 — Newsletter double opt-in enforcement

| Field | Detail |
|---|---|
| **Test ID** | TC-51.12.10 |
| **Priority** | Critical |
| **Precondition** | Newsletter subscription form is available. |
| **Steps** | 1. Enter an email address in the newsletter subscription form. 2. Submit the form. 3. Verify a confirmation message is displayed (e.g., "Please check your email to confirm your subscription"). 4. Check the email inbox for a double opt-in confirmation email. 5. Do NOT click the confirmation link. 6. Verify no newsletter emails are sent to the unconfirmed address. 7. Click the confirmation link. 8. Verify subscription is now active. |
| **Expected Result** | The newsletter uses a strict double opt-in process. No marketing emails are sent before the confirmation link is clicked. The confirmation email clearly states what the user is subscribing to. Required by German UWG section 7 and GDPR Art. 6(1)(a). |
