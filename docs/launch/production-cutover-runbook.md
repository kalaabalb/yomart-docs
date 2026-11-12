# YoMart production cutover runbook

Snapshot: 12 November 2026

## Release target

Production is the intended destination for YoMart. The current public Render deployment is still a staging deployment and must not be described or treated as production.

- Customer storefront: https://yomart.onrender.com
- Admin: https://yomart-admin.onrender.com
- API: https://yomart-api.onrender.com
- Staging API health currently identifies its environment as staging.
- Existing Atlas staging project: YoMart-Staging, database: yomart_staging.
- New empty Atlas project provisioned for separation: YoMart-Production (project ID 6af5a8b7c1682d425fef1dac).
- Staging Atlas cluster is a Free tier cluster in AWS AP_SOUTH_1; it is not the production database.

## Production boundary

Do not point the public app, admin, or API at the staging database and do not relabel the staging API as production. Keep staging available for closed-test validation until production has passed its own smoke tests.

The production project is intentionally empty at this point. No production cluster, production database user, production database, or production records have been created. Creating a dedicated production cluster incurs ongoing Atlas charges; provision it only after the owner approves the paid tier and expected spend.

## Cutover sequence

1. Provision a dedicated production Atlas cluster with backups enabled and termination protection. Use a production-only database user with least-privilege access to the production database. Keep staging credentials and network access separate.
2. Configure the production Render API service with the production database URI and production environment mode. Keep secrets out of Git, docs, logs, and screenshots. Confirm exact allowed origins for the customer storefront and admin.
3. Confirm Cloudinary production asset folders and upload behavior. Payment proofs must remain authenticated/private; never expose proof URLs in order JSON or public static folders.
4. Deploy the API and verify health reports production, database connected, and the expected app version. Verify CORS, authentication, catalog reads, checkout, stock reservation/release/consumption, order ownership, payment-proof authorization, and admin role restrictions against production.
5. Configure customer storefront and admin to call the production API. Verify no staging hostname, staging banner, test credentials, placeholder legal copy, or noindex directives remain in public production output.
6. Publish privacy policy and account-deletion pages with the real publisher/support details and owner-approved deletion/retention language.
7. Build a signed Android App Bundle against the production API. Verify application ID, version code, target SDK, signing certificate/upload-key match, release manifest, and absence of staging URLs/secrets.
8. Run production smoke tests and capture genuine screenshots from the running app and web pages. Do not fabricate orders, customers, payment evidence, or screenshot evidence.
9. Only after the production checklist passes, submit the closed-testing build to Play Console. Keep the 14 real testers and 14-day requirement in view for accounts subject to Google's closed-testing rule.

## Go-live acceptance checklist

- [ ] Dedicated production Atlas cluster exists; backups and termination protection verified.
- [ ] Production database is isolated from staging and has a least-privilege database user.
- [ ] Production API reports production and connected database.
- [ ] Production API CORS allowlist contains only the approved HTTPS origins.
- [ ] Customer storefront and admin use the production API.
- [ ] Authentication and admin authorization pass on production.
- [ ] Checkout and inventory lifecycle pass on production.
- [ ] Payment-proof access is private and owner/admin authorization is enforced.
- [ ] Legal pages are published with approved publisher and support details.
- [ ] Production privacy/deletion behavior is approved and accurately described.
- [ ] Signed AAB uses the verified Play upload certificate and production API.
- [ ] Release build and Play listing assets are reviewed.
- [ ] Genuine Android-device acceptance and production screenshots are recorded.
- [ ] Closed-testing track is submitted and tester opt-in period is tracked.

## Current release evidence

- Mobile main CI: https://github.com/kalaabalb/yomart-mobile/actions/runs/43210860089 — passed on a776a77b208043095d27a44f675e0a04bf728826.
- The CI artifact is a debug APK, not a signed Play Store AAB.
- Staging catalog is synthetic review data; sample prices and stock are not real offers.
- Current staging payment configuration is cash collection, with no online gateway.
