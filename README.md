# YoMart Documentation

Operational, technical, release, and tester documentation for the YoMart API, customer storefront, administrator dashboard, and Flutter Android app.

**Snapshot: 12 November 2026.** The customer storefront and admin static sites are deployed on Render. The API deployment is live and reports the staging environment; do not treat the Render environment label as proof of production. Android checkout compatibility fix is merged and the post-merge main-branch CI build has passed, producing a debug APK. This is not a signed Play Store release.

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

## Repositories and deployed addresses

| Component | Source | Current address / evidence |
|---|---|---|
| API | [yomart-api](https://github.com/kalaabalb/yomart-api) | [yomart-api.onrender.com](https://yomart-api.onrender.com) — deployment at commit `0371cbd01fb7b40df05391c49cedd58f3ea1736f`; API health previously reported staging and connected database. |
| Admin | [yomart-admin](https://github.com/kalaabalb/yomart-admin) | [yomart-admin.onrender.com](https://yomart-admin.onrender.com) — Render static site. |
| Customer storefront | [yomart-web](https://github.com/kalaabalb/yomart-web) | [yomart.onrender.com](https://yomart.onrender.com) — Render static site. |
| Android app | [yomart-mobile](https://github.com/kalaabalb/yomart-mobile) | Flutter app, application ID `com.yomobiles.client`. Checkout fix merged as `69c91510c4b26d5c976a46727f28252319e7a109`; [post-merge CI and debug APK artifact](https://github.com/kalaabalb/yomart-mobile/actions/runs/43207373086). |
| Documentation | [yomart-docs](https://github.com/kalaabalb/yomart-docs) | This repository. |

## Current release boundary

- API environment is **staging**. Testers must use staging data and must not enter real payment details, real receipts, or sensitive personal information.
- Staging catalogue has 8 active synthetic sample products plus 2 inactive historical stock/checkout fixtures, 4 categories, 4 subcategories, 2 brands, 2 collections and 2 campaign posters. All products remain unverified for search. Sample prices and quantities are not retail offers.
- Cloudinary holds product/category images and two 1600×600 campaign banners. No payment proofs, receipts or customer private media were seeded.
- Cash on collection is configured in staging. This is manual payment, not an online payment gateway.
- Android PR #11 is merged. Main-branch format, analysis, tests and Android debug build passed; artifact is not a signed Play upload AAB.
- A signed release AAB, upload-key verification, Play Console listing/declarations, privacy and account-deletion URLs, and closed-test setup are separate gates.
- For new personal Play developer accounts created after 13 November 2023, Google requires at least 12 testers continuously opted in to a closed test for 14 days before applying for production access. Recruiting 14 gives a two-person buffer; it does not replace continuous opt-in.

## Evidence conventions

Every readiness statement must name its evidence and date. Distinguish source/test evidence, provider deployment evidence, staging acceptance, owner/legal decisions, and Play Console state. Never label the product production-ready from a successful build alone.
