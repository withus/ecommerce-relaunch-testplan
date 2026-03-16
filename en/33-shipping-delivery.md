# 33. Shipping & Delivery (detailed)

### Shipping zone configuration (domestic, EU, international)

- Verify domestic zone (e.g., Germany) is correctly isolated from EU zones — a DE postal code must not resolve to an EU-zone rate
- Test zone boundary: order shipping to a German address with a non-standard postal code (e.g., island postcodes 18565 Rügen, 25980 Sylt) — confirm correct zone assignment
- Add a product to cart, set shipping address to each EU country individually (AT, FR, PL, ES, IT) and confirm the zone label and rate displayed matches the configured EU zone
- Set shipping address to a non-EU international destination (US, CH, NO, GB) and verify it resolves to the correct "international" or country-specific zone, not the EU zone
- Test an address in a country that is geographically in Europe but outside EU customs (Switzerland, Norway) — confirm it maps to the international zone, not EU
- Remove a country from a configured shipping zone in the admin, then attempt an order to that country — confirm checkout blocks or shows "not deliverable" rather than silently applying wrong rates
- Configure overlapping zones (EU zone + specific country zone for France) and confirm the more specific rule takes precedence
- Verify that zone assignment uses the shipping address, not the billing address, when they differ
- Confirm that changing the shipping address country in checkout dynamically updates the shipping zone and recalculates costs without requiring page reload
- Test a country that has no configured shipping zone — confirm a clear error message ("We do not ship to this country") rather than a €0 or €NULL entry

### Shipping cost calculation by weight/size/destination

- Add a single product of exactly the lowest weight tier boundary (e.g., 0.99 kg vs. 1.00 kg) to cart and verify the correct rate bracket is applied in each case
- Add products whose combined weight crosses a tier boundary (e.g., items totaling 4.99 kg vs. 5.00 kg) and confirm cost jumps to the next tier at exactly 5.00 kg
- Test the maximum weight tier: add items totaling above the highest defined weight threshold and confirm either the highest rate applies or an "oversize — please contact us" message appears
- Configure dimensional weight (volumetric weight): add an oversized but light product and verify the system uses the greater of actual vs. dimensional weight for cost calculation
- Verify that weight shown in cart (displayed to customer) matches the weight used in the shipping cost calculation in the backend
- Add multiple items from different weight categories and confirm the combined weight total is used, not the highest individual weight
- Test shipping cost for a destination at the zone boundary where per-kg rates differ (e.g., DE flat rate vs. EU per-kg) — confirm correct formula is applied
- Add a product with weight = 0 (digital or misconfigured) and verify the system does not calculate a €0 shipping cost silently; confirm it either defaults to minimum rate or flags an error
- Combine a heavy physical product with a digital/zero-weight product — verify the physical weight alone drives the calculation
- Test that changing product quantity in cart (e.g., 1 → 3 units) proportionally increases total weight and updates shipping cost accordingly

### Free shipping above threshold (display + correct calculation)

- Set cart value exactly €1 below the free shipping threshold (e.g., €49.99 when threshold is €50.00) — confirm paid shipping is displayed and the "X more for free shipping" progress indicator shows €0.01 remaining
- Set cart value to exactly the threshold (€50.00) — confirm free shipping is applied, shipping line shows €0.00, and no paid shipping option is forced
- Set cart value above the threshold (€75.00) — confirm free shipping remains applied when adding more items
- Apply a percentage coupon that reduces the cart below the threshold (e.g., 20% off on a €60 cart → €48) — confirm free shipping is revoked and the appropriate shipping cost is added
- Apply a fixed-amount coupon that reduces the cart below the threshold — confirm same revocation behavior as above
- Set cart to above threshold, then reduce item quantity so it falls below — confirm free shipping is removed dynamically
- Verify that the free shipping threshold applies only to the configured zone: domestic threshold of €50 should not grant free shipping for EU or international orders
- Confirm that shipping cost (before free shipping applies) is excluded from the threshold calculation — i.e., it is based on product subtotal only, not subtotal + shipping
- Test that a "free shipping" coupon code bypasses the threshold check entirely and grants free shipping regardless of cart value
- Confirm that the free shipping progress bar / message updates in real time as the customer changes cart quantities without a full page refresh

