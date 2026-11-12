# YoMart Documentation

Operational, technical, release, and tester documentation for the YoMart API, customer storefront, administrator dashboard, and Flutter Android app.

**Snapshot: 12 November 2026.** The customer storefront and admin static sites are deployed on Render. The API deployment is live and reports the staging environment; do not treat the Render environment label as proof of production. Android checkout compatibility work is in CI; a debug build is not a Play Store release.

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
- [Closed-testing playbook for 14 testers / 15 days](docs/launch/closed-testing-playbook.md)

## Repositories and deployed addresses

| Component | Source | Current address / evidence |
|---|---|---|
| API | [yomart-api](https://github.com/kalaabalb/yomart-api) | [yomart-api.onrender.com](https://yomart-api.onrender.com) — Render deployment at commit `0371cbd01fb7b40df05391c49cedd58f3ea1736f`; API health previously reported staging and connected database. |
| Admin | [yomart-admin](https://github.com/kalaabalb/yomart-admin) | [yomart-admin.onrender.com](https://yomart-admin.onrender.com) — Render static site. |
| Customer storefront | [yomart-web](https://github.com/kalaabalb/yomart-web) | [yomart.onrender.com](https://yomart.onrender.com) — Render static site. |
| Android app | [yomart-mobile](https://github.com/kalaabalb/yomart-mobile) | Flutter app, application ID `com.yomobiles.client`. Checkout compatibility fix is being verified in GitHub Actions; no signed Play upload AAB is certified here. |
| Documentation | [yomart-docs](https://github.com/kalaabalb/yomart-docs) | This repository. |

## Current release boundary

- The API's current environment is **staging**. Testers must use staging data and must not enter real payment details, real receipts, or sensitive personal information.
- The staging catalog currently contains clearly labelled synthetic test records. Replace or extend these only with owner-approved, accurately described listings and images before inviting external testers.
- Cash on collection is configured in staging. This is manual payment, not an online payment gateway.
- Android CI must pass on the final PR head and produce a verified artifact before merge. A debug APK is not a Play upload AAB.
- A signed release AAB, upload-key verification, Play Console listing/declarations, privacy and account-deletion URLs, and closed-test setup are separate gates.
- For new personal Play developer accounts created after 13 November 2023, Google requires at least 12 testers continuously opted in to a closed test for 14 days before applying for production access. Recruiting 14 gives a two-person buffer; it does not replace the continuous opt-in requirement.

## Evidence conventions

Every readiness statement must name its evidence and date. Distinguish source/test evidence, provider deployment evidence, staging acceptance, owner/legal decisions, and Play Console state. Never label the product production-ready from a successful build alone.
