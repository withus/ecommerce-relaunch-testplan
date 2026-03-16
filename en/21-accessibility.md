# 21. Accessibility (a11y)

---

### Keyboard Navigation (Tab, Enter, Escape)

- **Tab order follows logical reading order** — Tab through the entire page; focus moves top-to-bottom, left-to-right matching the visual layout. No focus jumps unexpectedly to unrelated sections. (WCAG 2.1 AA 2.4.3 Focus Order)
- **All interactive elements are reachable by keyboard** — Every link, button, form field, dropdown, modal trigger, and widget can be focused and activated using Tab (and Shift+Tab for reverse). No element is skippable unless intentionally hidden. (WCAG 2.1 AA 2.1.1 Keyboard)
- **Enter/Space activates focused elements** — Pressing Enter on a focused link navigates; pressing Enter or Space on a button triggers its action. Custom components (carousels, accordions, custom selects) respond correctly. (WCAG 2.1 AA 2.1.1)
- **Escape closes overlays and modals** — Opening a modal (quick view, cookie banner, size guide), a mega menu dropdown, or a flyout cart and pressing Escape closes it and returns focus to the triggering element. (WCAG 2.1 AA 2.1.2 No Keyboard Trap)
- **No keyboard trap exists** — Focus cannot become stuck inside any component. After a modal closes, focus returns to the element that opened it. Tab from the last item in a modal cycles back to the first item inside it (focus lock while open), then releases on Escape. (WCAG 2.1 AA 2.1.2)
- **Dropdown menus openable and closable by keyboard** — Mega menu categories open on Enter/Space or arrow key, all sub-links are reachable, and the menu closes on Escape without losing context. (WCAG 2.1 AA 2.1.1)
- **Checkout flow completable by keyboard alone** — All steps (address entry, shipping selection, payment selection, order placement) can be completed without a mouse. Test with: Tab, Enter, Space, arrow keys only.
- **Carousel/slider is keyboard-accessible** — Previous/next controls receive focus and activate on Enter/Space. Auto-playing sliders pause on keyboard focus. (WCAG 2.1 AA 2.2.2 Pause, Stop, Hide)
- **Date pickers and custom selects are operable** — Any custom date picker or styled select dropdown opens, allows value selection, and closes using keyboard alone. (WCAG 2.1 AA 2.1.1)
- **Product configurator options reachable** — Color swatches, size buttons, and material toggles are all focusable and selectable via keyboard without requiring a mouse click. (WCAG 2.1 AA 2.1.1)
- **Tool: axe DevTools** — Run axe browser extension on key pages; "keyboard trap" and "interactive element not focusable" violations must be zero.

---

### Focus Styles Visible

- **Focus indicator present on all interactive elements** — Every link, button, input, select, and custom widget shows a clearly visible focus ring when tabbed to. No `outline: none` or `outline: 0` without a custom replacement. (WCAG 2.1 AA 2.4.7 Focus Visible; WCAG 2.2 AA 2.4.11 Focus Appearance)
- **Focus contrast ratio meets minimum** — The focus indicator has a contrast ratio of at least 3:1 against the adjacent background color. (WCAG 2.2 AA 2.4.11)
- **Focus not obscured by sticky elements** — When a focused element is scrolled into view, it is not hidden behind a sticky header, cookie banner, or chat widget. (WCAG 2.2 AA 2.4.12 Focus Not Obscured)
- **Custom focus styles are consistent** — If the default browser outline is replaced, the custom style (e.g., colored outline, underline, box-shadow) is applied consistently across all interactive elements and across all browsers.
- **Focus visible in all color themes** — If a dark mode or alternative theme is offered, focus indicators remain visible against dark backgrounds.
- **Focus visible on buttons inside product gallery** — Image thumbnails, zoom toggle, and lightbox navigation arrows all show visible focus when tabbed to.
- **Cart and checkout CTAs show focus** — "Add to cart", "Proceed to checkout", and "Place order" buttons have unambiguous focus styles.
- **Tool: axe DevTools / WAVE** — Run WAVE (wave.webaim.org) on checkout page; inspect "Focus visible" alerts. Also manually tab through the page with browser DevTools open to verify no CSS is suppressing outlines.

---

### Screen Reader Compatible (ARIA Labels)