### Shipment tracking integration (DHL, DPD, GLS, etc.)

- Place a test order and trigger a shipping notification; verify the tracking number stored in the order record is the exact number provided by the carrier API
- Click the tracking link in the shipping confirmation email and confirm it resolves to the correct carrier tracking page with the correct tracking number pre-filled in the URL
- Test tracking link for each configured carrier (DHL, DPD, GLS, Hermes/Evri, UPS, FedEx) — verify each uses the correct carrier-specific URL format
- Confirm tracking link in the customer account order detail page matches the tracking link in the email
- Simulate carrier API returning an invalid or empty tracking number — verify the system logs the error, does not send a broken link to the customer, and alerts the admin
- Verify that tracking status updates (e.g., "In transit", "Out for delivery", "Delivered") are pulled and reflected in the customer account order status page if the integration supports polling
- Test tracking for a split shipment (two packages for one order) — confirm both tracking numbers are stored and displayed separately
- Confirm that DHL Packstation deliveries include the correct post number in the carrier data passed to the API
- Test tracking number format validation: DHL tracking numbers are typically 20 digits starting with "00" — verify the system rejects obviously malformed tracking numbers before saving
- Verify that the tracking link is accessible without login (via order confirmation email link), so customers without accounts can track their orders

### Delivery time estimate on product page and in checkout

- On a product page, verify that the displayed delivery time (e.g., "2–3 business days") matches the configured estimate for the default shipping zone
- Change the shipping destination country in checkout and confirm the delivery time estimate updates to reflect the zone-specific transit time (e.g., DE: 2–3 days, EU: 5–7 days)
- Select Express shipping option and verify the delivery time estimate shortens to the configured express window
- Test estimated delivery when the current time is past the daily order cut-off (e.g., after 14:00) — confirm the estimate shifts by one business day
- Test estimated delivery when today is a public holiday — confirm the system skips the holiday and calculates the correct next-business-day start
- Add an item explicitly marked as "on backorder" (e.g., 4–6 weeks) and verify the delivery estimate shown is the backorder lead time, not the standard shipping time
- Verify that a product page shows "in stock — ships today" only when stock > 0 and the order is placed before the cut-off time
- Confirm the delivery time shown on the product page and the delivery time shown at checkout are consistent for the same product and destination
- Test delivery time estimate for a cart containing items with different lead times — confirm the displayed estimate is the longest lead time (worst case), not an average
- After completing an order, verify the delivery time stated in the order confirmation email matches what was shown during checkout

### Special destinations: parcel lockers, PO boxes, islands, rural addresses

- Enter a DHL Packstation address (Packstation 123, 12345 City) in the shipping address — confirm it is accepted and the carrier API receives the correct structured data including post number
- Enter a PO Box address — confirm it is either accepted for carriers that support PO Box delivery, or blocked with a specific error message (not a generic validation error)
- Enter an address on a German North Sea or Baltic island (e.g., 25980 Sylt, 18565 Rügen) — confirm the island surcharge is automatically added to the shipping cost
- Test island surcharge for German offshore islands (Helgoland, Wangerooge) and confirm each applies the correct surcharge amount as configured
- Enter a Canary Islands address (Spain, but different tax/customs zone) — verify it is not treated as standard EU, and the correct surcharge or restriction is applied
- Enter a remote rural UK postcode (e.g., Scottish Highlands, BT postcodes for Northern Ireland) — confirm carrier-specific remote area surcharge is applied where configured
- Enter an Azores or Madeira address (Portugal overseas territories) — verify correct zone assignment and any applicable surcharge
- Test a French overseas department address (e.g., Martinique, 97200) — confirm it is not treated as standard EU France
- Enter an address where the carrier does not deliver (e.g., military base, restricted zone) — confirm a clear message prevents order completion rather than creating an undeliverable shipment
- Test that parcel locker addresses are blocked for oversized or high-value items that exceed the locker's size/value limits, with a clear explanatory message to the customer

