# 34. Tax & Price Calculation

### VAT calculation for different countries (DE 19%, AT 20%, CH 0%, etc.)

- Verify that a customer with a German billing address is charged exactly 19% VAT; product priced at €100.00 net results in €119.00 gross.
- Verify that an Austrian billing address triggers 20% VAT; same €100.00 net product displays €120.00 gross.
- Verify that a Swiss billing address results in 0% VAT applied (CH is not EU); net price equals gross price.
- Verify that a French billing address applies French standard rate (20%) rather than the shop's domestic rate.
- Verify that changing the country in checkout recalculates VAT in real time before the order is placed.
- Verify that reduced VAT rates are applied for eligible product categories (e.g., books at 7% in Germany instead of 19%).
- Verify that an order shipped to a non-EU country outside EU VAT scope shows 0% VAT in the order summary.
- Verify that when a customer switches country from DE to AT mid-checkout, the displayed VAT amount and rate label both update correctly.
- Verify that the VAT rate used at the time of order placement is stored on the order record and does not retroactively change if tax rules are updated later.
- Verify that the correct country's VAT rate is used when billing address and shipping address are in different countries (determine which one governs: shipping destination for goods is typically decisive).
- Verify that products with a mixed VAT scenario (e.g., a bundle containing a book at 7% and a lamp at 19%) show each item's VAT rate separately on the invoice.
- Verify that a customer who changes their stored country in their account profile sees updated prices on their next visit.

### Net/gross toggle (B2B vs. B2C)

- Verify that B2C customers (no VAT ID provided) always see gross prices including VAT on all pages (product page, cart, checkout, confirmation).
- Verify that a B2B customer with a valid, verified VAT ID sees net prices excluding VAT on all pages after entering the VAT ID.
- Verify that the net/gross toggle (if visible as a UI element) correctly recalculates and redisplays all prices without a page reload.
- Verify that entering an invalid VAT ID format is rejected with an appropriate error message and the customer remains in B2C (gross) mode.
- Verify that a valid intra-EU VAT ID (e.g., DE123456789) triggers zero-rated intra-community supply and shows 0% VAT on the order.
- Verify that a domestic VAT ID (same country as shop) does NOT trigger zero-rating — full domestic VAT still applies.
- Verify that the order confirmation email and PDF invoice reflect the correct net/gross mode that was active at checkout.
- Verify that switching from B2B to B2C mode (removing VAT ID) immediately reinstates VAT on all displayed prices.
- Verify that a customer group specifically configured as "B2B net display" always sees net prices regardless of whether they enter a VAT ID.
- Verify that the cart total shown in the mini-cart header updates immediately when the net/gross mode changes.
- Verify that the tax disclosure line ("incl. 19% VAT: €X.XX" or "excl. VAT") is accurate and matches the mode selected.

### Rounding logic with discounts + VAT combination

- Verify that applying a 10% percentage discount to 3 items at €9.99 each (subtotal €29.97) results in a discount of €3.00 (rounded correctly) and a discounted subtotal of €26.97, not €26.973.
- Verify that a fixed €5.00 discount applied to a €29.97 cart gives a subtotal of €24.97 and that subsequent VAT calculation is based on this discounted amount.
- Verify that VAT is calculated on the post-discount amount, not on the pre-discount gross.
- Verify that rounding is applied after VAT calculation (not before), to avoid compounding rounding errors: e.g., 3 items at €0.01 each with 19% VAT does not show €0.00 VAT.
- Verify the "1 cent rounding" edge case: total of €10.005 rounds to €10.01 (half-up) consistently across product page, cart, and checkout.
- Verify that a coupon of 15% off a cart containing items at different VAT rates (7% and 19%) correctly pro-rates the discount across rates and rounds each component independently.
- Verify that a scenario of 1 item at €0.99 with 19% VAT shows VAT of €0.16 (not €0.158 or €0.15) — confirm correct half-up rounding.
- Verify that a scenario of 2 items at €0.99 with 19% VAT shows VAT of €0.32 (2 × €0.158 = €0.316, rounds to €0.32), not €0.31.
- Verify that a discount code for "20% off" on an order of €49.95 yields a discount of €9.99 (not €9.990) and a total of €39.96.
- Verify that when a free shipping coupon is applied alongside a percentage discount, the shipping line shows €0.00 and the discount line shows only the product discount — no double-counting.
- Verify that rounding behavior is identical regardless of whether the customer pays by credit card, PayPal, or bank transfer (payment method must not affect price).
- Verify that the sum of all line-item VAT amounts on an invoice equals the total VAT figure (no hidden rounding residual).

