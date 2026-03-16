# E-Commerce Shop Relaunch — Complete Test Plan

> Goal: Complete listing of all testable areas, pages, features, and scenarios.
> Status: Complete — 53 categories, 727 test points (own knowledge + online research)

---

## 1. Homepage

- Hero banner / slider display
- Category overview / navigation
- Product highlights / bestsellers
- Current offers / sale banners
- Search field display + functionality
- Newsletter signup
- Trust elements (badges, reviews, USPs)
- Footer (links, contact, social media, payment method icons)
- Cookie banner / consent management
- Load time / performance
- Above-the-fold content correct
- Responsive layout (mobile, tablet, desktop)
- SEO meta tags (title, description, OG tags)
- Structured data (Schema.org)
- Breadcrumbs
- Language switcher (if multilingual)

## 2. Navigation / Header

- Main menu all links working
- Mega menu / dropdown menus
- Mobile hamburger menu
- Sticky header behavior on scroll
- Logo links to homepage
- Cart icon with counter
- Wishlist icon
- Account icon / login link
- Search field in header
- Category tree fully navigable
- Breadcrumb navigation on all pages

## 3. Category Pages

- Category overview display
- Subcategory navigation
- Product list correct display
- Product images load correctly
- Price display (incl. RRP, discount, unit price)
- Sorting (price ascending/descending, popularity, newest, name, rating)
- Filtering (price range, color, size, material, brand, rating)
- Filter combinations (multiple filters simultaneously)
- Reset filters
- Pagination / infinite scroll
- Products per page (change quantity)
- Grid view / list view toggle
- Empty category display
- Category description text (SEO)
- Category image / banner
- Quick view
- Add to wishlist
- Add to cart directly from category page

## 4. Product Page

- Product images (gallery, zoom, lightbox)
- Image switching (thumbnails, slider)
- Product name correct
- Price display (unit price, tiered pricing, base price)
- VAT notice
- Shipping cost notice
- Availability status (in stock, low stock, out of stock)
- Delivery time display
- Product description (short + long)
- Technical specifications
- Product variants (size, color, material, etc.)
- Variant change updates price
- Variant change updates image
- Variant change updates availability
- Quantity selector
- Quantity limits (min/max)
- Add to cart button
- Buy now button (if applicable)
- Wishlist button
- Product comparison
- Reviews display
- Submit a review
- Review with images
- Average rating / stars
- Ask a question about the product
- Social sharing buttons
- Tabs (description, reviews, FAQ, shipping)
- Similar products / cross-selling
- Accessories / upselling
- Recently viewed products
- Breadcrumbs
- Structured data (Product Schema, reviews)
- Canonical URL
- SEO meta tags

## 5. Product Options / Configurator

- Simple options (dropdown, radio, checkbox)
- Required options vs. optional options
- Options with price surcharge
- Options with price reduction
- Percentage-based price changes
- Color selection (swatches)
- Size selection
- Material selection
- Text field input (engraving, personalization)
- File upload (custom design)
- Configurator logic (dependencies between options)
- Unavailable combinations grayed out
- Real-time price update
- Volume discounts / tiered pricing
- Bundle products / sets
- Weight calculation on option change
- Preview image on option change

## 6. Search

- Search field input + submit
- Autocomplete / suggest
- Search results display
- Search results sorting
- Search results filtering
- No results display
- Typo tolerance (fuzzy search)
- Synonyms
- Search by article number / SKU
- Search by category
- Search by brand
- Search term highlighting in results
- Search history
- Popular search terms
- Empty search handling
- Special characters in search
- XSS protection on search input

## 7. Shopping Cart

- Add product (from product page)
- Add product (from category page)
- Cart display (mini cart / sidebar)
- Full cart page
- Change quantity
- Set quantity to 0 / remove product
- Remove individual product
- Clear cart
- Price calculation correct (unit price x quantity)
- Subtotal correct
- VAT calculation and display
- Shipping cost preview
- Enter coupon code
- Remove coupon code
- Multiple coupons (if allowed)
- Coupon validation (expired, minimum order value, already used)
- Cross-selling in cart
- Cart persistence (after closing browser)
- Cart merge (guest → login)
- Empty cart display
- Continue shopping button
- Proceed to checkout button
- Weight / dimensions display
- Delivery time display per item
- Mark unavailable items in cart
- Maximum order quantity per product
- Minimum order value

