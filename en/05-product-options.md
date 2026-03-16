# Product Options / Configurator

### Simple options (dropdown, radio, checkbox)

- Open a product page that has a dropdown option (e.g., "Size"). Verify the dropdown renders with all expected choices and no placeholder value is selected by default unless intended.
- Select each available value in a dropdown one by one. Verify the selected value is visually highlighted and the correct value is stored before proceeding to cart.
- On a product with radio button options (e.g., "Color"), click each radio button. Verify only one radio button can be active at a time and the previously selected one deselects.
- On a product with checkbox options (e.g., "Add gift wrapping"), check the box. Verify it can be independently checked and unchecked without affecting other options.
- Open the same product on a mobile device. Verify dropdowns open a native mobile picker or a usable custom picker, and radio/checkbox targets are large enough to tap without mis-tapping.
- Verify that the option label text is fully visible and not truncated for all option values, including long names (e.g., "Extra-large reinforced double-stitched canvas").
- Open the product configurator via keyboard only (Tab to reach the field, Space/Enter/Arrow keys to select values). Verify full keyboard operability for all control types.
- Verify that selecting an option does not cause an unexpected full page reload; only the relevant page section (price, image, availability) should update.
- Add a product to cart without interacting with any option. Verify behavior: either the system pre-selects a default value and adds it, or it blocks the action and prompts the user to make a selection.

---

### Required options vs. optional options

- Identify a product with at least one required option and one optional option. Attempt to add the product to cart without filling in any option. Verify the system blocks the add-to-cart action and clearly indicates which option is required.
- Fill in the required option but leave the optional option empty. Attempt to add to cart. Verify the product is added successfully with the optional option either absent or set to its default.
- Verify that required options are visually distinguished from optional ones (e.g., asterisk, label "required", color indicator) before any interaction.
- After a failed add-to-cart attempt due to a missing required option, verify the focus moves to or scrolls to the unfilled required field so the user does not have to search for it.
- Complete all required options, then add to cart. Open the cart and verify the selected required option values are correctly listed in the line item details.
- On a product with multiple required options, fill only the first one and attempt to add to cart. Verify all unfilled required options are flagged simultaneously, not one at a time.
- Verify that optional options with no selection do not appear in the cart line item or order confirmation as empty fields causing confusion.
- Reload the product page mid-selection. Verify that the required/optional distinction is consistent after reload and no options silently become optional or vice versa.

---

### Options with price surcharge

- Select a product option that carries a documented price surcharge (e.g., "Premium material +€5.00"). Verify the displayed price increases by exactly the surcharge amount immediately upon selection.
- Deselect the surcharge option (switch back to the base option). Verify the price returns exactly to the base price with no residual addition.
- Select multiple options each carrying individual surcharges. Verify the total displayed price equals the base price plus the sum of all active surcharges.
- Add the product with a surcharge option to the cart. Open the cart page. Verify the line item price matches the price shown on the product page at the time of adding.
- Proceed to checkout with a surcharge option in the cart. Verify the surcharge-adjusted price is carried consistently through the order summary and payment step.
- Verify the surcharge label is displayed next to or below the option (e.g., "+ €5.00") so the customer understands the cost before selecting.
- Check the order confirmation email. Verify it shows the correct total including the surcharge, and ideally lists the selected option that caused it.
- On a product with tiered pricing AND a surcharge option, select the surcharge option at different quantity levels. Verify the surcharge is correctly applied per unit and reflected in the tiered total.

---

### Options with price reduction

- Select an option that carries a documented price reduction (e.g., "Without accessories –€10.00"). Verify the displayed price decreases by exactly the reduction amount immediately upon selection.
- Switch between a reduction option and the default option multiple times. Verify the price updates correctly each time with no accumulation or drift.
- Select a price-reducing option and add to cart. Open the cart. Verify the reduced price is shown, not the base price.
- Verify that the price reduction is not displayed as a negative surcharge in a confusing way; the UI should clearly present it as a saving (e.g., "–€10.00" or "Save €10.00").
- Combine a price-reducing option with a coupon code in the cart. Verify the coupon discount is calculated on the already-reduced price (or on the base price, per business rules) and the total is mathematically correct.
- Proceed through checkout with a price-reducing option. Verify the invoice/order document reflects the final reduced price and the reduction is either itemised or clearly shown.
- Verify the reduced price does not go below zero for any legitimate option combination; if it could mathematically, the system should floor at €0.00 or block the combination.

---

### Percentage-based price changes