- **All buttons have accessible names** — Icon-only buttons (cart, wishlist heart, close modal X, hamburger menu) have either an `aria-label`, `aria-labelledby`, or visually hidden text so a screen reader announces their purpose. (WCAG 2.1 AA 4.1.2 Name, Role, Value)
- **ARIA landmarks are present and correct** — Page has `<header>`, `<nav>`, `<main>`, `<footer>` (or equivalent `role=""` attributes). Screen reader users can jump between landmarks. Test with NVDA+Firefox or VoiceOver+Safari. (WCAG 2.1 AA 1.3.6)
- **Dynamic content updates are announced** — Adding a product to cart, applying a coupon, or submitting a form shows a live region announcement (`aria-live="polite"` or `aria-live="assertive"`) so screen readers announce the status change without a page reload. (WCAG 2.1 AA 4.1.3 Status Messages)
- **Modal dialogs use correct ARIA** — Modals have `role="dialog"`, `aria-modal="true"`, and `aria-labelledby` pointing to the modal title. Screen reader focus is moved inside on open. (WCAG 2.1 AA 4.1.2)
- **Product variant changes are announced** — When selecting a different color/size and the price or availability updates, the change is announced via a live region or focus is moved to updated content. (WCAG 2.1 AA 4.1.3)
- **Accordion and tab widgets use correct ARIA** — Product description tabs use `role="tablist"`, `role="tab"`, `role="tabpanel"`, with `aria-selected` and `aria-controls`. Accordions use `aria-expanded` on triggers. (WCAG 2.1 AA 4.1.2)
- **Carousel uses ARIA roles and live regions** — A hero slider has `role="region"` with an accessible name, individual slides are announced, and navigation controls have descriptive labels ("Next slide", "Previous slide"). Auto-advance is pausable. (WCAG 2.1 AA 2.2.2)
- **Rating stars communicate value textually** — A 4-star product rating is not announced as "star star star star empty-star" but as "4 out of 5 stars" via `aria-label` or visually hidden text. (WCAG 2.1 AA 1.1.1)
- **Quantity inputs have accessible labels** — The quantity field in cart and product page is associated with a visible or hidden label; screen reader announces "Quantity" when focused. (WCAG 2.1 AA 1.3.1)
- **Navigation menus announce expanded state** — Dropdown nav items use `aria-expanded="true/false"` so screen reader users know whether a submenu is open. (WCAG 2.1 AA 4.1.2)
- **Tool: axe DevTools** — Zero critical ARIA violations on product page, cart, and checkout. **Tool: WAVE** — No "Missing form label" or "Empty button" errors. **Tool: Lighthouse a11y audit** — Score 90+ on all key pages.

---

### Alt Text on All Images

- **All product images have descriptive alt text** — Alt text describes the product, not just "product image". E.g., `alt="Blue ceramic table lamp 40cm"` not `alt="img1"`. (WCAG 2.1 AA 1.1.1 Non-text Content)
- **Decorative images use empty alt (`alt=""`)** — Background textures, dividers, and purely decorative icons have `alt=""` so screen readers skip them. They are NOT missing the attribute (that causes screen readers to read the filename). (WCAG 2.1 AA 1.1.1)
- **Logo has descriptive alt text** — The header logo has `alt="[Brand name] — Home"` or equivalent, not `alt="logo"` or `alt=""`. (WCAG 2.1 AA 1.1.1)
- **Category banner images have alt text** — Images at the top of category pages that convey context (e.g., showing lamp designs) have meaningful alt text.
- **Blog post images have alt text** — Every editorial image in blog posts has a description relevant to the article topic.
- **Thumbnails in gallery match main image context** — When switching product images via thumbnails, the alt text of each thumbnail (and the enlarged main image) reflects the specific view (e.g., "Front view", "Detail stitching", "Size comparison").
- **Image-based trust badges have alt text** — Payment method icons (Visa, Mastercard, PayPal), seal icons (Trusted Shops), and similar badge images have meaningful alt text (e.g., `alt="Pay with Visa"`). (WCAG 2.1 AA 1.1.1)
- **No placeholder alt text in production** — Search for `alt="TODO"`, `alt="image"`, `alt="photo"`, `alt="placeholder"` in rendered HTML — none should be present. **Tool: WAVE** flags missing/suspicious alt text. **Tool: axe DevTools** reports "image-alt" violations.
- **Inline SVG icons have accessible labels** — SVG icons used as functional elements (wishlist, cart, search) have `aria-label` or `<title>` elements. Decorative SVGs have `aria-hidden="true"`. (WCAG 2.1 AA 1.1.1)
- **Tool: Lighthouse a11y audit** — "Image elements do not have [alt] attributes" check must pass on all pages.