## 8. Checkout

### 8.1 Address Entry
- Billing address form (all fields)
- Different shipping address
- Address validation (postal code format, required fields)
- Address autocomplete
- Business address (company name, VAT ID)
- Parcel locker / post office
- Country selection
- Saved addresses (logged-in customer)
- Guest checkout (without account)

### 8.2 Shipping Options
- Shipping methods display (standard, express, pickup)
- Shipping costs correctly calculated
- Free shipping above amount X
- Delivery time display per shipping method
- Preferred delivery date (if available)
- Bulky goods surcharge
- Island surcharge
- International shipping costs
- Click & collect / in-store pickup

### 8.3 Payment Methods
- Credit card (Visa, Mastercard, Amex)
- PayPal
- PayPal Express
- Klarna (invoice, installments, instant)
- Amazon Pay
- Apple Pay
- Google Pay
- SEPA direct debit
- Bank transfer / prepayment
- Cash on delivery
- Purchase on invoice
- Installment payments
- Payment method selection UI
- Payment method validation
- 3D Secure / SCA
- Failed payment (handling + error message)
- Payment timeout
- Prevent double payment

### 8.4 Order Summary
- All items correctly listed
- Prices correct
- Shipping costs displayed
- VAT breakdown
- Coupon discount displayed
- Total amount correct
- Terms & conditions checkbox
- Cancellation policy checkbox
- Privacy policy checkbox
- Place order button
- Double-submit protection

### 8.5 Order Confirmation
- Thank-you page display
- Order number displayed
- Order summary
- Confirmation email sent
- Confirmation email content correct
- Redirect after payment (redirect URLs)
- Tracking pixel / conversion tracking

## 9. Express Checkout

- Apple Pay button on product page
- Google Pay button on product page
- PayPal Express button on product page
- Express checkout in cart
- Express checkout on checkout page
- Address transfer from wallet
- Shipping cost calculation in express flow
- Cancel express checkout → return to normal flow
- Payment intent creation
- Payment intent update on changes

## 10. Customer Account

### 10.1 Registration
- Registration form
- Required fields validation
- Email format validation
- Password strength requirements
- Password confirmation
- Privacy policy checkbox
- Newsletter opt-in
- Double opt-in email
- Confirmation link works
- Already registered email handling
- Social login (Google, Facebook, Apple)
- Registration during checkout

### 10.2 Login
- Login with email + password
- Login with username (if supported)
- Wrong credentials error message
- Account locked after X failed attempts
- "Forgot password" flow
- Password reset email
- Password reset link expiry
- Set new password
- "Stay logged in" / remember me
- Session timeout
- Login redirect (back to previous page)
- Social login
- Two-factor authentication (if available)

### 10.3 Customer Account Area
- Dashboard / overview
- Edit personal data
- Change email address
- Change password
- Address book (add, edit, delete addresses)
- Set default address
- Order history / order overview
- View individual order (details)
- Reorder
- Download invoice (PDF)
- Shipment tracking
- Initiate return
- View wishlist
- Newsletter settings
- Delete account (GDPR)
- Logout

## 11. Wishlist

- Add product to wishlist
- Remove product from wishlist
- View wishlist
- Move from wishlist to cart
- Share wishlist (link, email)
- Wishlist as guest (cookie-based)
- Wishlist as logged-in user (persistent)
- Transfer wishlist after login

## 12. Reviews / Ratings

- Submit review (stars + text)
- Review with image/video
- Reviews only for verified buyers
- Review moderation (not immediately visible)
- Edit review
- Delete review
- Sort reviews (newest, best, worst)
- Filter reviews (by stars)
- Average rating correctly calculated
- Reviews on product page
- Reviews on category page (stars)
- Review schema (rich snippets)