### Price consistency: product page = cart = checkout = confirmation

- Verify that the price shown on the product detail page matches exactly the price added to the cart (same variant, same quantity of 1).
- Verify that the cart subtotal matches the sum of (unit price × quantity) for all line items without any discrepancy.
- Verify that the order summary on the checkout page shows the identical line-item prices and totals as the cart page.
- Verify that the order confirmation page (thank-you page) shows the same total as was presented at the final checkout step before clicking "Place Order."
- Verify that the order confirmation email reflects the exact same amounts shown on the confirmation page.
- Verify that the PDF invoice generated after order placement contains the same prices as the order confirmation email.
- Verify that a price change in the backend during an active checkout session does not silently update the price the customer is charged — either lock the price or display a warning.
- Verify that a coupon applied on the cart page is still reflected correctly at all subsequent steps (not silently dropped or recalculated differently).
- Verify that shipping costs displayed on the cart page match those on the checkout shipping step and the final order summary.
- Verify that the order record stored in the database matches the amounts on the confirmation page (test via admin order view).
- Verify that a customer who browses for 30 minutes and then checks out is not affected by a flash sale that started during their session — or if they are, they are shown the updated price clearly before confirming.
- Verify that the tracking/analytics purchase event fires with the same revenue amount as the confirmed order total.

### Currency formatting (comma vs. dot, symbol position)

- Verify that German locale displays prices as "€ 1.234,56" (period as thousands separator, comma as decimal, symbol before amount) per German convention.
- Verify that English (UK/US) locale displays prices as "£1,234.56" or "$1,234.56" with comma as thousands separator and dot as decimal.
- Verify that Swiss locale (CHF) displays "CHF 1'234.56" using the Swiss apostrophe as thousands separator.
- Verify that prices without cents display consistently (e.g., "€ 10,00" rather than "€ 10" or "€ 10.00") according to the locale setting.
- Verify that zero amounts display as "€ 0,00" (not blank, null, or "€ -").
- Verify that negative amounts (e.g., discount lines) are displayed with a leading minus sign: "−€ 9,99" not "€ −9,99" or "(€ 9,99)" unless the design specifies parenthesis notation.
- Verify that very large order amounts (e.g., €10,000.00) have the thousands separator applied correctly and do not overflow the UI layout.
- Verify that the currency symbol does not appear duplicated (e.g., "€ €10,00") due to a template rendering bug.
- Verify that emails use the same currency formatting as the web interface (no locale mismatch between server-rendered email and browser-rendered page).
- Verify that switching the shop language changes the currency format accordingly and that prices do not shift or overlap in the layout.

### Tiered pricing / volume discounts correctly calculated

- Verify that a product configured with tiered pricing (e.g., 1–4 units at €20.00, 5–9 units at €18.00, 10+ units at €15.00) displays the correct unit price when the quantity selector reaches each threshold.
- Verify that the cart total uses the tier price applicable to the quantity in the cart, not the base price.
- Verify that adding 4 items at the €20.00 tier and then increasing to 5 automatically recalculates the line total at €18.00 × 5 = €90.00, not €18.00 × 1 + €20.00 × 4.
- Verify that reducing the quantity from 10 back to 4 reverts the unit price to the €20.00 tier, not the €15.00 tier.
- Verify that tiered prices are shown in a pricing table on the product page so the customer can see all thresholds before choosing a quantity.
- Verify that VAT is applied to the tier-adjusted price, not to the base price.
- Verify that a coupon discount stacks correctly with a volume discount: apply the coupon to the post-tier price (or per configuration), not the pre-tier base price.
- Verify that a cart containing two different products, each qualifying for a volume tier on its own, calculates each tier independently (tiers are per-product, not cart-wide unless specifically configured).
- Verify that the checkout order summary line shows the per-unit tier price alongside the total (e.g., "5 × €18.00 = €90.00").
- Verify that tiered pricing edge case: exactly 5 items at the 5-unit threshold results in the lower tier price, not the upper tier (boundary inclusion is correct).
- Verify that the PDF invoice reflects the tier price used, not the regular price, to avoid confusion for the customer.

### Price display for configurable products (dynamic update)

