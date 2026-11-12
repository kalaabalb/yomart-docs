# YoMart Documentation

Operational, technical, release, and tester documentation for the YoMart API, customer storefront, administrator dashboard, and Flutter Android app.

**Snapshot: 12 November 2026.** The API is live in production on the existing MongoDB Atlas Free cluster. [Backend workflow 43233324854](https://github.com/kalaabalb/yomart-api/actions/runs/43233324854) passed the full 253-test suite and live production health/catalogue/checkout-settings/CORS smoke. `YOMART_SALES_ENABLED=false` is explicitly set. The web storefront/admin remain staging/noindex preview builds. Twelve genuine desktop/mobile web screenshots are available from [web screenshot artifact 13951200004](https://github.com/kalaabalb/yomart-web/actions/runs/43229601660). Split-ABI, production-connected, preview-only Android APKs are available from [mobile artifact 13951718520](https://github.com/kalaabalb/yomart-mobile/actions/runs/43234715460). The hosted Android emulator did not boot, so Android screenshots must be captured on a physical device. No signed Play AAB exists yet.

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
- [Play Store closed-test listing draft](docs/launch/play-store-listing-preview.md)
- [Staging catalogue and screenshot evidence](docs/launch/staging-catalog-and-screenshot-evidence.md)
- [Free-tier production decision](docs/launch/free-tier-production-decision.md)
- [Production cutover runbook](docs/launch/production-cutover-runbook.md)
- [Live database audit, 12 November 2026](docs/launch/live-database-audit-2026-11-12.md)
- [Product catalogue owner-approval template](docs/launch/catalog-owner-approval.csv)
- [Owner inputs required for legal approval and Play signing](docs/launch/owner-inputs-required.md)

## Repositories and deployed addresses

| Component | Source | Current address / evidence |
|---|---|---|
| API | [yomart-api](https://github.com/kalaabalb/yomart-api) | [yomart-api.onrender.com](https://yomart-api.onrender.com) — production runtime, health HTTP 200, database connected, 8 preview products redacted, cash settings and CORS verified in [run 43233324854](https://github.com/kalaabalb/yomart-api/actions/runs/43233324854). |
| Admin | [yomart-admin](https://github.com/kalaabalb/yomart-admin) | [yomart-admin.onrender.com](https://yomart-admin.onrender.com) — static preview/noindex site; [CI passed](https://github.com/kalaabalb/yomart-admin/actions/runs/43225797127). |
| Customer storefront | [yomart-web](https://github.com/kalaabalb/yomart-web) | [yomart.onrender.com](https://yomart.onrender.com) — static preview/noindex site; [CI and genuine screenshot artifact](https://github.com/kalaabalb/yomart-web/actions/runs/43229601660). |
| Android app | [yomart-mobile](https://github.com/kalaabalb/yomart-mobile) | Flutter app, ID `com.yomobiles.client`. Checkout compatibility fix merged as `69c91510c4b26d5c976a46727f28252319e7a109`. [Split-ABI preview APK artifact 13951718520](https://github.com/kalaabalb/yomart-mobile/actions/runs/43234715460) targets the production API with preview-only catalogue mode. Emulator screenshots were not captured because the hosted emulator failed to boot. |
| Documentation | [yomart-docs](https://github.com/kalaabalb/yomart-docs) | This repository. |

## Current release boundary

- API runs in **production** on the existing Atlas Free cluster/database. No paid database resource was created and no existing database was deleted. Historical Atlas project/database names still contain “Staging”; see the free-tier decision.
- Latest live health: HTTP 200, success true, environment production, database connected, Cloudinary image storage, authenticated private payment-proof storage, version 1.0.4.
- CORS preflight: HTTP 204 with exact allow-origin for both customer storefront and admin.
- Hosted noindex legal drafts are available at [privacy policy](https://yomart.onrender.com/privacy-policy.html) and [account deletion](https://yomart.onrender.com/account-deletion.html); publisher/support details and retention wording are not finalized.
- Production checkout is fail-closed: `YOMART_SALES_ENABLED=false` is explicitly set. Public catalog APIs omit synthetic price and quantity fields.
- Admin and customer static sites remain staging/noindex builds. The customer storefront displays a preview-catalogue notice and ordering is unavailable.
- Active catalogue has 8 synthetic products with illustrative prices/stock, 2 inactive historical test products, 4 categories, 4 subcategories, 2 brands, 2 collections and 2 posters. All active products remain unverified for public search. Do not sell these as real offers.
- Live database audit found 0 orders and 0 payment-proof records. One admin account and one customer account exist; personal identifiers and credentials are excluded from documentation.
- Cash at shop or handover is the only configured payment method. No online gateway or bank/wallet method is configured.
- Backend suite: 253 tests, 251 passed, 0 failed, 2 optional tests skipped. Live health/catalogue/checkout-settings/CORS smoke passed in run 43233324854.
- Web screenshot artifact contains 12 genuine hosted screenshots: desktop/mobile storefront, product listing/detail, cart, login, support, legal drafts, and admin sign-in. It does not contain an authenticated admin dashboard.
- Android split-ABI debug APKs are available for arm32, arm64 and x86_64 sideload acceptance. The emulator did not boot, so no Android screenshot artifact exists. These are not Play release AABs.
- Play Console listing, upload-key verification, legal publisher/support details, owner-approved catalogue, physical Android-device acceptance, and owner-assisted authenticated admin acceptance remain outstanding.