## 13. Coupons / Discounts / Promotions

- Percentage coupon
- Fixed amount coupon
- Free shipping coupon
- Minimum order value coupon
- Single-use vs. multi-use coupon
- Coupon limited per customer
- Time-limited coupon (expiry date)
- Coupon for specific products/categories
- Coupon stacking (multiple simultaneously)
- Automatic discounts (e.g., 10% off from 3 items)
- Volume discounts / tiered pricing
- Sale prices / strikethrough prices
- Bundle discounts
- New customer discount
- Loyalty points / rewards program
- Gift certificates (purchase + redeem)

## 14. CMS / Content Pages

- Legal notice / imprint
- Privacy policy
- Terms & conditions
- Cancellation policy
- Shipping information
- Payment methods page
- FAQ / frequently asked questions
- About us
- Contact page
- Contact form (validation + submission)
- Blog / guides
- Blog single page
- Blog categories
- Blog search
- Landing pages
- 404 error page
- Maintenance page

## 15. Contact Options

- Contact form validation
- Contact form submission
- Contact form confirmation email
- Phone number clickable (mobile)
- Email address clickable
- Live chat widget
- WhatsApp button
- Social media links
- FAQ / self-service

## 16. Transactional Emails

- Registration confirmation
- Double opt-in email
- Password reset email
- Order confirmation
- Payment confirmation
- Shipping confirmation with tracking link
- Delivery confirmation
- Invoice email (with PDF)
- Review request (after delivery)
- Cart reminder (abandoned cart)
- Wishlist reminder
- Newsletter welcome email
- Newsletter unsubscribe confirmation
- Coupon email
- Return confirmation
- Refund confirmation
- Account deletion confirmation
- Email rendering (desktop, mobile, dark mode)
- Email links working
- Email sender / reply-to correct
- SPF/DKIM/DMARC correct
- Spam score

## 17. SEO

- Title tags on all pages
- Meta descriptions on all pages
- H1 structure (exactly one per page)
- Heading hierarchy (H1 > H2 > H3)
- Canonical URLs
- Hreflang tags (multilingual)
- XML sitemap present + correct
- Robots.txt correct
- 301 redirects from old site
- No 404 errors on important URLs
- Image alt texts
- Internal linking
- Breadcrumbs
- Structured data (Organization, Product, Review, FAQ, Breadcrumb)
- Open Graph tags
- Twitter Cards
- PageSpeed / Core Web Vitals (LCP, FID, CLS)
- Mobile-friendliness
- URL structure (clean URLs)
- Trailing slashes consistent
- Index/noindex correctly set
- Pagination (rel=next/prev or alternatives)

## 18. Performance / Load Times

- Homepage load time
- Category page load time
- Product page load time
- Checkout load time
- Time to First Byte (TTFB)
- Largest Contentful Paint (LCP)
- First Input Delay (FID) / Interaction to Next Paint (INP)
- Cumulative Layout Shift (CLS)
- Image optimization (WebP, AVIF, lazy loading)
- CSS/JS minification
- HTTP/2 or HTTP/3
- Gzip/Brotli compression
- Browser caching correct
- CDN working
- Database queries optimized
- Server response time
- Load time under load (concurrent users)
- Third-party scripts impact

## 19. Mobile / Responsive

- All pages correct on mobile
- All pages correct on tablet
- Touch targets large enough (min. 44x44px)
- Swipe gestures (image gallery, slider)
- Mobile navigation (hamburger menu)
- Mobile checkout optimized
- Mobile payment (Apple Pay, Google Pay)
- Landscape + portrait mode
- Pinch-to-zoom on product images
- Sticky add-to-cart on mobile
- Mobile keyboard (numeric for postal code, email keyboard for email)
- Forms usable on mobile
- Popups closable on mobile
- Viewport meta tag correct

## 20. Cross-Browser Compatibility