---

### Color Contrast Sufficient (WCAG AA)

- **Normal text meets 4.5:1 contrast ratio** — All body text, product descriptions, prices, and labels meet a minimum contrast ratio of 4.5:1 against their background. (WCAG 2.1 AA 1.4.3 Contrast Minimum)
- **Large text meets 3:1 contrast ratio** — Headings and text sized 18pt+ (or 14pt+ bold) meet a minimum of 3:1. (WCAG 2.1 AA 1.4.3)
- **UI component contrast meets 3:1** — Input field borders, button outlines, focus indicators, and interactive icons (when not decorative) meet 3:1 against adjacent colors. (WCAG 2.1 AA 1.4.11 Non-text Contrast)
- **Placeholder text contrast checked** — Form placeholder text (e.g., "Enter your email") meets 4.5:1. Many designs use light gray placeholders that fail. (WCAG 2.1 AA 1.4.3)
- **Disabled state contrast is intentionally lower — documented** — Disabled buttons may have lower contrast; verify this is intentional and documented. Note: WCAG explicitly exempts disabled components from 1.4.3, but usability should still be considered.
- **Sale/discount price contrast verified** — Red strikethrough "old price" and green "sale price" text both meet contrast requirements against white backgrounds.
- **Badge and label contrast checked** — "New", "Sale", "Out of stock" badges with colored backgrounds have sufficient contrast for their text. (WCAG 2.1 AA 1.4.3)
- **Contrast on product images or hero banners** — Text overlaid on hero banner images (headlines, CTAs) meets contrast requirements against the image at all viewport widths. Use semi-transparent overlays if needed.
- **Dark mode contrast verified (if offered)** — All text and UI elements meet contrast requirements in dark mode as well.
- **Tool: axe DevTools** — "color-contrast" is a critical violation; must be zero on all pages. **Tool: Lighthouse a11y audit** — "Background and foreground colors do not have a sufficient contrast ratio" must pass. **Tool: WAVE** — No contrast errors. Manual check: browser extension "Colour Contrast Analyser" by TPGi.

---

### Font Size Adjustable

- **Page does not break when browser zoom is set to 200%** — At 200% zoom (browser-level), all content is readable and functional; no text is cut off, no buttons become unreachable, no horizontal scrollbar appears on a 1280px-wide viewport. (WCAG 2.1 AA 1.4.4 Resize Text)
- **Text sized in relative units (rem/em), not px** — Body text, headings, and form labels use `rem` or `em` so they scale with user's browser font-size preference. (WCAG 2.1 AA 1.4.4)
- **User OS font-size setting is respected** — Setting the OS or browser default font size to "Large" or 20px and loading the shop does not break layouts or cause text overlap.
- **No fixed-height containers clip text at large sizes** — Elements with fixed `height` in CSS are tested at 200% zoom to confirm text does not overflow out of its container and become invisible. (WCAG 2.1 AA 1.4.4)
- **Checkout forms readable at 200% zoom** — All form fields, labels, error messages, and submit buttons remain visible and operable at 200% zoom. No fields overlap.
- **Mobile viewport: minimum font size not below 16px** — Body text on mobile is at least 16px (CSS) to avoid iOS auto-zoom on input focus and to ensure readability. (Related: WCAG 2.1 AA 1.4.4)
- **Tool: Lighthouse a11y audit** — Check for meta viewport tags that prevent scaling (`user-scalable=no` or `maximum-scale=1`) — these fail WCAG 1.4.4 and must not be present.

---

### Form Labels Correctly Linked

