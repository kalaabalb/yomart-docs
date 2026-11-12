# YoMart catalogue and screenshot evidence

**Updated:** 12 November 2026  
**Historical origin:** Atlas project `YoMart-Staging`, cluster `yomart-staging`, database `yomart_staging`.  
**Current status:** this same Free-tier cluster/database is now the active production data store. There is no separate staging database or API.

## Current catalogue inventory

The active production database contains:

| Collection | Documents |
|---|---:|
| Categories | 4 |
| Subcategories | 4 |
| Brands | 2 |
| Products | 10 |
| Collections | 2 |
| Posters | 2 |

Eight products are active and two old checkout/stock fixtures are inactive. The seeded catalogue spans phones, tablets, audio, computers and wearables. All active products remain `isSearchVerified: false`.

The eight active product records contain illustrative prices and stock quantities, not owner-approved retail offers. Their source metadata identifies the values as synthetic. The public API omits price, offer price, quantity and internal provenance while production sales are disabled. Do not enable sales or mark products search-verified until the owner confirms each exact model, condition, warranty, ETB selling price, and available stock.

## Production checkout and payment

- Production order creation is disabled by `YOMART_SALES_ENABLED=false`.
- One enabled payment method exists: cash at shop pickup or direct handover by YoMart staff.
- No payment proof is required for this cash method.
- No online gateway, bank transfer or wallet method is configured.
- There are zero orders and zero payment-proof records.
- Do not create synthetic orders, receipts, reviews, users or tester activity in this production database.

## Cloudinary assets

The current Cloudinary account stores product images in `products/`, category images in `categories/`, and campaign banners in `posters/`. Public catalogue images are separate from private payment-proof storage. Never expose payment-proof URLs through public catalog or order responses.

## Genuine screenshot evidence

The hosted web screenshot artifact is available from [Customer Storefront CI run 43229601660](https://github.com/kalaabalb/yomart-web/actions/runs/43229601660). Artifact 13951200004 contains 12 genuine PNG captures of the deployed sites:
- Storefront desktop and mobile home
- Product listing desktop and mobile
- Product detail mobile
- Cart mobile
- Customer login mobile
- Support desktop
- Privacy-policy draft desktop
- Account-deletion draft mobile
- Admin sign-in desktop and mobile

Important scope limits:
- The admin screenshots are the sign-in page, not an authenticated admin dashboard.
- These screenshots show the preview-catalogue state, not an approved live-sales experience.
- The preview-only Android split-ABI debug APKs are available from [mobile CI run 43234715460](https://github.com/kalaabalb/yomart-mobile/actions/runs/43234715460), artifact 13951718520. It contains arm32, arm64 and x86_64 APKs; the arm64 build is for most modern phones. These are sideload acceptance builds, not Play submission AABs.
- The Android emulator screenshot job failed because the GitHub-hosted emulator never reported sys.boot_completed within the 240-second boot limit. No Android screenshot artifact was produced. The code/tests/APK verification job passed independently.
- Capture genuine app screenshots on a physical Android device after the owner signs in. Physical Android-device acceptance remains outstanding.

## Safety rules

- The database named `yomart_staging` is the active production data store despite its historical name. Do not treat it as a safe test database.
- Do not run synthetic write tests, dummy orders, fake payment uploads, stock mutations or destructive cleanup against it.
- Keep production checkout disabled until all catalogue prices and stock are owner-approved.
- Do not publish synthetic prices/stock as real offers or as search-engine product offers.
- Do not fabricate customers, tester opt-ins, reviews, orders, payment proofs or engagement.
- Keep secrets, credentials, personal data, receipts and unredacted customer screenshots out of source control and public docs.