- Chrome (desktop + mobile)
- Firefox (desktop + mobile)
- Safari (desktop + mobile / iOS)
- Edge (desktop)
- Samsung Internet (mobile)
- Opera (optional)
- Various OS versions (iOS 15+, Android 10+)
- Various screen resolutions

## 21. Accessibility (a11y)

- Keyboard navigation (Tab, Enter, Escape)
- Focus styles visible
- Screen reader compatible (ARIA labels)
- Alt text on all images
- Color contrast sufficient (WCAG AA)
- Font size adjustable
- Form labels correctly linked
- Error messages screen reader accessible
- Skip-to-content link
- No purely visual information (e.g., color only = status)
- Video subtitles (if videos present)

## 22. Security

- HTTPS on all pages
- HSTS header
- Content Security Policy
- XSS protection (input fields, URL parameters)
- SQL injection protection
- CSRF protection (forms)
- Brute force protection (login)
- Rate limiting (API, login)
- Secure password hashing
- Session management (timeout, fixation)
- Cookie flags (Secure, HttpOnly, SameSite)
- File upload validation
- Admin area secured
- PCI-DSS compliance (payment data)
- No sensitive data in URLs
- Error pages without sensitive info (no stack trace)
- Dependency scanning (known vulnerabilities)
- Robots.txt does not reveal admin URLs

## 23. GDPR / Data Protection / Legal

- Cookie consent banner
- Cookie settings changeable
- Only necessary cookies without consent
- Analytics only after consent
- Marketing cookies only after consent
- Privacy policy complete + up to date
- Legal notice / imprint complete
- Terms & conditions current
- Cancellation policy + sample form
- Right to data deletion (delete account)
- Data export (GDPR Art. 20)
- Newsletter double opt-in
- Checkbox texts correctly worded
- No pre-checked checkboxes (opt-in)
- Data processing agreements (third-party)
- Cookie durations documented

## 24. Analytics / Tracking

- Google Analytics 4 integrated
- GA4 events correct (page_view, view_item, add_to_cart, purchase)
- Enhanced e-commerce tracking
- Conversion tracking (Google Ads, Meta Pixel)
- Google Tag Manager correct
- Server-side tracking (if applicable)
- Consent Mode v2 (Google)
- Facebook/Meta Pixel
- Pinterest Tag (if relevant)
- Heatmap tool (Hotjar, Clarity)
- A/B testing tool
- Tracking disabled after consent rejection
- Revenue tracking correct (currency, amounts)
- Funnel tracking (checkout steps)

## 25. APIs / Integrations

- Payment gateway APIs (Stripe, PayPal)
- Shipping provider API (DHL, DPD, GLS, Hermes, UPS, FedEx)
- ERP integration
- Inventory management sync
- Accounting export
- Product feed (Google Shopping, Meta)
- Warehouse management sync
- CRM integration
- Email service provider (newsletter)
- Review platform API (Trustpilot, Ekomi)
- Price comparison portal feed
- Marketplace integrations (Amazon, eBay)
- Webhook processing
- API rate limits
- API error handling
- API retry logic

## 26. Admin Backend

- Admin login / authentication
- Dashboard overview
- Order management
- Change order status
- Create invoice
- Create delivery note
- Manage returns
- Process refunds
- Product management (CRUD)
- Category management
- Customer management
- Coupon management
- CMS page management
- Media management (images, files)
- Settings (shop, shipping, payment, taxes)
- User / role management
- Export functions (CSV, PDF)
- Import functions
- Statistics / reports
- Log viewer

## 27. Internationalization (if applicable)

- Multi-language support (texts, URLs)
- Currency conversion
- Country-specific prices
- Country-specific shipping costs
- Country-specific payment methods
- Country-specific tax rates
- Date format country-specific
- Address format country-specific (postal code field, etc.)

## 28. Infrastructure / DevOps

- SSL certificate valid
- DNS correctly configured
- Server monitoring
- Error logging (Sentry, Bugsnag)
- Backup strategy
- Deployment pipeline
- Staging environment available
- Rollback capability
- Auto-scaling (during traffic spikes)
- Cron jobs / scheduled tasks
- Queue system (emails, jobs)
- Redis / cache layer

