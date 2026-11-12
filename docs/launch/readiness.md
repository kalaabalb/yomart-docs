# YoMart launch readiness

**Updated:** 12 November 2026  
**Disposition:** API runtime is production and live health/CORS checks pass. Production checkout is intentionally disabled until catalogue approval. Web clients remain staging/noindex. Android CI is still building a debug APK and emulator screenshot. A signed Play AAB and Play Console test release do not yet exist.

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
| Storefront | [yomart.onrender.com](https://yomart.onrender.com), Render deployment from commit `9575398150b8daa1fe3190556aba9f3904363ad8`. | Staging/noindex preview build. |
| Admin web | [yomart-admin.onrender.com](https://yomart-admin.onrender.com), commit `5151cbcfd7d51f7e6eccfe4df0894faa9e11da65`. | Staging build. |
| Web screenshots | [Workflow 43225981502](https://github.com/kalaabalb/yomart-web/actions/runs/43225981502) uploaded genuine 1440x1100 desktop and 390x844 mobile screenshots for storefront and admin. | Captured; rerun after deployment to verify preview notice. |
| Mobile | App ID `com.yomobiles.client`, version `1.0.0+5`, target SDK 36. [Latest preview-mode CI 43222093964](https://github.com/kalaabalb/yomart-mobile/actions/runs/43222093964) is building. | Debug APK/emulator screenshot pending; no signed AAB. |
| Backend | Inventory reservation lifecycle deployed. Latest suite: 251 tests, 249 passed, 0 failed, 2 optional tests skipped. | Automated checks pass; live transactional checkout intentionally disabled. |
| Docs | Free-tier decision, production cutover runbook, live database audit and catalogue owner-approval CSV committed. | Updated for production switch. |

## Remaining technical gates

- [ ] Verify admin login, customer registration/login and admin role restrictions after runtime switch.
- [ ] Verify storefront and admin live screenshots after latest deployed build; ensure preview notice is visible and mobile layout is correct.
- [ ] Verify Android emulator screenshot artifact after current CI completes.
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