### Express shipping surcharge correctly calculated

- Select standard shipping, note the cost, then switch to express — verify the price difference matches the configured express surcharge exactly
- Add multiple items to cart and select express — verify the express surcharge is applied once (per shipment), not per item
- Test express shipping eligibility: add an item that is excluded from express (e.g., oversized/custom item) and verify the express option is either hidden or disabled with an explanatory note
- Confirm the express shipping delivery time estimate (e.g., "next business day by 12:00") is displayed alongside the surcharge during option selection
- Place an express order after the express cut-off time (e.g., after 12:00) — verify the system either rejects the express option or correctly shows the next-eligible delivery date
- Test express shipping for EU destination — confirm the EU express rate is applied, not the domestic express rate
- Apply a "free shipping" coupon and verify it does not eliminate the express surcharge (unless the coupon explicitly covers express)
- Verify that the express surcharge is shown as a line item separate from the base shipping cost in the order summary, confirmation email, and invoice
- Test express surcharge with a cart at the free shipping threshold — confirm that free shipping applies to the base cost but the express upgrade surcharge is still charged
- Test same-day delivery option (if configured): verify it is only offered for orders within the eligible geographic radius and placed before the same-day cut-off time

### Click & collect / in-store pickup

- Select "In-store pickup" as shipping method — verify shipping cost drops to €0.00
- Confirm that selecting in-store pickup hides or disables the shipping address form (since no delivery address is needed)
- If multiple pickup locations exist, verify the customer can select a specific store/location and the selected location is recorded on the order
- Verify the pickup confirmation email contains the store address, opening hours, and estimated ready date
- Test the "ready for pickup" notification: when order status is updated to "ready for collection" in the admin, confirm the customer receives the notification email/SMS
- Attempt to place a click & collect order for an item that is out of stock at the selected location but available at another — verify the system shows the correct stock status per location
- Test that the click & collect option is not offered when the cart contains an item that cannot be collected (e.g., requires home delivery due to size or hazmat restrictions)
- Verify that a pickup order does not trigger carrier API calls or generate a shipping label
- Test the order expiry flow: if an order is not collected within the defined window (e.g., 7 days), confirm the order is cancelled and the customer is notified
- Confirm that a logged-in customer's saved addresses are not pre-filled when in-store pickup is selected, preventing address-confusion errors

### Delivery notifications (email/SMS on shipment, delivery)

- Place an order and trigger shipment — verify the shipping confirmation email is sent within the configured time and contains the tracking number and clickable tracking link
- Verify the "out for delivery" notification (if configured) is sent only on the actual delivery day, not on intermediate transit days
- Verify the "delivered" notification is sent only after the carrier API confirms delivery status, not just when the order is marked shipped
- Test SMS notification (if enabled): confirm the SMS is sent to the correct phone number provided during checkout, not the billing contact number
- Verify notification preferences: if the customer has opted out of SMS but not email, confirm SMS is suppressed while email is still sent
- Test the notification for a failed delivery attempt — confirm the customer receives a "delivery attempted" message with instructions for rescheduling
- Verify split shipment notifications: for an order shipped in two packages, confirm two separate tracking emails are sent (one per package), each with the correct individual tracking number
- Confirm that notification emails use the customer's preferred language (if multilingual) and not the shop's default language
- Test that re-sending a shipping notification from the admin panel sends the email to the current customer email address (in case it was updated), not the address stored at order time
- Verify that transactional notification emails pass SPF/DKIM validation and are not flagged as spam; check the "from" address matches the configured sender domain