## 29. Edge Cases / Special Scenarios

- Product becomes unavailable during checkout
- Price changes during checkout
- Session expires during checkout
- Browser back button in checkout
- Double-click on "Place Order"
- Very long product name
- Very many variants (100+ options)
- Empty cart → go directly to checkout
- Cart with 50+ items
- Order with 0 EUR (coupon covers everything)
- Coupon higher than order value
- Special characters in names/addresses (umlauts, accents)
- Very long addresses
- Simultaneous purchase of same item (race condition)
- Slow internet connection
- Checkout abandonment + resumption
- Multiple tabs open simultaneously
- Incognito mode
- Cookies disabled
- JavaScript disabled (graceful degradation)
- Ad blocker active

## 30. Third-Party Integrations

- Google Maps (store locator, contact page)
- reCAPTCHA / spam protection
- Trusted Shops / buyer protection badges
- Review widgets (Trustpilot, Google Reviews)
- Social media embeds
- YouTube/Vimeo videos
- Chat widget (Zendesk, Intercom, Tidio)
- Push notifications
- Affiliate tracking
- Retargeting pixels

## 31. Returns / Refunds / Cancellations

- Cancellation during order processing (before shipping)
- Cancellation after shipping / during delivery
- Return request by customer (self-service portal)
- RMA process (Return Merchandise Authorization)
- Return label generation and download
- Refund to original payment method
- Partial refund for partial return
- Store credit instead of refund
- Exchange workflow (different product/variant)
- Return period check (automatic rejection after deadline)
- Status updates for return via email/SMS

## 32. Order Status Lifecycle / Order Management

- Complete status workflow: Received > Paid > Processing > Shipped > Delivered
- Partial shipment (multiple packages for one order)
- Backorder / reorder on supply shortage
- Order status page for customers (without login, via link)
- Address change on pending order
- Prevention of duplicate orders (double-submit protection)
- Merge multiple orders
- Reorder from order history

## 33. Shipping & Delivery (detailed)

- Shipping zone configuration (domestic, EU, international)
- Shipping cost calculation by weight/size/destination
- Free shipping above threshold (display + correct calculation)
- Shipment tracking integration (DHL, DPD, GLS, etc.)
- Delivery time estimate on product page and in checkout
- Special destinations: parcel lockers, PO boxes, islands, rural addresses
- Express shipping surcharge correctly calculated
- Click & collect / in-store pickup
- Delivery notifications (email/SMS on shipment, delivery)

## 34. Tax & Price Calculation

- VAT calculation for different countries (DE 19%, AT 20%, CH 0%, etc.)
- Net/gross toggle (B2B vs. B2C)
- Rounding logic with discounts + VAT combination
- Price consistency: product page = cart = checkout = confirmation
- Currency formatting (comma vs. dot, symbol position)
- Tiered pricing / volume discounts correctly calculated
- Price display for configurable products (dynamic update)
- Strikethrough prices / RRP correctly displayed
- Tax disclosure on invoice (mandatory information)

## 35. Invoices & Documents

- PDF invoice generation and download
- Delivery note generation
- Correct mandatory information on invoice (tax number, VAT ID, address)
- Invoice accessible in customer account
- Send invoice via email
- Credit note document for returns
- Sequential and correct invoice numbers

## 36. Inventory Management

- Real-time inventory synchronization
- "Sold out" display when stock = 0
- "Only X left in stock" notice for low stock
- Prevention of overselling (concurrency: 2 users buy last item simultaneously)
- Stock reservation during checkout (temporary lock)
- Automatic stock reduction after purchase
- Stock increase after cancellation/return
- "Notify when available" feature
- Variant-specific stock (color/size has own inventory)

## 37. Session Management & Cart Persistence

- Cart persists after closing browser (logged in)
- Cart merging: guest cart + login cart combined
- Session timeout behavior (checkout form data loss?)
- Simultaneous sessions on multiple devices
- Cart abandonment trigger (timing of "left" detection)
- Cart contents after password reset
- Cookie expiry and session recovery

