# YoMart launch readiness

**Updated:** 12 November 2026  
**Disposition:** The API is live in production on the existing Atlas Free cluster. Production health, catalogue redaction, checkout settings and CORS passed live smoke checks. Checkout is deliberately disabled until real catalogue prices and stock are approved. Storefront/admin are deployed preview builds, not public-sales builds. Twelve genuine web screenshots are captured. A production-connected, preview-only Android debug APK is available for sideload acceptance. The Android emulator screenshot attempt was cancelled during the previous run; a smaller split-ABI rebuild and isolated emulator job are running. No signed Play AAB or Play test release exists.

## Verified state

| Area | Evidence | Status |
|---|---|---|
| API runtime | [Backend workflow 43233324854](https://github.com/kalaabalb/yomart-api/actions/runs/43233324854) passed live smoke. Health HTTP 200, success true, production, database connected, Cloudinary image storage and authenticated private proof storage, version 1.0.4. | Verified |
| CORS | Same live workflow confirmed HTTP 204 exact allow-origin for customer storefront and admin. | Verified |
| Checkout guard | Render has explicit YOMART_SALES_ENABLED=false. The backend order path remains fail-closed until owner-approved catalogue data exists. | Protected |
| Database | Existing Atlas Free cluster yomart-staging, project YoMart-Staging, database yomart_staging. 20 collections; 1.44 MB at the latest audit. | Active production data store; no separate staging database. |
| Data | 10 products (8 active, 2 inactive), 4 categories, 4 subcategories, 2 brands, 2 collections, 2 posters, 1 admin account, 1 customer account, 1 settings record, 0 orders, 0 payment proofs. | No real order/payment evidence yet. No fake records created. |
| Catalogue | All 8 active products have isSearchVerified:false and source notes mark prices/stock as synthetic. Live smoke confirms public list/detail responses omit price, offerPrice, quantity and internal provenance. | Do not sell or index these as verified offers until owner approves prices/stock. |
| Payment | One enabled method: Cash at shop or handover; no proof required; delivery fee zero. | Manual cash at handover; no online gateway or bank/wallet method. |
| Storefront | [yomart.onrender.com](https://yomart.onrender.com), deployed from commit d50f052e8f2095b87692dda33cb7a9c203a295e4. | Preview catalogue notice visible; ordering unavailable. Not approved as a sales storefront. |
| Admin web | [yomart-admin.onrender.com](https://yomart-admin.onrender.com), deployed from commit dbe80bc26f718e2c54dae4446811936e9c771ba2. | Sign-in screen captured. Authenticated dashboard/role flows still need owner-assisted acceptance. |
| Web screenshots | [Screenshot artifact 13951200004](https://github.com/kalaabalb/yomart-web/actions/runs/43229601660) contains 12 genuine hosted PNGs: desktop/mobile storefront, listing, product detail, cart, login, support, privacy/account-deletion drafts, and admin sign-in. | Captured; admin screenshots are sign-in only, not authenticated dashboard. |
| Mobile | App ID com.yomobiles.client, source version 1.0.0+5, target SDK 36. [All-ABI preview APK artifact 13950889093](https://github.com/kalaabalb/yomart-mobile/actions/runs/43232961622) was built with production API, privacy URL and YOMART_PREVIEW_CATALOG=true. [Split-ABI rebuild run 43234715460](https://github.com/kalaabalb/yomart-mobile/actions/runs/43234715460) is in progress. | Flutter format, analyze, tests and APK build passed in the all-ABI run. APK is debug-signed for sideload testing, not Play submission. No Android screenshot captured yet. |
| Backend | Inventory reservation lifecycle deployed. Latest suite: 253 tests, 251 passed, 0 failed, 2 optional tests skipped. | Automated checks pass; live transactional checkout intentionally disabled. |
| Docs | Free-tier decision, production cutover runbook, live database audit, catalogue owner-approval CSV and Play listing draft committed. | Updated for production switch and preview-only testing. |

## Remaining technical and owner-assisted gates

- [ ] Verify customer registration/login and admin login/role restrictions with an authorized owner session.
- [ ] Capture authenticated admin dashboard and protected pages after the owner signs in.
- [ ] Complete Android emulator screenshot capture; if the hosted emulator cannot boot, capture the app on a physical Android device.
- [ ] Install the preview APK on a real Android device and confirm launch, login, catalogue, image loading and preview-safe checkout messaging.
- [ ] Verify private payment-proof upload/retrieval authorization only under a controlled test plan; do not use real receipts.
- [ ] Confirm Free-tier storage/availability constraints and export/restore process.
- [ ] Rebuild storefront/admin as production only after legal details and approved catalogue are ready; configure indexing intentionally.

## Owner inputs required before Play submission or real selling

- [ ] Supply legal publisher/business name, monitored support email and public business contact/address.
- [ ] Approve privacy/account deletion retention language. Current implementation pseudonymizes some order/payment records; it does not erase every record.
- [ ] Add Android signing secrets to the mobile repository and set YOMART_PRIVACY_POLICY_URL as described in [owner-inputs-required.md](owner-inputs-required.md). Never paste keystore secrets into chat.
- [ ] Compare release keystore certificate with Play Console upload-key certificate. Private key/password remain local.
- [ ] Confirm the friend's Play Console account type/creation date, package ID availability and highest version code already used.
- [ ] Approve actual ETB prices, stock, exact variants/condition and warranty in [catalog-owner-approval.csv](catalog-owner-approval.csv) before enabling sales. These are not required for preview-only closed testing.
- [x] Cash fulfillment wording is configured as shop pickup or direct handover by YoMart staff; public checkout smoke confirms one enabled cash method, no proof requirement and zero delivery fee.
- [x] Closed-test preview decision implemented: products remain browseable, illustrative prices/stock are hidden, app/storefront ordering is disabled, and the API independently blocks new orders.

## Play release gates

- [ ] Finalize and publish privacy/account-deletion pages with owner-approved publisher/support details and retention wording.
- [ ] Build signed production AAB using verified production API and final privacy URL.
- [ ] Confirm package ID, incremented version code, target SDK, release manifest and upload signing certificate.
- [ ] Install Play-delivered build on physical Android devices.
- [ ] Complete Play listing, feature graphic, genuine phone screenshots, support details, app access, content rating, target audience, ads and Data Safety declarations.
- [ ] Publish closed testing only after the owner accepts the preview build and Play Console details.
- [ ] If the Play account is a new personal developer account created after 13 November 2023, maintain at least 12 opted-in testers continuously for 14 days before applying for production access.

## Safety boundary

Hosted legal drafts: [privacy policy](https://yomart.onrender.com/privacy-policy.html) and [account deletion](https://yomart.onrender.com/account-deletion.html). Both are noindex drafts; the deletion page is wired to the production API but must not be treated as Play-ready until owner/legal approval.

The existing Atlas Free cluster is the production database. Do not run synthetic write tests, stock mutations, dummy orders, fake payment uploads or destructive cleanup against it. Production checkout remains disabled until the owner approves actual prices and stock. No paid infrastructure is authorized.
