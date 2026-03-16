# 27. Internationalization (if applicable)

### Multi-language support (texts, URLs)

- Verify that switching the language via the language switcher immediately renders all UI text, labels, button captions, and error messages in the selected language (de_DE → en_US → fr_FR round-trip).
- Verify that the URL structure changes appropriately per language: `/de/produkte/`, `/en/products/`, `/fr/produits/` — no language bleeds into wrong URL slugs.
- Verify that hreflang attributes in `<head>` are correct and bidirectional for all language/region pairs (e.g., `hreflang="de-DE"`, `hreflang="en-US"`, `hreflang="x-default"`).
- Verify that product descriptions, category names, CMS pages, and email templates all have translations and do not fall back to a raw translation key (e.g., `product.add_to_cart`) visible to the user.
- Verify that the language switcher preserves the current page context: switching language on a product page lands on the same product in the new language, not the homepage.
- Verify that language choice is persisted across sessions (cookie or account setting) so the user does not need to switch again on return visit.
- Verify that 404 and error pages are also translated — no English-only fallback when browsing in fr_FR.
- Verify that SEO meta tags (title, description, OG tags) are also translated per locale, not hard-coded in a single language.
- Verify that dynamic content (product names from the database, review text) renders without character encoding corruption for non-ASCII characters: umlauts (ä, ö, ü), accented characters (é, à, ñ), and special punctuation (« », „ ").
- Verify that a non-existent locale in the URL (e.g., `/xx/products/`) returns a proper 404, not a blank page or a PHP error.
- Verify that the sitemap contains locale-specific URLs for each language and that hreflang annotations in the sitemap are correct.
- Verify that canonical URLs per locale point to the correct locale URL, not a cross-language canonical.

### Currency conversion

- Verify that the displayed currency matches the selected locale or country: EUR for de_DE and fr_FR, USD for en_US, GBP for en_GB.
- Verify that currency conversion rates are sourced from a live or regularly refreshed feed — not hardcoded static rates — and that a clearly visible "prices shown in EUR, converted at today's rate" disclaimer is displayed when applicable.
- Verify rounding behavior: converted prices are rounded to 2 decimal places, with no floating-point artefacts (e.g., 19.999999 displayed instead of 20.00).
- Verify that the cart total, order summary, and confirmation email all show the same currency and identical amounts — no silent currency switch between steps.
- Verify that a coupon with a fixed amount (e.g., 10 EUR) is not applied as 10 USD when the user's session currency is USD — either convert the coupon value or restrict it to the original currency.
- Verify that the checkout is blocked or shows a clear warning if the payment gateway does not support the selected currency, rather than silently charging in a fallback currency.
- Verify that switching currency mid-session updates all prices already shown in the cart without requiring a page refresh.
- Verify that the currency symbol is positioned correctly per locale: USD uses prefix `$19.99`, EUR in de_DE uses suffix `19,99 €`, EUR in fr_FR uses suffix `19,99 €` with a non-breaking space before the symbol.
- Verify extreme exchange rate scenarios: a 1 USD product displayed in JPY should show a plausible integer value (e.g., ¥150), not ¥0 due to premature truncation.
- Verify that currency selection does not affect VAT calculation — tax is always computed on the base currency price before conversion.

### Country-specific prices

- Verify that a product priced at 99 EUR in Germany is displayed at the correct country-specific price for Austria (may differ due to explicit price rules), Switzerland (CHF, different price tier), and the United States (USD price tier).
- Verify that country-specific pricing is applied immediately when the user changes their delivery country in the checkout address form — prices in the cart update without a full page reload.
- Verify that a user logged into an account with a saved country preference sees country-specific prices from the first page load, not after explicit country selection.
- Verify that strikethrough/RRP prices are also updated to the country-specific equivalent — no EUR RRP shown next to a CHF sale price.
- Verify that B2B net prices (if applicable) are shown in the correct country currency with the country-specific net price tier.
- Verify that Google Shopping / product feed prices match the country-specific prices actually shown on the product page for the same country.
- Verify that country detection via IP address shows the correct default currency but always allows the user to manually override their country/currency.
- Verify that price rules do not stack unexpectedly: a country-specific price plus a coupon discount produces the correct final amount, not a double discount.

### Country-specific shipping costs

- Verify that shipping costs are calculated correctly for the domestic market (de_DE → Germany: standard zone), EU destinations (de_DE → France, Poland, Austria), and non-EU destinations (de_DE → Switzerland, UK post-Brexit, USA).
- Verify that the shipping cost estimator on the cart page updates dynamically when the user changes the destination country in the country selector.
- Verify that island/remote surcharges are applied for known postal code ranges: Canary Islands (Spain), Azores/Madeira (Portugal), Scottish Highlands (UK), Corsica (France).
- Verify that free-shipping thresholds are country-specific: a 49 EUR free-shipping threshold for Germany does not incorrectly apply to a Swiss CHF order.
- Verify that unavailable shipping methods for a specific country are not shown (e.g., a DHL domestic option must not appear as an option for a US destination).
- Verify that bulky-goods surcharges are applied correctly for international destinations and that the surcharge is shown in the correct currency.
- Verify that click & collect (in-store pickup) options are hidden when the user selects an international shipping destination.
- Verify that the checkout cannot be completed with a country for which no shipping method is configured — a clear error message must appear, not a silent 500 error.
- Verify that when the user selects a country after placing items in the cart, the revised shipping cost is reflected in the cart total before reaching the payment step.
- Verify that estimated delivery times shown in checkout adjust per country (e.g., 1–2 days for Germany vs. 5–10 days for the USA).

### Country-specific payment methods

- Verify that PayPal is available for de_DE, en_US, fr_FR, but potentially restricted for some markets (e.g., certain Eastern European countries).
- Verify that Klarna (invoice, installment, instant transfer) is only offered for supported Klarna markets (Germany, Austria, Switzerland, Sweden, Netherlands, UK, USA) and is hidden for all others.
- Verify that SEPA direct debit is offered exclusively for SEPA member countries and is not shown as an option for en_US or other non-SEPA locales.
- Verify that iDEAL appears as a payment option when the shipping/billing country is the Netherlands (nl_NL), and is hidden for all other countries.
- Verify that Bancontact appears for Belgium (nl_BE, fr_BE) and is hidden for other countries.
- Verify that the payment method list updates dynamically when the user changes their billing country mid-checkout — previously selected methods that are no longer valid are deselected with a clear message.
- Verify that Apple Pay and Google Pay availability is correctly determined by both the device/browser capability and any country restrictions imposed by the payment gateway.
- Verify that cash-on-delivery is only offered for countries where the logistics provider supports it.
- Verify that the "purchase on invoice" option (if offered) enforces address verification for the billing country and is not accessible for international orders if not supported.
- Verify that the correct payment processor acquirer is used per country (e.g., Stripe EUR for EU, Stripe USD for US) and that no currency mismatch errors occur at payment processing time.
- Verify that a user who selects a payment method valid in country A, then changes their country to B (where that method is invalid), receives an error or prompt before reaching the payment confirmation step.

### Country-specific tax rates

- Verify that the correct VAT rate is applied per country: Germany 19% (standard) / 7% (reduced), Austria 20% / 10%, France 20% / 5.5%, Switzerland 8.1% / 2.6% (non-EU), USA 0% (for EU-based shop exporting goods).
- Verify that the VAT rate used in the cart matches the VAT rate shown in the final order confirmation and on the PDF invoice — no rate discrepancy between checkout steps.
- Verify that providing a valid EU VAT ID (for B2B cross-border transactions within the EU) results in zero-rated VAT (Reverse Charge), and that the invoice clearly states "Reverse Charge — VAT to be accounted for by the recipient."
- Verify that VAT ID validation actually queries the EU VIES service and rejects syntactically valid but VIES-inactive VAT IDs.
- Verify that OSS (One Stop Shop) VAT registration is handled correctly: for digital goods or distance sales above the EU threshold, the destination country VAT rate is applied, not the seller's domestic rate.
- Verify rounding: 100.00 EUR at 19% VAT = 19.00 EUR tax, total 119.00 EUR. Confirm no cent discrepancy due to floating-point arithmetic.
- Verify that mixed-rate carts (products with different reduced/standard VAT rates) show a correct VAT breakdown per rate in the order summary and on the invoice.
- Verify that Swiss and UK orders (post-Brexit) correctly apply a 0% VAT rate for the shop (export), and that the checkout clearly informs the customer that local import duties and taxes may apply on arrival.
- Verify that a country change mid-checkout recalculates VAT in real time and the new VAT amount is visibly updated before the user confirms the order.
- Verify that B2B net price display (prices without VAT) is toggled correctly per country: German B2B customers see net prices, whereas B2C customers in all countries always see gross prices.

### Date format country-specific

- Verify that order dates and delivery estimates are displayed in the locale-appropriate format: `DD.MM.YYYY` for de_DE (e.g., 15.03.2026), `MM/DD/YYYY` for en_US (e.g., 03/15/2026), `DD/MM/YYYY` for en_GB (e.g., 15/03/2026), and `DD/MM/YYYY` for fr_FR (e.g., 15/03/2026).
- Verify that time displays include the correct locale-specific time format: 24-hour clock for de_DE and fr_FR (14:30), 12-hour AM/PM for en_US (2:30 PM).
- Verify that date pickers (preferred delivery date, date of birth for age verification) enforce the locale-specific format and do not accept, for example, `03/15/2026` when the active locale is de_DE.
- Verify that coupon expiry dates shown to the user are formatted in the active locale format — a German user must not see `Expires 03/15/2026` (US format).
- Verify that dates in transactional emails (order confirmation, shipping notification) are formatted according to the recipient's locale, not the server locale.
- Verify that date parsing is robust: a date stored as ISO 8601 (`2026-03-15`) in the database is always converted to the display locale format, and is never output raw to the frontend.
- Verify that relative time strings ("3 days ago", "ships in 2 business days") are translated and grammatically correct in each supported locale.
- Verify that business day calculations for delivery estimates correctly exclude national public holidays for the destination country (e.g., Fête Nationale on 14 July in France).
- Verify that the date format shown in account order history matches the locale of the currently logged-in user, even if the order was placed with a different locale.

### Address format country-specific (postal code field, etc.)

- Verify that the checkout address form renders the appropriate fields and labels per country: Germany shows "PLZ" as a 5-digit numeric field; the United States shows separate "ZIP Code" and "State" fields; France shows "Code postal" (5 digits); the UK shows "Postcode" (alphanumeric, e.g., SW1A 1AA).
- Verify that postal code validation is country-aware: a 5-digit German postal code is accepted for de_DE but rejected for en_GB (which requires the UK alphanumeric format); entering `ABCDE` must be rejected for de_DE.
- Verify that the "State / Province / County" field appears for countries that require it (USA, Canada, Australia, Brazil) and is hidden for countries where it is not applicable (Germany, France, Austria).
- Verify that the phone number field accepts and correctly validates international formats: `+49 30 12345678` for Germany, `+1 (555) 123-4567` for the USA, `+33 1 23 45 67 89` for France — and that the field label reflects the expected format.
- Verify that the country selector lists countries in an order that is appropriate to the shop's primary market — the most relevant countries (DE, AT, CH for a German shop) appear at the top rather than buried alphabetically.
- Verify that "company name" and "VAT ID" fields appear (or become required) when "business customer" is selected for countries where VAT IDs are expected (EU member states), and are hidden for countries without this concept (USA).
- Verify that the address line labels are adapted: "Hausnummer" as a separate field in some German form implementations is not shown to US customers who expect a single "Address Line 1" field.
- Verify that the address form correctly handles Japanese address order (prefecture before city before street) if Japanese locale is supported — the field order must match the local postal convention.
- Verify that autocomplete/address lookup (Google Places or equivalent) returns and populates results in the format appropriate to the selected country — UK results must not populate the State field, German results must fill "PLZ" with 5 digits.
- Verify that the address preview in the order confirmation renders in the country-appropriate format — a UK address displays "postcode after city on the final line" as per Royal Mail convention; a US address displays "City, State ZIP" on the last line.
- Verify that saved addresses in the customer account store and re-display country-specific fields correctly — a saved German address re-loaded for a new order retains the German postal format and does not have blank required fields due to format mismatch.
- Verify that address validation during checkout correctly handles non-Latin character sets in address fields (e.g., Arabic street names, Japanese kanji in city names) without stripping characters or causing encoding errors.