## 38. Error Handling & Graceful Degradation

- Behavior on payment gateway outage (fallback or error message)
- Behavior on database connection loss
- Behavior on external API timeout (shipping, tax, etc.)
- User-friendly error messages (no stack trace, no technical details)
- 404 page with search function and navigation help
- 500 error: automatic admin notification
- Form recovery after error (inputs not lost)
- Circuit breaker pattern for external services
- Offline tolerance / service worker caching

## 39. Fraud Prevention

- Bot detection on login (brute force protection)
- Rate limiting on form submissions
- CAPTCHA/reCAPTCHA at critical points (login, registration, checkout)
- Honeypot fields in forms
- Credit card testing detection (many failed payments)
- IP-based anomaly detection
- Address Verification System (AVS) for credit cards
- Velocity checks (unusually many orders per IP/account)

## 40. Notification System (beyond email)

- Push notifications (browser/web push)
- SMS notifications (shipping, delivery)
- In-app notifications in customer account
- Webhook reliability (retry on failure)
- Notification preferences (opt-in/opt-out per channel)
- Price change notification (wishlist)
- Back-in-stock notification
- Order status updates via WhatsApp/Telegram

## 41. Personalization & Recommendations

- "Recently viewed" widget correctly populated
- "Similar products" recommendations relevant
- "Customers also bought" logic
- "Frequently bought together" bundles
- Personalized homepage based on browsing history
- Recommendation widgets on 404 page and empty search results page
- A/B testing capability for recommendation algorithms

## 42. Social Commerce & Sharing

- Social media share buttons on product pages
- Open Graph tags (og:title, og:image, og:description) correctly populated
- Twitter Card tags correct
- Social login (Facebook, Google, Apple)
- Share preview in social networks (image + text correct?)
- Social proof: display "X people are viewing this product right now"
- Instagram Shopping / Facebook Shop integration

## 43. Loyalty Program & Subscriptions

- Points earned on purchase correctly calculated
- Points redeemable at checkout
- Points balance visible in customer account
- Points expiry/decay
- Subscription orders (recurring delivery)
- Subscription management (change interval, pause, cancel)
- Recurring payment for subscriptions
- Referral program (invitation link tracking)

## 44. Bulk Operations & Import/Export

- CSV/Excel product import (prices, descriptions, images)
- Product export for marketplaces (Google Shopping, idealo)
- Bulk product editing (price changes, status)
- Data validation on import (report faulty rows)
- Import of customer data / order data (migration)
- Export of orders for accounting/ERP

## 45. Visual Regression Testing

- Screenshot comparison before/after code changes
- Consistency across different page types (PDP, PLP, cart, checkout)
- Font rendering and typography
- Color consistency and brand compliance
- Icon/button consistency
- Responsive breakpoints visual check
- Dark mode compatibility (if applicable)

## 46. Navigation & UX Patterns (detailed)

- Breadcrumb navigation correct and complete
- Back button behavior (browser history correct?)
- Pagination vs. "Load more" vs. infinite scroll
- Scroll position restoration after back navigation
- Sticky header/navigation behavior on scroll
- Footer links (all functional, no dead links)
- Mega menu behavior on desktop and mobile
- Keyboard navigation (tab order logical)
- Skip-to-content link for screen readers

## 47. Images & Media

- Broken image detection (missing product images)
- Alt text for all product images present and descriptive
- Lazy loading correct (above-the-fold NOT lazy loaded)
- Image format optimization (WebP/AVIF with fallback)
- Product gallery: zoom, swipe, fullscreen
- Video embedding on product pages
- 360-degree product view (if applicable)
- Image size consistency in product lists
- Placeholder/skeleton during image loading

## 48. Live Chat & Customer Support Integration

- Chat widget loads and opens correctly
- Chat widget does not cover critical UI elements (CTA buttons!)
- Chatbot answers to standard questions correct
- Handoff chatbot → human agent works
- Chat history persists across page changes
- Chat widget usable on mobile
- Offline mode of chat (contact form instead of live chat)
- Help center / FAQ pages searchable