- Select an option with a documented percentage surcharge (e.g., "+15%"). Verify the displayed price reflects exactly base price × 1.15, rounded according to the shop's rounding rules.
- Select an option with a percentage discount (e.g., "–10%"). Verify the displayed price is exactly base price × 0.90.
- Change the quantity to 3 with a percentage surcharge option active. Verify the total is calculated as (base price × surcharge multiplier) × quantity, not base price × quantity + flat surcharge.
- Stack a percentage surcharge option with a percentage coupon in the cart. Verify the calculation order matches the stated business rule (e.g., surcharge applied first, coupon applied to the result or to the base price).
- Verify the percentage calculation is consistent across all currency-display points: product page, cart, checkout summary, order confirmation, and invoice.
- On a product with a very low base price (e.g., €0.99), apply a percentage surcharge and verify the resulting price is rounded correctly and not shown as a non-sensical value.
- Switch between multiple percentage-changing options rapidly. Verify there is no race condition in the price display; the final displayed price always matches the currently selected option.

---

### Color selection (swatches)

- Verify that all available color swatches are rendered and visually correct (correct color or texture displayed in each swatch).
- Click each color swatch one by one. Verify the selected swatch receives a clear active indicator (border, checkmark, ring) and only one swatch can be active at a time.
- Select a color and verify the product main image (and gallery thumbnails if applicable) update to show the product in the chosen color.
- Hover over a color swatch that has a name (e.g., "Midnight Blue"). Verify a tooltip or accessible label appears with the color name.
- Select a color swatch that is marked as unavailable (e.g., crossed out, greyed out). Verify the system either prevents selection and shows an explanation, or selects it and clearly communicates that the item is out of stock.
- On mobile, tap each color swatch. Verify swatches are large enough for reliable tap input (minimum 44×44 px) and the selected state is clearly visible on a small screen.
- Add the product with a specific color selected to the cart. Open the cart and verify the selected color name is shown in the line item.
- Verify that color swatches for out-of-stock options are visually distinct (e.g., diagonal strike-through, reduced opacity) but the color is still identifiable.
- If the shop supports "notify when available" for out-of-stock color variants, clicking an unavailable swatch should present the notification signup option.

---

### Size selection

- Verify that all documented sizes are present in the size selector and none are duplicated.
- Select each size value. Verify the availability status (in stock / low stock / out of stock) updates to reflect the inventory for that specific size.
- Select a size and verify the price updates if size is a factor in pricing (e.g., larger sizes cost more).
- Attempt to add a product without selecting a size when size is a required option. Verify the system blocks the action and prompts size selection.
- Select a size that is out of stock. Verify the "Add to cart" button is disabled or replaced with an alternative action (e.g., "Notify me"), and a clear out-of-stock message is shown.
- If a size guide link is present, click it. Verify it opens the size guide (modal, new tab, or overlay) without losing the current product configuration.
- Add the product with size "L" to the cart, then return to the product page and add it again with size "M". Verify the cart contains two separate line items differentiated by size.
- Verify that size labels use the shop's standard notation consistently (e.g., all in S/M/L/XL or all numeric) and are not mixed within a single product.
- On mobile, verify the size selector (dropdown or button grid) is usable with touch input and the selected value is clearly visible after selection.

---

### Material selection

- Verify all available materials are listed in the material selector with accurate names.
- Select each material option one by one. Verify the product image or preview updates if material-specific images are configured.
- Select a material that carries a price surcharge. Verify the price updates immediately and the surcharge is labeled.
- Select a material that is unavailable. Verify it is visually distinguished and selection either does not change the active material or clearly shows an out-of-stock state.
- Add a product with a specific material to the cart. Verify the material name appears in the cart line item and in the order confirmation.
- If the product page contains a material description or tooltip, select each material and verify the corresponding description updates accurately.
- Combine material selection with size and color selection. Verify all three selected attributes are independently stored and all appear correctly in the cart.
- Verify the material option does not reset when another option (size, color) is changed; previously selected values should persist unless a dependency rule forces a change.

---

### Text field input (engraving, personalization)

