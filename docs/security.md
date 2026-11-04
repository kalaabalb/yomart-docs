# Security and privacy

## Authentication and authorization

Customers and admins use JWT bearer tokens (`Authorization: Bearer ...`). Admin authorization checks active account state and role on the API; client navigation is not an access control. Current roles are `moderator`, `admin`, and `super_admin`. Moderators are denied customer/order details and receipt bytes. Active admins and super-admins have the documented order/payment review capabilities; super-admin-only operations remain separately protected. Customer order/detail/proof access is ownership-scoped.

The React storefront holds the access token in JavaScript memory and clears it on reload/logout; there is no token refresh. Browser protected routes are UX only—the backend must enforce every access check.

## Payment proofs

- New proofs are accepted through authenticated, validated upload routes, normalized/validated server-side, stored as authenticated Cloudinary assets or configured private filesystem assets, and associated through database records. The backend returns an opaque proof-record ID, not provider URLs.
- Authorized retrieval is server-mediated. It checks customer ownership or active permitted admin role; moderators and inactive admins are denied. API order serialization strips provider URLs and internal storage references.
- Legacy Cloudinary proof references can be intentionally unavailable through the API while their old provider URLs may remain anonymously accessible. This is a **critical public-launch blocker** until inventory/restriction/migration and anonymous denial are proven.
- Never use a public catalog-image URL as a receipt URL; do not proxy arbitrary stored URLs. Backend provider failure must fail closed. Signed URLs are bearer secrets; do not log, persist, expose to customer JSON, or add them to screenshots.
- Verify originals and derived assets, expiry, refresh, CDN caching/invalidation, and recovery with one synthetic asset. No synthetic test result proves all historical assets have been restricted.

## Uploads and storage

The backend caps supported image uploads at 10 MiB and normalizes supported image bytes; story video uploads also have a 10 MiB cap. MIME/extension claims must be checked against actual bytes. Public static upload serving is restricted to allowlisted public catalog paths; payment-proof files are not served through the public `/uploads` route. Public and private local roots must not overlap; symlinks/path traversal fail closed. Do not put uploads on Render Free local filesystem.

## Retention and account deletion

Account deletion removes direct customer identifiers and revokes customer access to receipts, while anonymized order/payment evidence is retained. Customer wording that says orders/evidence are “deleted” is inaccurate and needs owner/legal review. Both orphan and superseded-proof retention periods remain unset; changing them or running cleanup requires explicit owner/legal approval and evidence-preserving review.

## CORS, secrets and builds

- Exact HTTPS origins only in staging/production; no wildcard.
- Frontend build settings are public. Never put JWT secrets, DB URI, admin passwords, Cloudinary API secret, OneSignal REST key, signing keys, or tokens in browser variables or Git.
- Use separate secrets and least-privilege database users by environment. Set local secret files to owner-only permissions and ensure Git ignores them.
- Gitleaks and dependency audits are useful checks, not proof that Git history, old releases, provider assets, backups, or external forks are clean.
