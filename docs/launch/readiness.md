# YoMart launch readiness

**Updated:** 12 November 2026  
**Disposition:** API is live in production on the existing Atlas Free cluster; live health and CORS checks pass. Checkout is deliberately blocked until real catalogue prices and stock are approved. Storefront/admin are deployed preview builds, not public-sales builds. Genuine web screenshots and a debug APK artifact exist. Android emulator screenshot capture is being stabilized. No signed Play AAB or Play test release exists.

## Verified state

| Area | Evidence | Status |
|---|---|---|
| API runtime | Live smoke workflow [43226507608](https://github.com/kalaabalb/yomart-api/actions/runs/43226507608) passed. Health HTTP 200, success true, production, database connected, Cloudinary image storage and authenticated private proof storage, version 1.0.4. | Verified |
| CORS | Same live workflow confirmed HTTP 204 exact allow-origin for customer storefront and admin. | Verified |
| Checkout guard | Backend source has fail-closed `YOMART_SALES_ENABLED`; production env set to false. API blocks new order creation until owner-approved price/stock data is ready. | Protected |
| Database | Existing Atlas Free cluster `yomart-staging`, project `YoMart-Staging`, database `yomart_staging`. 20 collections; 1.44 MB. | Active production data store; no separate staging database. |
| Data | 10 products (8 active, 2 inactive), 4 categories, 4 subcategories, 2 brands, 2 collections, 2 posters, 1 admin account, 1 customer account, 1 settings record, 0 orders, 0 payment proofs. | No real order/payment evidence yet. No fake records created. |
| Catalogue | All 8 active products have `isSearchVerified:false` and source notes explicitly mark prices/stock as synthetic. | Do not sell or index these as verified offers until owner approves prices/stock. |
| Payment | One enabled method: Cash at shop or handover; no proof required. | Manual cash at handover; no online gateway or bank/wallet method. |
| Storefront | [yomart.onrender.com](https://yomart.onrender.com), latest verified deployment commit `c0aecd43b58f4780b8ecd43aa6c2ca7f866950f3`. | Preview catalogue notice is visible; ordering is unavailable. Not approved as a sales storefront. |
| Admin web | [yomart-admin.onrender.com](https://yomart-admin.onrender.com), latest verified deployment commit `dbe80bc26f718e2c54dae4446811936e9c771ba2`. | Deployed; sign-in screen screenshot captured. Authenticated dashboard/role flows still need owner-assisted acceptance. |
| Web screenshots | [Workflow 43225981502](https://github.com/kalaabalb/yomart-web/actions/runs/43225981502) artifact `yomart-live-web-screenshots` contains genuine desktop/mobile storefront, product, cart, login, support, legal-draft and admin sign-in screenshots. | Captured from hosted sites; admin artifact is sign-in only, not authenticated dashboard. |
| Mobile | App ID `com.yomobiles.client`, version `1.0.0+5`, target SDK 36. [Debug APK artifact run 43226005733](https://github.com/kalaabalb/yomart-mobile/actions/runs/43226005733) is available (83.2 MB). Latest CI after emulator config fix: [run 43227075413](https://github.com/kalaabalb/yomart-mobile/actions/runs/43227075413). | Format, analyze, tests passed and debug APK built in the preceding run; screenshot step had emulator boot timeout. Retried with API 34 / GPU off; no signed AAB. |
| Backend | Inventory reservation lifecycle deployed. Latest suite: 253 tests, 251 passed, 0 failed, 2 optional tests skipped. | Automated checks pass; live transactional checkout intentionally disabled. |
| Docs | Free-tier decision, production cutover runbook, live database audit and catalogue owner-approval CSV committed. | Updated for production switch. |

## Remaining technical gates

- [ ] Verify admin login, customer registration/login and admin role restrictions after runtime switch (requires authorized account credentials and a controlled browser session).
- [x] Capture hosted storefront/admin sign-in screenshots at desktop and mobile widths; preview catalogue notice is visible.
- [ ] Capture authenticated admin dashboard and all role-protected admin pages after an authorized owner login.
- [ ] Verify Android emulator screenshot artifact after run 43227362300 completes; prior emulator boot timed out, so the screenshot artifact is not yet available.
- [ ] Physical Android-device acceptance is still required.
- [ ] Verify private payment-proof upload/retrieval authorization only under a controlled test plan; do not use real receipts.
- [ ] Confirm Free-tier storage/availability constraints and export/restore process.
- [ ] Rebuild storefront/admin as production only after legal details and approved catalogue are ready; configure indexing intentionally.

## Owner inputs required before real selling or Play submission

- [ ] Approve actual ETB price and available stock for each active product using [catalog-owner-approval.csv](catalog-owner-approval.csv).
- [ ] Confirm product authenticity, condition, warranty, exact model/storage/color and sale availability.
- [ ] Provide legal publisher/business name, monitored support email and public business contact/address.
- [ ] Approve privacy/account deletion retention language. Current implementation pseudonymizes some order/payment records; it does not erase every record.
- [ ] Confirm cash-on-collection fulfillment meaning: shop pickup, staff handover/delivery, or both.
- [ ] Provide the friend’s Play Console account type/creation date to establish whether the 12-tester/14-day rule applies.
- [ ] Compare release keystore certificate with Play Console upload-key certificate. Private key/password remain local.
- [ ] Approve whether the eight synthetic products remain visible to closed testers while actual prices/stock are pending. They must not be sold as real offers.

## Play release gates

- [ ] Build signed production AAB using verified production API and final privacy URL.
- [ ] Confirm package ID, incremented version code, target SDK, release manifest and upload signing certificate.
- [ ] Install Play-delivered build on physical Android devices.
- [ ] Complete store listing, feature graphic, genuine phone screenshots, support details, app access, content rating, target audience, ads and Data Safety declarations.
- [ ] Publish closed testing only after the catalogue and checkout are safe for the test plan.
- [ ] If the Play account is a new personal developer account created after 13 November 2023, maintain at least 12 opted-in testers continuously for 14 days before applying for production access.

## Safety boundary

Hosted legal drafts: [privacy policy](https://yomart.onrender.com/privacy-policy.html) and [account deletion](https://yomart.onrender.com/account-deletion.html). Both are noindex drafts; the deletion page is wired to the production API but must not be treated as Play-ready until owner/legal approval.\n\nThe existing Atlas Free cluster is the production database. Do not run synthetic write tests, stock mutations, dummy orders, fake payment uploads or destructive cleanup against it. Production checkout remains disabled until the owner approves actual prices and stock. No paid infrastructure is authorized.
