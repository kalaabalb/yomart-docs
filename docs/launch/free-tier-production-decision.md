# YoMart production infrastructure decision

Decision: use the existing MongoDB Atlas Free cluster. No paid M10 cluster or other paid database resource is approved or required for this cutover.

## Existing infrastructure

- Atlas organization: yomarket.
- Existing project: YoMart-Staging.
- Existing cluster: yomart-staging (Free tier, AWS AP_SOUTH_1).
- Existing database: yomart_staging.
- Existing Render API: https://yomart-api.onrender.com.
- The current catalog and checkout configuration are retained on this cluster.

## Runtime conversion

On 12 November 2026, the Render API environment was updated using a merge-only environment update to set NODE_ENV=production and PLATFORM=Production. Existing secrets and other environment variables were not replaced. Render started deployment dep-dbqqjq6i0phs73df2uj0 to apply this runtime change.

The existing database is being reused as the production data store. Its historical database name may remain yomart_staging during this conversion; do not run a risky rename/migration solely to change the name. The service environment and operational role are being converted to production.

## Free-tier limitations

The Atlas Free tier has constrained storage, compute, backup, and availability characteristics. It is acceptable for a low-volume launch with careful monitoring, but it is not equivalent to a paid production database with managed backup/restore guarantees. Export important data regularly and monitor storage limits. Do not create paid resources without explicit owner approval.

## Remaining release gates

- Confirm Render deployment dep-dbqqjq6i0phs73df2uj0 succeeds.
- Confirm GET /health reports environment production and database connected.
- Confirm exact CORS origins still allow the customer storefront and admin.
- Confirm existing admin login, customer registration/login, public catalog, checkout, inventory, and payment-proof access after the runtime change.
- Confirm the active product catalogue contains real approved prices and stock before public selling. Current staging catalogue values are synthetic review data and are not automatically safe for real sales.
- Complete legal publisher/support details and publish privacy/account deletion pages.
- Build and verify the signed production AAB against the production API and Play upload certificate.
- Capture real screenshots from running Android and web UIs; do not use generated mockups as app screenshots.

## Explicit non-actions

- No paid Atlas cluster is created.
- No existing Free cluster or database is deleted.
- The unused empty YoMart-Production Atlas project is left untouched; it has no cluster and creates no database compute charge.
- No credentials are printed, rotated, or replaced as part of this environment-mode change.
