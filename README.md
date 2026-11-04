# YoMart Documentation

Public operating and implementation documentation for the YoMart API, customer storefront, administrator dashboard, and Flutter customer app.

> **Status snapshot: 5 November 2026.** The API and admin have published repositories and are reported as deployed. The React customer storefront is published at [`kalaabalb/yomart-web`](https://github.com/kalaabalb/yomart-web), commit `f7f2350cbdb50beb4b4ad774a91b82eb4f5f666e`; its Render deployment has not been verified. The service URLs below are deployment addresses, not proof that the corresponding backend environment is production. No production customer-site origin has been confirmed.

## Start here

- [Architecture and system boundaries](docs/architecture.md)
- [Deployment guides](docs/deployment/README.md)
- [Environment variable reference](docs/deployment/environment-variables.md)
- [Operations and recovery](docs/operations.md)
- [Security and privacy](docs/security.md)
- [Customer workflow](docs/workflows/customer.md)
- [Administrator workflow](docs/workflows/admin.md)
- [Launch readiness and blockers](docs/launch/readiness.md)
- [Staging acceptance plan](docs/launch/staging-acceptance.md)

## Repositories and current service addresses

| Component | Source repository | Current address/status |
|---|---|---|
| API | [yomart-api](https://github.com/kalaabalb/yomart-api) | [`https://yomart-api.onrender.com`](https://yomart-api.onrender.com), reported deployed; environment classification and runtime health must be checked before each acceptance window. |
| React admin | [yomart-admin](https://github.com/kalaabalb/yomart-admin) | [`https://yomart-admin.onrender.com`](https://yomart-admin.onrender.com), reported deployed; runtime behavior is not independently established by this documentation publication. |
| React customer storefront | [yomart-web](https://github.com/kalaabalb/yomart-web) | Requested address [`https://yomart.onrender.com`](https://yomart.onrender.com) is **not confirmed as assigned or deployed**. |
| Flutter customer app | [yomoblies](https://github.com/kalaabalb/yomoblies) | Android client; Play Store release is a separate process. |

The React storefront build and documentation publishing checks are local/repository checks. They do not constitute a live checkout, Cloudinary privacy, database backup, or Google indexing acceptance test.

## Important release gates

- Legacy payment-proof Cloudinary assets may still be publicly accessible through old URLs. Hiding those URLs in API responses does not revoke the assets. Do not accept real receipts until restriction/migration and anonymous denial are verified.
- Cloudinary original/derived access, temporary signed retrieval and expiry must be verified using a synthetic asset in the authorized staging cloud. Do not assume SDK behavior proves provider behavior.
- Payment-evidence retention and account-deletion wording require owner/legal approval. Retention variables must remain unset until approved; orders/payment evidence are anonymized and retained by account deletion, not erased.
- The current product API has no safe per-variant price/stock contract. Products with configured variants are blocked from online purchase until that contract is implemented.
- Storefront site URL, production API/site origin inventory, exact CORS entries, billing safeguards, backup/restore, and hosted acceptance need explicit verification. Never infer production origins from historical records.
- Current application code provides no payment gateway integration. COD/manual payment review is not equivalent to automated payment processing.

## Documentation conventions

“Code verified” means a behavior was confirmed in published source/tests. “Owner-reported” is information supplied by the project owner. “Staging required” means a provider- or browser-level check is still needed. “Production blocked” marks a release gate, not a claim that a production environment currently exists.

This documentation repository does not yet declare a license. Public visibility alone does not grant permission to reuse its contents; select a license before encouraging reuse.