## 49. Disaster Recovery & Rollback

- Backup restoration tested (DB + files)
- Backup integrity check (no empty/corrupt backups)
- Rollback plan for failed deployment
- RPO/RTO defined and tested
- Failover on server outage
- Database replication working
- Automatic alerts on system outage

## 50. Data Migration & Validation (relaunch-specific)

- Product data fully migrated (title, description, prices, images)
- Customer data correct (addresses, order history, password hashes)
- Order history viewable and correct
- 301 redirects set up for all old URLs
- SEO metadata migrated (title, description, canonical)
- Category structure correctly mapped
- Customer reviews migrated
- Coupon/discount codes still valid
- Cart contents of existing customers (if possible)
- Record count comparison: source vs. target

## 51. Legal Compliance (detailed)

- Cookie consent banner appears on first visit
- Cookies set ONLY after consent (no pre-tracking)
- Cookie preferences changeable and saved
- Legal notice / imprint complete and accessible
- Terms & conditions linked and acceptable in checkout
- Cancellation policy correctly displayed
- Privacy policy linked and complete
- Age verification (if age-relevant products)
- Price Indication Regulation: base price, total price
- Cancellation form downloadable as PDF
- Button solution: "Order with obligation to pay" on order button

## 52. Load & Stress Tests (detailed)

- Spike tests: sudden traffic increase (e.g., newsletter send)
- Soak tests: sustained load over hours
- Checkout under load (parallel orders)
- Database performance with large catalogs (>10,000 products)
- CDN failover on CDN outage
- Auto-scaling tested (if cloud hosting)
- Recovery time after crash measurement

## 53. B2B-Specific Features (if applicable)

- Customer groups with different prices
- Tiered pricing / volume discounts
- Request for quote
- Purchase on invoice (payment terms 30/60/90 days)
- Approval workflows (order approval by supervisor)
- Customer numbers / reference numbers on orders
- Net price display
- Customer-specific catalogs/assortments
- Quick order by article number

---

## Sources (Online Research)

- [Katalon: 100 Test Cases for E-Commerce Website](https://katalon.com/resources-center/blog/test-cases-for-ecommerce-website)
- [BrowserStack: 60 Test Cases for E-Commerce Website](https://www.browserstack.com/guide/test-cases-for-ecommerce-website)
- [6amtech: 130+ Must-Have Test Cases for eCommerce](https://6amtech.com/blog/ecommerce-website-testing-and-test-cases-checklist/)
- [Shopify: E-Commerce Testing Guide 2026](https://www.shopify.com/blog/ecommerce-testing)
- [Maruti Techlabs: Guide to E-Commerce Website Testing](https://marutitech.com/guide-to-ecommerce-website-testing/)
- [TestGrid: 70 Test Cases for E-Commerce Websites](https://testgrid.io/blog/test-cases-for-ecommerce-website/)
- [TestGrid: Black Friday E-Commerce Testing Tips](https://testgrid.io/blog/black-friday-ecommerce-testing/)
- [bugbug.io: E-Commerce Testing Guide 2026](https://bugbug.io/blog/software-testing/ecommerce-testing/)
- [bugbug.io: Test Cases Best Practices and Templates](https://bugbug.io/blog/software-testing/test-cases-for-ecommerce-website/)
- [Lyssna: E-Commerce Website Testing Checklist](https://www.lyssna.com/guides/website-testing/ecommerce-website-testing-checklist/)
- [InstaWP: E-Commerce Migration Checklist (Go-Live + Post-Launch)](https://instawp.com/ecommerce-migration-checklist/)
- [Cloudways: Complete E-Commerce Migration Checklist 2026](https://www.cloudways.com/blog/ecommerce-migration-checklist/)
- [Eat Fresh Tech: 12-Point E-Commerce Disaster Recovery Plan](https://www.eatfresh.tech/blog/12-point-ecommerce-disaster-recovery-plan-checklist)
- [OWASP: Session Timeout Testing Guide](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout)