- Verify that selecting a product variant (e.g., size "Large" with a +€5.00 surcharge) updates the displayed price in real time without a page reload.
- Verify that deselecting a paid option (e.g., unchecking "Gift wrapping +€2.50") immediately removes the surcharge from the displayed price.
- Verify that the base price is clearly distinguished from the surcharge in the displayed price (e.g., "€30.00 + €5.00 for Large = €35.00") or the total updates cleanly.
- Verify that multiple simultaneous option changes (e.g., selecting a size and a color with different surcharges) result in the sum of all surcharges added to the base price.
- Verify that the final price shown on the product page after all options are selected matches what ends up in the cart.
- Verify that a percentage-based option surcharge (e.g., "+10% for premium material") is calculated on the current base price, not on a stale cached value.
- Verify that a configuration with a price reduction option (e.g., "without packaging −€2.00") correctly subtracts and does not go below a minimum price if one is enforced.
- Verify that the price update occurs even when JavaScript is slow or the network is throttled — the displayed price should not get out of sync with the actual cart price.
- Verify that the "Add to Cart" button is disabled or shows a warning until all required options are selected, preventing a misconfigured item from being added at the wrong price.
- Verify that refreshing the product page with a previously selected configuration restores the same price without errors.
- Verify that switching back to a previously selected variant combination restores the exact same price as was shown initially.

### Strikethrough prices / RRP correctly displayed

- Verify that a product with an RRP (recommended retail price) of €49.99 and a sale price of €39.99 shows both prices, with the RRP visually struck through.
- Verify that the percentage savings badge (e.g., "−20%") is mathematically correct: (49.99 − 39.99) / 49.99 × 100 = 20.0%, displayed as "−20%."
- Verify that a rounding edge case for savings percentage is handled: e.g., RRP €29.99, sale price €19.99 gives 33.35% savings — decide whether to round down (33%) or round half-up (33%) and apply consistently.
- Verify that products not on sale show only the regular price without a strikethrough or misleading "was" label.
- Verify that an expired sale (end date passed) automatically removes the strikethrough display and reverts to the regular price.
- Verify that the strikethrough price does not appear in cart, checkout, or invoice if it was not part of the actual transaction price (to avoid UTP — unfair trading practice concerns).
- Verify that the "lowest price in the last 30 days" is displayed where required by the EU Omnibus Directive (for discounted products) and is accurate.
- Verify that a product with multiple pricing layers (tiered + sale + coupon) shows a coherent single final price rather than multiple overlapping strikethrough values.
- Verify that variant changes (e.g., changing size) correctly update or remove the strikethrough price if the new variant has a different or no RRP.
- Verify that structured data (Schema.org `offers`) contains the correct `price` (sale price) and `highPrice`/`priceValidUntil` fields matching what is visually displayed.

### Tax disclosure on invoice (mandatory information)

- Verify that every PDF invoice contains the legally required VAT amount as a separate line: e.g., "incl. 19% VAT: €X.XX."
- Verify that the shop's VAT identification number (Umsatzsteuer-Identifikationsnummer) is printed on every invoice.
- Verify that the shop's full legal address appears on every invoice.
- Verify that the invoice number is sequential, unique, and non-reusable.
- Verify that the invoice date matches the order date (or payment receipt date, per configuration) and is formatted correctly for the locale.
- Verify that for an order containing items at multiple VAT rates (e.g., 7% and 19%), the invoice shows a separate tax disclosure line for each rate (e.g., "VAT 7%: €X.XX" and "VAT 19%: €X.XX").
- Verify that intra-EU B2B orders (reverse charge) include the text "Reverse Charge — Tax liability transfers to the recipient" and show 0% VAT on the invoice.
- Verify that a credit note (for a return/refund) carries a reference to the original invoice number and also discloses the VAT amount being reversed.
- Verify that the net subtotal, total VAT, and gross total on the invoice mathematically add up: net + VAT = gross, with no rounding discrepancy.
- Verify that a free-of-charge item (e.g., a promotional gift) on the invoice shows €0.00 rather than being omitted, to maintain a complete item list.
- Verify that the "amount paid" or "payment method" field on the invoice correctly reflects the method used (e.g., "Paid via PayPal on 2026-03-01").
- Verify that invoices for cancelled orders or zero-value orders (fully covered by a coupon) still include all mandatory fields and show €0.00 totals rather than being suppressed.