- Locate a product with a text field option (e.g., "Engraving text"). Enter a standard ASCII string. Verify it is accepted and visible in the input field.
- Enter a string that exceeds the documented character limit. Verify the field enforces the limit (truncates, prevents input, or shows an error) and does not silently cut off characters that the user cannot see.
- Enter special characters and symbols (e.g., `& < > " ' / \ # @`). Verify the text is stored and displayed safely without breaking layout or triggering XSS.
- Enter emoji or non-Latin characters (e.g., Chinese, Arabic, umlauts). Verify correct handling: either accepted and displayed, or rejected with a clear error message.
- Leave the text field empty when it is a required option and attempt to add to cart. Verify the system blocks the action and highlights the field.
- Leave the text field empty when it is optional. Add to cart. Verify the product is added without the personalisation option in the line item.
- Add a product with personalisation text to the cart. Open the cart and verify the exact text entered by the user is displayed in the line item.
- Verify the personalisation text appears in the order confirmation email and in the admin order detail view.
- Enter only whitespace characters in the text field. Verify the system either trims and treats it as empty, or rejects it with an appropriate message.
- Verify the text field has an appropriate label, placeholder, and character counter (if applicable) so the user understands what is expected.

---

### File upload (custom design)

- On a product that allows file upload, upload a supported file type (e.g., JPG, PNG, PDF) within the allowed file size. Verify the upload completes successfully and a confirmation or thumbnail is shown.
- Attempt to upload a file type that is not permitted (e.g., .exe, .php). Verify the upload is rejected with a clear error message listing allowed types.
- Attempt to upload a file that exceeds the maximum allowed file size. Verify the upload is rejected before or during upload with a clear size-limit message.
- Upload a valid file, then attempt to replace it by uploading a second file. Verify the first file is replaced (not stacked) and the new file is shown.
- Remove an uploaded file using the delete/remove control. Verify the file is removed and the field returns to its empty state.
- Attempt to add the product to cart without uploading a file when upload is required. Verify the system blocks the action and prompts the user to upload.
- Verify the uploaded file is securely handled: the file should not be accessible at a predictable public URL, and the filename should be sanitised to prevent path traversal or injection.
- Verify the upload field is usable on mobile (the device's file picker opens when tapping the upload button).
- Upload an image file. Verify a preview thumbnail is shown so the customer can confirm they uploaded the correct file.
- Verify the uploaded file reference appears in the order confirmation and in the admin backend so the merchant can access the file for production.

---

### Configurator logic (dependencies between options)

- Identify a product with at least one dependency rule (e.g., selecting "Leather" material only shows certain colors). Select the triggering option and verify the dependent option set updates correctly.
- Select a combination that is documented as valid. Verify all fields remain active and no false-out-of-stock or error state appears.
- Select a combination that is documented as invalid. Verify the invalid dependent option is automatically deselected or hidden, and an informational message is shown if relevant.
- Step through all documented option dependency paths systematically and verify each produces the correct downstream option state.
- Verify that when a parent option changes and causes a child option to be hidden/reset, the hidden child option's value does not persist silently and get added to the cart.
- Rapidly change a parent option multiple times. Verify the dependent options always reflect the last selection and do not get stuck in an intermediate state due to a race condition.
- Verify dependency logic works identically on mobile and desktop.
- After triggering a dependency change, verify the price updates to reflect the new valid combination.
- If a dependency resets a previously selected child option, verify the user is notified (e.g., "Your previous selection for X has been updated").

---

### Unavailable combinations grayed out

- Identify a product where some option combinations are unavailable. Select an option that makes certain other options unavailable. Verify those unavailable options become visually grayed out or crossed out.
- Verify grayed-out options cannot be selected by clicking or keyboard interaction.
- Hover over or focus a grayed-out option. Verify an explanation is shown (e.g., "Not available in combination with [selected option]").
- Select an option combination that leaves the current secondary selection unavailable. Verify the secondary option is automatically deselected and a neutral/default state is shown.
- Verify that grayed-out options are still readable (accessible contrast) despite being visually muted.
- Verify that options grayed out due to stock (versus configuration dependency) are distinguished from those grayed out due to incompatibility, if the shop supports both states.
- Select a previously unavailable combination after deselecting the conflicting primary option. Verify the previously grayed option becomes active and selectable again.
- On mobile, verify grayed-out states are visually clear given smaller screen and touch interaction constraints.

---

### Real-time price update

- Select an option that changes the price. Verify the price on the page updates within 1 second without requiring a page reload.
- Rapidly cycle through all options for a product. Verify the price display always shows the correct value for the currently active selection and never shows a stale price.
- Change the quantity while a price-modifying option is selected. Verify the total price (unit price × quantity) updates in real time to reflect both the option and the new quantity.
- Verify that the price update is reflected in all price display elements on the page simultaneously (e.g., both a sticky add-to-cart bar and the main price block).
- Simulate a slow network connection (browser dev tools throttle to "Slow 3G"). Select an option and verify either the price updates correctly once the response arrives, or a loading indicator is shown during the delay, preventing the user from seeing a stale price as final.
- Verify there is no visible price flicker (brief display of wrong price) between the user's selection and the updated value.
- Verify the VAT and base price (if displayed separately) also update in real time when the total price changes.
- Open the product page in two browser tabs. Change an option in one tab. Verify the other tab is not affected (each tab operates independently).

---

### Volume discounts / tiered pricing

- Set the quantity to each tier threshold defined for a product (e.g., 1, 5, 10, 20) and verify the unit price changes to the correct tiered rate at each threshold.
- Set the quantity to one below a tier threshold. Verify the higher-tier discount does not apply prematurely.
- Set the quantity to exactly the threshold. Verify the discount applies.
- Verify the tiered pricing table or explanation (if displayed on the product page) accurately reflects the actual discounts applied as quantity is changed.
- Add a quantity that qualifies for a volume discount to the cart. Open the cart and verify the discounted unit price is reflected in the line item, not the base price.
- Change the quantity in the cart below the tier threshold. Verify the price reverts to the higher unit price and the cart total recalculates correctly.
- Verify that volume discounts are not stacked with individual coupon codes unless the business rules explicitly allow it; if stacking is prohibited, verify the system applies only the better discount.
- Combine a volume discount product with a non-discounted product in the cart. Verify the volume discount applies only to the eligible product line item.
- Verify the order confirmation and invoice reflect the tiered unit price actually charged, not the base price.

---

### Bundle products / sets

- Navigate to a bundle product page. Verify all component products are listed with correct names, images, and individual prices (if shown).
- Verify the bundle price is displayed and equals the documented discount versus buying components individually (or is clearly labeled as a fixed bundle price).
- Add the bundle to the cart. Verify it appears as a single bundle line item (or as component items grouped under the bundle), not as separate unlinked individual items.
- Attempt to change the quantity of the bundle to 2. Verify all component quantities update accordingly (i.e., 2 bundles = 2× all components).
- Verify that bundle components that are out of stock either prevent the bundle from being added or show a clear availability warning.
- If the bundle allows customisation of individual components (e.g., "Choose your color for item 1"), verify each component's configurator functions independently and all selections are captured in the cart.
- Remove the bundle from the cart. Verify all component items are removed together and not left as orphaned items.
- Verify the order confirmation lists the bundle and all its components clearly so the customer knows exactly what they ordered.
- Apply a coupon to a cart containing a bundle. Verify the discount calculation follows the stated business rule (discount on bundle price or on individual components).
- Verify the bundle product page has the correct structured data (Product schema with aggregateOffer or itemListElement as appropriate).

---

### Weight calculation on option change

- Select an option documented to change product weight (e.g., additional component, heavier material). Verify the displayed weight updates if weight is shown on the product page.
- Verify that the shipping cost calculation in the cart reflects the correct weight for the selected options; if a heavier option pushes the order into a higher shipping tier, the shipping cost should increase.
- Select multiple weight-affecting options simultaneously. Verify the displayed weight equals the sum of base weight plus all active option weight additions.
- Add a product with a weight-increasing option and a product with no weight option to the cart. Verify total cart weight is calculated correctly and the correct shipping method pricing applies.
- Change the quantity of a weight-affected product. Verify the total weight scales linearly (e.g., 3 items of a 500 g product = 1,500 g) and shipping costs update accordingly.
- Verify that weight is not shown to the customer if the shop does not display it, but the shipping cost calculation still uses the correct weight internally (verify via shipping cost change at tier boundaries).

---

### Preview image on option change

- Select each available option value (color, material, size, etc.) one by one. Verify the main product image updates to show the product as configured with that option.
- Verify the gallery thumbnails also update when an option is selected, showing only the images relevant to the current configuration.
- Rapidly switch between option values. Verify the image always reflects the last selected option and does not get stuck on a previous image.
- Select an option for which no specific image is configured. Verify a sensible fallback image is shown (e.g., default product image) rather than a broken image or blank space.
- On mobile, select an option and verify the image updates without triggering an unwanted page scroll or layout shift.
- Verify that image transitions (if animated) complete correctly and do not leave the image in a half-loaded state when options are changed quickly.
- Open the product page and verify the initial image shown corresponds to the pre-selected or default option, not a random image from the gallery.
- After selecting an option and viewing the updated image, open the image in the lightbox or zoom view. Verify the zoomed/lightbox image also corresponds to the currently selected option, not the base product image.
