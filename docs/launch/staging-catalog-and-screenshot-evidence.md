# YoMart staging catalogue and screenshot evidence

**Environment:** `yomart_staging` (Atlas project `YoMart-Staging`)  
**Updated:** 2026-11-12  
**Purpose:** provide a populated, clearly synthetic catalogue for closed-test UI review and screenshot capture.

## Seeded catalogue

The staging database currently contains:

| Collection | Documents |
|---|---:|
| Categories | 4 |
| Subcategories | 4 |
| Brands | 2 |
| Products | 10 |
| Collections | 2 |
| Posters | 2 |

The 10 products include two earlier stock/checkout test fixtures and eight catalogue samples across phones, tablets, audio, computers and wearables. Apple and Samsung are the seeded brands. All seeded products remain `isSearchVerified: false`; they must not be represented as verified retail offers or indexed as production catalogue content.

The catalogue sample prices and quantities are illustrative test data only. Confirm actual prices, stock, product authenticity, warranty, and source images with the business owner before any production use.

## Cloudinary assets

The current Cloudinary account stores product images in `products/`, category images in `categories/`, and two 1600×600 campaign banners in `posters/`. The poster records in MongoDB point to the corresponding secure Cloudinary URLs. No payment proofs, customer receipts, or private user uploads were seeded.

## Payment test configuration

The staging store settings currently have cash on collection enabled, no payment proof requirement, and a zero delivery fee. Testers must not transfer money or upload real payment receipts during this test. An order placed with cash on collection is a test order and should be cancelled/cleaned up after validation.

## Safety rules

- Do not copy staging data into production.
- Do not mark synthetic product records as search-verified.
- Do not use real customer identities or fabricate tester accounts, opt-ins, reviews, or engagement.
- Use individual tester accounts created through the app for real closed testing.
- Use only synthetic customer details and test orders in staging.
- Never commit credentials, personal data, receipts, or unredacted customer screenshots.

## Screenshot capture set

Capture screenshots from the actual running apps, not generated mockups. Use the same staging catalogue and note the commit/version and viewport/device for each capture.

### Customer mobile app
1. Splash and first launch
2. Home with posters, collections and product cards
3. Category and brand browsing
4. Product detail with images, price and stock
5. Cart with quantity changes
6. Checkout delivery/collection details
7. Cash on collection explanation
8. Order confirmation
9. Order list and order detail/tracking
10. Empty, loading, offline and retry states

### Admin
1. Sign-in and dashboard
2. Product list and product edit
3. Category, subcategory and brand management
4. Collection and poster management
5. Orders list and order detail
6. Payment review and order status actions
7. Store settings and payment method
8. Mobile responsive navigation and tables

### Storefront
1. Desktop home and campaign posters
2. Mobile home
3. Product listing and product detail
4. Cart and checkout
5. Empty/error/retry states

### File naming
Use `YYYY-MM-DD_surface_screen_viewport_version.png`, for example `2026-11-12_mobile_home_pixel-7_android-15_1.0.0.png`.

Before sharing screenshots, redact names, phone numbers, email addresses, addresses, access tokens, receipts, and order identifiers. Keep originals in a private project folder; publish only approved sanitized captures.

## Current limitation

This document records database and Cloudinary seed state only. It is not evidence that every UI flow has been exercised on a physical device, that a release-signed Android App Bundle exists, or that a Play closed-test track has been published. Those require a signed release build and real device/Play Console verification.
