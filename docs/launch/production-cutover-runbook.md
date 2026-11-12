# YoMart production cutover runbook

Snapshot: 12 November 2026

## Release target

The existing free infrastructure is being converted for production use. Do not provision paid database resources.

- Customer storefront: https://yomart.onrender.com
- Admin: https://yomart-admin.onrender.com
- API: https://yomart-api.onrender.com
- Existing Atlas project: YoMart-Staging (historical project name).
- Existing Atlas cluster: yomart-staging (Free tier, AWS AP_SOUTH_1).
- Existing database: yomart_staging (historical database name retained to avoid a risky rename).

## Production boundary and cost decision

The owner explicitly chose the existing Atlas Free cluster because no paid infrastructure budget is available. Reuse the existing cluster and its current database/catalog. Do not create an M10, Flex, or other paid resource. Do not delete the existing cluster or database.

The API Render service was updated on 12 November 2026 to set NODE_ENV=production and PLATFORM=Production. Deployment dep-dbqqjq6i0phs73df2uj0 completed and the new instance logged a successful MongoDB connection. The runtime environment still requires a live /health response confirming production and database connected.

An unused empty Atlas project named YoMart-Production (ID 6af5a8b7c1682d425fef1dac) exists from earlier setup. It has no cluster and is not in use. Leave it alone; it is not part of the active data path.

## Cutover and verification sequence

1. Keep the existing Free Atlas cluster and database. Monitor its free-tier storage/availability constraints and export important data regularly.
2. Verify API health reports production and database connected. Confirm exact CORS origins for the customer storefront and admin.
3. Verify Cloudinary product/category/poster assets and upload behavior. Payment proofs must remain authenticated/private; never expose proof URLs in order JSON or public static folders.
4. Verify admin login, customer registration/login, public catalog, checkout, stock reservation/release/consumption, order ownership, payment-proof authorization, and admin role restrictions.
5. Confirm the active product catalogue contains real owner-approved prices and stock before public selling. Current catalogue sample prices and stock are synthetic review data, not sale offers.
6. Configure customer storefront and admin to call the production API. Check deployed output for staging hostnames, test credentials, placeholder legal copy, and unintended noindex directives.
7. Publish privacy policy and account-deletion pages with real publisher/support details and owner-approved deletion/retention language.
8. Build a signed Android App Bundle against the production API. Verify application ID, version code, target SDK, signing certificate/upload-key match, release manifest, and absence of staging URLs/secrets.
9. Run production smoke tests and capture genuine screenshots from the running app and web pages. Do not fabricate orders, customers, payment evidence, or screenshot evidence.
10. Submit the closed-testing build only after release checks pass. Confirm the Play account's testing requirements and track the tester opt-in period.

## Go-live acceptance checklist

- [ ] API /health reports production and connected database.
- [ ] API CORS allowlist contains only approved HTTPS origins.
- [ ] Customer storefront and admin use the production API.
- [ ] Authentication and admin authorization pass against the active database.
- [ ] Checkout and inventory lifecycle pass against the active database.
- [ ] Payment-proof access is private and owner/admin authorization is enforced.
- [ ] Real product prices and stock are approved before public selling.
- [ ] Legal pages are published with approved publisher and support details.
- [ ] Production privacy/deletion behavior is approved and accurately described.
- [ ] Signed AAB uses the verified Play upload certificate and production API.
- [ ] Release build and Play listing assets are reviewed.
- [ ] Genuine Android-device acceptance and production screenshots are recorded.
- [ ] Closed-testing track is submitted and tester opt-in period is tracked.

## Current release evidence

- Mobile CI artifact: https://github.com/kalaabalb/yomart-mobile/actions/runs/43210860089 — debug APK only, not a signed Play Store AAB.
- Backend inventory lifecycle changes are deployed; latest documented backend suite: 245 passed, 2 optional replica-set tests skipped.
- Catalogue currently contains synthetic review prices/stock; these must not be represented as verified real offers.
- Checkout is configured for cash collection; no online payment gateway is configured.
