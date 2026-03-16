# E-Commerce Shop Relaunch Test Plan

A QA test plan for the relaunch of a Laravel-based e-commerce shop. It covers 53 test areas, from homepage and checkout to security, GDPR, disaster recovery and load tests. The English test files in `en/` contain 6,240 individual test points.

## Background

The plan was written in 2026 to make the acceptance testing for the relaunch of a Laravel-based online shop systematic.

## Structure

```
├── testplan-komplett.md          # German master version (53 categories, 727 test points)
├── testplan-complete-en.md       # English translation (same structure)
└── en/                           # Detailed test point files (English)
    ├── 01-homepage.md
    ├── 02-navigation-header.md
    ├── ...
    └── 53-b2b-features.md
```

## Categories

| #  | Category                    | #  | Category                      |
|----|-----------------------------|----|-------------------------------|
| 01 | Homepage                    | 28 | Infrastructure & DevOps       |
| 02 | Navigation / Header         | 29 | Edge Cases                    |
| 03 | Category Pages              | 30 | Third-Party Integrations      |
| 04 | Product Page                | 31 | Returns & Refunds             |
| 05 | Product Options             | 32 | Order Lifecycle               |
| 06 | Search                      | 33 | Shipping & Delivery           |
| 07 | Shopping Cart               | 34 | Tax & Price Calculation       |
| 08 | Checkout                    | 35 | Invoices & Documents          |
| 09 | Express Checkout            | 36 | Inventory Management          |
| 10 | Customer Account            | 37 | Session & Cart Persistence    |
| 11 | Wishlist                    | 38 | Error Handling                |
| 12 | Reviews & Ratings           | 39 | Fraud Prevention              |
| 13 | Coupons & Discounts         | 40 | Notification System           |
| 14 | CMS / Content Pages         | 41 | Personalization               |
| 15 | Contact Options             | 42 | Social Commerce               |
| 16 | Transactional Emails        | 43 | Loyalty & Subscriptions       |
| 17 | SEO                         | 44 | Bulk Import / Export          |
| 18 | Performance                 | 45 | Visual Regression             |
| 19 | Mobile / Responsive         | 46 | UX Patterns                   |
| 20 | Cross-Browser               | 47 | Images & Media                |
| 21 | Accessibility               | 48 | Live Chat                     |
| 22 | Security                    | 49 | Disaster Recovery             |
| 23 | GDPR / Data Protection      | 50 | Data Migration                |
| 24 | Analytics & Tracking        | 51 | Legal Compliance              |
| 25 | APIs & Integrations         | 52 | Load & Stress Tests           |
| 26 | Admin Backend               | 53 | B2B Features                  |
| 27 | Internationalization        |    |                               |

## Test Point Format

The files in `en/` come in two forms:

- `01` to `50`, `52` and `53` list one test point per bullet, grouped under headings. Each bullet describes a single check, most often opening with "Verify", "Confirm", "Test" or "Check" (6,114 bullets in total).
- `51-legal-compliance.md` uses a table per test case with these fields (126 test cases):
  - **Test ID** (e.g., TC-51.1.1)
  - **Priority** (Critical / High / Medium / Low)
  - **Precondition**
  - **Steps**
  - **Expected Result**

## How to Use

1. Start with `testplan-complete-en.md` for an overview of all test areas.
2. Go through the individual `en/XX-*.md` files for the detailed test points.
3. Copy the test points into your test management tool. Only `en/51-legal-compliance.md` comes with test IDs, for the other files you assign your own.

## Tech Stack (Target)

- **Framework:** Laravel
- **Payments:** Stripe, PayPal, Klarna, Apple Pay, Google Pay, SEPA, bank transfer
- **Legal:** German/EU compliance (GDPR, PAngV, Button Solution, Cancellation Policy)

## Sources

The test plan was compiled from domain expertise and 14 online research sources, among them Katalon, BrowserStack and Shopify. Full source URLs are listed at the bottom of both master files.

## License

This work is licensed under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). Copyright 2026 Sven Withus. The full license text is in the `LICENSE` file.