- **Every input has an associated `<label>`** — All `<input>`, `<select>`, and `<textarea>` elements have a `<label>` with a matching `for` attribute pointing to the input's `id`. Alternatively, `aria-label` or `aria-labelledby` is used. (WCAG 2.1 AA 1.3.1 Info and Relationships; 4.1.2 Name, Role, Value)
- **Labels are not placeholder-only** — Forms do not rely solely on placeholder text as the visible label. Once the user starts typing, the label disappears and they cannot see what the field requires. A visible label above or beside the input must always be present. (WCAG 2.1 AA 1.3.1)
- **Grouped inputs use fieldset and legend** — Radio button groups (payment method selection, shipping option selection) and checkbox groups are wrapped in `<fieldset>` with a descriptive `<legend>`. (WCAG 2.1 AA 1.3.1)
- **Autocomplete attributes correct on checkout form** — Name, address, phone, email, and payment fields have correct `autocomplete` values (e.g., `autocomplete="given-name"`, `autocomplete="postal-code"`) enabling browser and screen reader autocomplete. (WCAG 2.1 AA 1.3.5 Identify Input Purpose)
- **Required fields indicated accessibly** — Required fields are marked with `required` attribute (or `aria-required="true"`) and a visible indicator (e.g., asterisk) explained near the form. Not just color alone. (WCAG 2.1 AA 3.3.2 Labels or Instructions)
- **Quantity field label in cart** — The cart quantity input has an accessible label that includes the product name (e.g., `aria-label="Quantity for Blue Table Lamp 40cm"`) so screen readers can distinguish multiple quantity fields. (WCAG 2.1 AA 1.3.1)
- **Search field has a label** — The header search input has either a visible label, a visually hidden `<label>`, or `aria-label="Search"`. (WCAG 2.1 AA 4.1.2)
- **Tool: WAVE** — "Missing form label" errors must be zero. **Tool: axe DevTools** — "label" violation must be zero on all forms. Manual check: click on each label to confirm focus moves to its input.

---

### Error Messages Screen Reader Accessible

- **Error messages are programmatically associated with their input** — Inline validation errors are linked via `aria-describedby` on the input pointing to the error element's `id`. Screen readers announce the error when the field receives focus. (WCAG 2.1 AA 3.3.1 Error Identification)
- **Errors are announced immediately on occurrence** — When a form is submitted with errors, screen readers are notified. Either focus moves to the first error, or an `aria-live="assertive"` region announces the number of errors. (WCAG 2.1 AA 3.3.1)
- **Error message text identifies the field and describes the problem** — "This field is required" is acceptable; "Error" alone is not. Messages like "Please enter a valid email address in the format name@example.com" are preferred. (WCAG 2.1 AA 3.3.1)
- **Inline validation errors are not color-only** — Error states use an icon, text, or border change in addition to a red color change. (WCAG 2.1 AA 1.4.1 Use of Color)
- **Success messages are also announced** — After successful form submission, coupon application, or newsletter signup, the confirmation message is in an `aria-live` region or focus is moved to it. (WCAG 2.1 AA 4.1.3 Status Messages)
- **Error summary at top of form (for multi-field errors)** — On checkout form submission failure, an error summary listing all invalid fields appears at the top and receives focus, allowing screen reader users to navigate through all errors. (WCAG 2.1 AA 3.3.3 Error Suggestion)
- **Payment error messages are accessible** — When a card is declined or payment fails, the error message from the payment provider is shown in an accessible way (not just a toast that disappears). (WCAG 2.1 AA 3.3.1)
- **Tool: axe DevTools** — "aria-describedby" and "form-field-multiple-labels" checks. **Tool: WAVE** — "Missing or broken aria-describedby" alerts. Manual test: Use NVDA or VoiceOver to submit a form with intentionally wrong data and verify all errors are announced.

---

### Skip-to-Content Link

- **A skip-to-content link is present as the first focusable element** — When tabbing from the browser address bar, the very first Tab keypress focuses a "Skip to main content" link before the header/navigation. (WCAG 2.1 AA 2.4.1 Bypass Blocks)
- **Skip link is visually hidden by default but visible on focus** — The link is off-screen (via CSS `position: absolute; top: -100%` or similar) until focused, at which point it becomes visible with sufficient contrast and size. (WCAG 2.1 AA 2.4.1)
- **Skip link target is the `<main>` landmark or `#main-content`** — Activating the skip link moves focus to the main content area, bypassing the navigation. Verify with keyboard: Tab once, Enter — focus and scroll jump to the content area. (WCAG 2.1 AA 2.4.1)
- **Skip link works on all page types** — Verify on homepage, category page, product page, checkout, and account pages. (WCAG 2.1 AA 2.4.1)
- **Additional skip links if needed** — Pages with a long sidebar filter panel or multiple navigation blocks may benefit from additional skip links (e.g., "Skip to product list"). Evaluate based on keyboard navigation burden.
- **Tool: Lighthouse a11y audit** — "Page does not have a heading, skip link, or landmark region" check. **Tool: WAVE** — Reports presence of skip navigation links under "Features".

