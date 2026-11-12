# YoMart production cutover runbook

Snapshot: 12 November 2026

## Active release target

- Storefront: https://yomart.onrender.com
- Admin: https://yomart-admin.onrender.com
- API: https://yomart-api.onrender.com
- Atlas project: YoMart-Staging (historical name).
- Cluster: yomart-staging (Free tier, AWS AP_SOUTH_1).
- Database: yomart_staging (historical name; active production database).

## Production boundary and verified status

The owner selected the existing Atlas Free cluster. Reuse its database/catalog. Do not create paid resources or delete/rename the cluster/database.

The API runs with `NODE_ENV=production` and `PLATFORM=Production`. Production checkout is disabled with `YOMART_SALES_ENABLED=false`; new orders fail closed with HTTP 503 until owner-approved real prices and stock are configured.

GitHub Actions [run 43226507608](https://github.com/kalaabalb/yomart-api/actions/runs/43226507608) passed backend tests and live production smoke checks:
- `GET /health`: HTTP 200, `success:true`, environment `production`, platform `Production`, database `connected`, Cloudinary image storage and authenticated private payment-proof storage, version `1.0.4`.
- CORS preflight: HTTP 204 with exact allow-origin for storefront and admin.

The historical database name remains `yomart_staging`; it is not a separate staging database. An unused empty Atlas project `YoMart-Production` (ID `6af5a8b7c1682d425fef1dac`) has no cluster and is not in use.

## Current data

- 20 collections; 1.44 MB.
- 8 active synthetic catalogue products, 2 inactive historical test fixtures, 4 categories, 4 subcategories, 2 brands, 2 collections, 2 posters.
- One admin account and one customer account.
- Zero orders and zero payment-proof records.
- One enabled payment method: Cash at shop or handover; no proof required, zero delivery fee. No online gateway or bank/wallet method.

Never present sample prices/stock as real sale offers. Do not fabricate customers, orders, reviews, payment proofs or tester activity.

## Cutover and verification sequence

1. Keep the existing Free Atlas cluster/database; monitor storage and availability, export important data regularly.
2. Keep `YOMART_SALES_ENABLED=false` until real product prices and stock are owner-approved.
3. Verify admin login, customer authentication, public catalog, order ownership and admin role restrictions. Avoid synthetic writes to the active production database.
4. Keep storefront and admin builds noindex until catalogue/legal details are approved. Both remain staging/noindex builds.
5. Verify Cloudinary catalog assets and upload behavior. Payment proofs remain authenticated/private; never expose proof URLs in order JSON or public static folders.
6. Publish privacy policy and account-deletion pages with real publisher/support details and owner-approved retention language.
7. Build a signed Android App Bundle against the production API. Verify application ID, version code, target SDK, signing certificate/upload-key match, release manifest and absence of staging URLs/secrets.
8. Test signed release on physical Android devices. Current CI screenshots cover web at desktop/mobile widths; Android emulator screenshot capture is still running.
9. Enable sales only after approved catalogue data and a controlled checkout acceptance plan. Verify inventory lifecycle and payment-proof authorization without fake production activity.
10. Submit closed testing only after Play release checks pass. Track actual tester opt-in dates.

## Go-live acceptance

- [x] API health reports production and connected database.
- [x] CORS preflight allows the two approved web origins.
- [x] Production checkout is disabled until catalogue approval.
- [ ] Storefront/admin rebuilt with intended production configuration and approved catalogue.
- [ ] Authentication and admin authorization pass after runtime switch.
- [ ] Real product prices and stock approved.
- [ ] Private proof upload/retrieval verified under a controlled test plan.
- [ ] Legal pages published with approved publisher/support details.
- [ ] Privacy/deletion behavior approved and accurately described.
- [ ] Signed AAB uses verified Play upload certificate and production API.
- [ ] Physical Android acceptance, Play listing assets and declarations complete.
- [ ] Closed-testing track submitted and tester opt-in period tracked.

## Current evidence

- Backend latest suite: 251 tests, 249 passed, 0 failed, 2 optional tests skipped.
- Live production health/CORS smoke: [run 43226507608](https://github.com/kalaabalb/yomart-api/actions/runs/43226507608).
- Genuine web screenshots: [run 43225981502](https://github.com/kalaabalb/yomart-web/actions/runs/43225981502), desktop/mobile storefront and admin.
- Android latest CI: [run 43222093964](https://github.com/kalaabalb/yomart-mobile/actions/runs/43222093964), debug APK and emulator screenshot still in progress at this snapshot.
- A debug APK is not a signed Play Store AAB.
