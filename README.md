# YoMart Documentation

Operational, technical, release, and tester documentation for the YoMart API, customer storefront, administrator dashboard, and Flutter Android app.

**Snapshot: 12 November 2026.** The API is running in production on the existing MongoDB Atlas Free cluster. Live health and exact CORS checks passed in [backend workflow 43228035338](https://github.com/kalaabalb/yomart-api/actions/runs/43228035338). Production checkout is intentionally disabled until the owner approves actual prices and stock. The web storefront/admin remain staging/noindex builds. Web screenshot automation has captured home, catalog, product detail, cart, login, support, legal drafts and admin sign-in at desktop/mobile widths; Android debug CI and emulator screenshot capture are in progress. No signed Play AAB exists yet.

## Start here

- [Architecture and system boundaries](docs/architecture.md)
- [Deployment guides](docs/deployment/README.md)
- [Environment variable reference](docs/deployment/environment-variables.md)
- [Operations and recovery](docs/operations.md)
- [Security and privacy](docs/security.md)
- [Customer workflow](docs/workflows/customer.md)
- [Administrator workflow](docs/workflows/admin.md)
- [Launch readiness and release gates](docs/launch/readiness.md)
- [Staging acceptance plan](docs/launch/staging-acceptance.md) — historical template only; do not run against the active production database.
- [Closed-testing playbook](docs/launch/closed-testing-playbook.md)
- [Staging catalogue and screenshot evidence](docs/launch/staging-catalog-and-screenshot-evidence.md)
- [Free-tier production decision](docs/launch/free-tier-production-decision.md)
- [Production cutover runbook](docs/launch/production-cutover-runbook.md)
- [Live database audit, 12 November 2026](docs/launch/live-database-audit-2026-11-12.md)
- [Product catalogue owner-approval template](docs/launch/catalog-owner-approval.csv)

## Repositories and deployed addresses

| Component | Source | Current address / evidence |
|---|---|---|
| API | [yomart-api](https://github.com/kalaabalb/yomart-api) | [yomart-api.onrender.com](https://yomart-api.onrender.com) — runtime production, live health HTTP 200 and CORS preflights pass in [run 43228035338](https://github.com/kalaabalb/yomart-api/actions/runs/43228035338). |
| Admin | [yomart-admin](https://github.com/kalaabalb/yomart-admin) | [yomart-admin.onrender.com](https://yomart-admin.onrender.com) — static site remains staging/noindex; [CI passed](https://github.com/kalaabalb/yomart-admin/actions/runs/43210816065). |
| Customer storefront | [yomart-web](https://github.com/kalaabalb/yomart-web) | [yomart.onrender.com](https://yomart.onrender.com) — static site remains staging/noindex; [CI and desktop/mobile screenshot artifact](https://github.com/kalaabalb/yomart-web/actions/runs/43227841647). |
| Android app | [yomart-mobile](https://github.com/kalaabalb/yomart-mobile) | Flutter app, ID `com.yomobiles.client`. Checkout compatibility fix merged as `69c91510c4b26d5c976a46727f28252319e7a109`. [Latest preview-mode CI](https://github.com/kalaabalb/yomart-mobile/actions/runs/43222093964) is building the debug APK and emulator screenshot. |
| Documentation | [yomart-docs](https://github.com/kalaabalb/yomart-docs) | This repository. |

## Current release boundary

- API runs in **production** on the existing Atlas Free cluster/database. No paid database resource was created and no existing database was deleted. Historical Atlas project/database names still contain “Staging”; see the free-tier decision.
- Production health: HTTP 200, success true, environment production, database connected, Cloudinary image storage, authenticated private payment-proof storage, version 1.0.4.
- CORS preflight: HTTP 204 with exact allow-origin for both customer storefront and admin.
- Hosted noindex legal drafts are available at [privacy policy](https://yomart.onrender.com/privacy-policy.html) and [account deletion](https://yomart.onrender.com/account-deletion.html); publisher/support details and retention wording are not finalized.\n- Production checkout is explicitly disabled with `YOMART_SALES_ENABLED=false`; new orders fail closed until owner-approved prices and stock are configured. Public catalog APIs also omit synthetic price and quantity fields.
- Admin and customer static sites remain staging/noindex builds. The customer storefront displays a preview-catalogue notice in non-production mode.
- Active catalogue has 8 synthetic products with illustrative prices/stock, 2 inactive historical test products, 4 categories, 4 subcategories, 2 brands, 2 collections and 2 posters. All active products remain unverified for public search. Do not sell these as real offers.
- Live database audit found 0 orders and 0 payment-proof records. One admin account and one customer account exist; personal identifiers and credentials are excluded from documentation.
- Cash at shop or handover is the only configured payment method. No online gateway or bank/wallet method is configured.
- Backend latest suite: 253 tests, 251 passed, 0 failed, 2 optional tests skipped. Live health/CORS smoke is green.
- Mobile debug APK and emulator screenshot are not a signed Play release AAB. Play Console listing, upload-key verification, legal publisher/support details, and real owner-approved catalogue remain outstanding.
