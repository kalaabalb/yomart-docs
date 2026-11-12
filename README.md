# YoMart Documentation

Operational, technical, release, and tester documentation for the YoMart API, customer storefront, administrator dashboard, and Flutter Android app.

**Snapshot: 12 November 2026.** The API Render service has been switched to runtime `production` using the existing MongoDB Atlas Free cluster and existing database. Render confirms deployment `dep-dbqqjq6i0phs73df2uj0` succeeded; boot logs confirm `Environment: production` and successful MongoDB connection. Public catalogue requests were observed after deployment. A direct live `GET /health` JSON response still needs confirmation. The customer storefront and admin remain deployed as staging/noindex static builds. Android CI has passed and produced a debug APK, not a signed Play release AAB.

## Start here

- [Architecture and system boundaries](docs/architecture.md)
- [Deployment guides](docs/deployment/README.md)
- [Environment variable reference](docs/deployment/environment-variables.md)
- [Operations and recovery](docs/operations.md)
- [Security and privacy](docs/security.md)
- [Customer workflow](docs/workflows/customer.md)
- [Administrator workflow](docs/workflows/admin.md)
- [Launch readiness and release gates](docs/launch/readiness.md)
- [Staging acceptance plan](docs/launch/staging-acceptance.md)
- [Closed-testing playbook: 14 testers / 15 days](docs/launch/closed-testing-playbook.md)
- [Staging catalogue and screenshot evidence](docs/launch/staging-catalog-and-screenshot-evidence.md)
- [Free-tier production decision](docs/launch/free-tier-production-decision.md)
- [Corrected production cutover runbook](docs/launch/production-cutover-runbook.md)
- [Live database audit, 12 November 2026](docs/launch/live-database-audit-2026-11-12.md)
- [Product catalogue owner-approval template](docs/launch/catalog-owner-approval.csv)

## Repositories and deployed addresses

| Component | Source | Current address / evidence |
|---|---|---|
| API | [yomart-api](https://github.com/kalaabalb/yomart-api) | [yomart-api.onrender.com](https://yomart-api.onrender.com) — runtime switched to production on 12 Nov 2026; deployment `dep-dbqqjq6i0phs73df2uj0` succeeded and boot logs confirm database connected. Live health JSON response remains to be verified. |
| Admin | [yomart-admin](https://github.com/kalaabalb/yomart-admin) | [yomart-admin.onrender.com](https://yomart-admin.onrender.com) — static site deployed from `5151cbcfd7d51f7e6eccfe4df0894faa9e11da65`; build remains staging/noindex. [CI passed](https://github.com/kalaabalb/yomart-admin/actions/runs/43210816065). |
| Customer storefront | [yomart-web](https://github.com/kalaabalb/yomart-web) | [yomart.onrender.com](https://yomart.onrender.com) — static site deployed from `e4c443c1b7d4049f9a5812d4956d797d348934cc`; build remains staging/noindex. [CI passed](https://github.com/kalaabalb/yomart-web/actions/runs/43210820743). |
| Android app | [yomart-mobile](https://github.com/kalaabalb/yomart-mobile) | Flutter app, application ID `com.yomobiles.client`. Checkout fix merged as `69c91510c4b26d5c976a46727f28252319e7a109`; [latest main CI and debug APK artifact](https://github.com/kalaabalb/yomart-mobile/actions/runs/43210871913). |
| Documentation | [yomart-docs](https://github.com/kalaabalb/yomart-docs) | This repository. |

## Current release boundary

- The API runtime is configured as **production** and uses the existing Atlas Free cluster/database. No paid database resource was created and no existing database was deleted. The historical Atlas project/database names still contain “Staging”; see the free-tier decision and live database audit.
- The admin and customer static sites have not yet been switched to production builds. Their current deployed builds remain staging/noindex, which is appropriate until the catalogue and public legal details are approved.
- The active catalogue has 8 synthetic products with illustrative prices/stock, 2 inactive historical test products, 4 categories, 4 subcategories, 2 brands, 2 collections and 2 campaign posters. All active products remain unverified for public search. The synthetic prices/quantities are not approved retail offers.
- Live database audit found 0 orders and 0 payment-proof records. One admin account and one customer account exist; personal identifiers and credential fields are excluded from documentation.
- Cash on collection is enabled. It is manual payment to YoMart staff at handover; no online payment gateway or bank/wallet payment method is configured.
- Backend inventory reservation lifecycle changes are deployed at `0371cbd01fb7b40df05391c49cedd58f3ea1736f`; [backend CI passed](https://github.com/kalaabalb/yomart-api/actions/runs/43108691199).
- Mobile checkout compatibility fix is merged. Latest main CI run `43210871913` passed and uploaded a debug APK. It is not a signed release AAB.
- A signed production AAB, upload-key verification, final publisher/support/legal details, Play Console listing/declarations, genuine device screenshots, and closed-test setup are separate gates.
- If the friend’s Play developer account is a new personal account created after 13 November 2023, Google requires at least 12 testers continuously opted in to a closed test for 14 days before applying for production access. The account type and creation date must be checked in that Play Console.

## Evidence conventions

Every readiness statement must name its evidence and date. Distinguish source/test evidence, provider deployment evidence, live database state, owner/legal decisions, and Play Console state. Never label the product production-ready from a successful build alone.