---

### No Purely Visual Information (e.g., Color Only = Status)

- **Stock status not indicated by color alone** — "In stock" (green), "Low stock" (orange), "Out of stock" (red) indicators also use text labels, icons, or patterns so users who cannot distinguish colors receive the information. (WCAG 2.1 AA 1.4.1 Use of Color)
- **Required form fields not marked by color alone** — Required fields marked only in red fail WCAG 1.4.1. An asterisk (*) with a legend, or the word "required", must accompany any color coding. (WCAG 2.1 AA 1.4.1)
- **Selected product variant has non-color indicator** — When a color swatch or size button is selected, the selected state is shown by a border, checkmark, or other visual change beyond color difference. Unavailable variants use a strikethrough, not just desaturation. (WCAG 2.1 AA 1.4.1)
- **Form validation states not color-only** — Invalid fields shown with a red border also show an error icon or text. Valid fields shown in green also show a checkmark or text confirmation. (WCAG 2.1 AA 1.4.1)
- **Links are distinguishable from surrounding text** — If hyperlinks within body text are not underlined, they must have a 3:1 contrast ratio against surrounding non-link text in addition to the standard background contrast. (WCAG 2.1 AA 1.4.1)
- **Order status in customer account not color-only** — "Processing" (blue), "Shipped" (green), "Cancelled" (red) order statuses include a text label alongside the color indicator. (WCAG 2.1 AA 1.4.1)
- **Charts and graphs use patterns or labels, not color alone** — Any analytics charts or infographics in the admin or customer area use labels or patterns in addition to color coding. (WCAG 2.1 AA 1.4.1)
- **Tool: WAVE** — "Very low contrast" errors may surface color-only issues. Manual check: view the page in grayscale (DevTools Rendering panel → Emulate vision deficiencies → Achromatopsia) and confirm all information is still conveyed. Also test with "Deuteranopia" and "Protanopia" emulation.

---

### Video Subtitles (if Videos Present)

- **All pre-recorded video content has synchronized captions** — Product demonstration videos, brand videos, and tutorial videos have accurate, synchronized closed captions. Auto-generated captions alone do not suffice unless manually reviewed for accuracy. (WCAG 2.1 AA 1.2.2 Captions Pre-recorded)
- **Captions are toggleable by the user** — If using a custom video player, the caption toggle button is keyboard-accessible and has an accessible label ("Toggle captions"). (WCAG 2.1 AA 1.2.2)
- **Captions identify speakers** — If multiple people speak in a video, captions identify who is speaking. (WCAG 2.1 AA 1.2.2)
- **Audio descriptions for video with visual-only information** — If a product video shows features that are not described in the audio track (e.g., color options shown silently), an audio description track or text alternative must be provided. (WCAG 2.1 AA 1.2.5 Audio Description Pre-recorded)
- **Background/ambient videos are pausable or muted** — Auto-playing hero videos with no spoken content must be pausable and must not auto-play with audio. A visible pause control must be provided. (WCAG 2.1 AA 2.2.2 Pause, Stop, Hide; 1.4.2 Audio Control)
- **Embedded YouTube/Vimeo videos inherit platform captions** — When using YouTube embeds, confirm caption availability in the embedded player. Ensure the embed does not hide the caption control button.
- **Text transcript provided as alternative** — For any video with spoken content, a text transcript is available as an additional aid (best practice beyond WCAG AA minimum; required for WCAG AAA 1.2.8). Strongly recommended for product explainer videos. (WCAG 2.1 AAA 1.2.8)
- **Tool: Lighthouse a11y audit** — Reports "video element does not have captions" for `<video>` elements without `<track kind="captions">`. Manual review of caption quality required as tools cannot verify accuracy.
