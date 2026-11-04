# Staging acceptance checklist

Use synthetic records and accounts only. A staging pass is not production approval.

## Identity and isolation

- [ ] API deployment commit and `NODE_ENV` confirmed; health says database connected.
- [ ] Database/cluster are the separate staging target and only synthetic data is present.
- [ ] Staging admin/customer secrets are unique and never reused in production.
- [ ] Staging Cloudinary cloud identity confirmed; credentials are distinct from production.
- [ ] Render, Atlas, Cloudinary billing/usage settings reviewed for $0; no paid add-ons, auto-overages, payment method or backup subscription has been enabled without approval.
- [ ] Exact assigned API/admin/customer HTTPS origins confirmed. Backend CORS contains only approved origins. Build expected origins match deployed sites. Staging output has noindex/nofollow/noarchive and no stale/production API origin.

## API and authorization

- [ ] Health and read-only checkout settings work; no writes during preflight.
- [ ] Direct API tests cover anonymous, customer owner, unrelated customer, moderator, active admin, super-admin and inactive admin.
- [ ] Customers cannot change order IDs to read another customer's order/proof; moderators cannot retrieve customer/proof data.
- [ ] Product/catalog data is server-backed; order totals/stock are server-authoritative.
- [ ] Idempotency index exists before synthetic order test; same key retry returns same order and changed payload is rejected.
- [ ] Destination phone/city requirements, optional area/coordinates, zero fee, staff-assigned collection point, Ready for Collection and Collected states are verified.

## Media and payment proof

- [ ] One synthetic proof only; anonymous original and transformed Cloudinary paths denied.
- [ ] Authorized owner/permitted active admin retrieval works; unrelated customer, moderator and inactive admin are denied.
- [ ] Expired signed download denied and backend refresh tested; no URL appears in API JSON/logs.
- [ ] Failed storage/provider operations fail closed; upload limits/actual format checked.
- [ ] Interrupted POC manifest is reconciled before any new upload. Exact synthetic asset identity and metadata independently verified before cleanup; no prefix/folder deletion.
- [ ] Legacy public receipt exposure plan approved and provider/CDN consequences addressed before real receipt collection.

## Customer and admin UI

- [ ] Public browsing/home/catalog/search/category/product images and missing-image states.
- [ ] Guest cart persistence; auth redirect preserves cart and intended destination; logout/session expiry protects account/order routes.
- [ ] Empty checkout methods state blocks submission without inventing payment values.
- [ ] Order history/details are customer-scoped; status/collection wording is accurate.
- [ ] Admin catalog, contacts, payment settings, orders, collection points and payment review show success/error/empty states and enforce server permissions.
- [ ] Desktop, tablet, Android, Flutter web if applicable; direct refresh of product/order pages; console/network errors recorded.

## Stories, recovery and operations

- [ ] Set `STORY_MAX_DURATION_SECONDS=15` only for staging testing; public story publication remains disabled. Keep one backend process while upload feature is enabled.
- [ ] Test upload/playback/seek, publish/unpublish/delete, retry and expiry on Android/web with synthetic videos ≤15 seconds and ≤10 MiB.
- [ ] Verify 5-minute signed-link residual access window; do not claim instant revocation. Evaluate supported mitigation before any publication.
- [ ] Provider quota failures have safe useful errors; posters/payment-proof flows remain unaffected.
- [ ] Test a database backup/restore into an isolated target and reconcile its media references; record recovery time/checksum.
- [ ] Verify monitoring, incident contact, rate limits, health and log redaction.
- [ ] Owner/legal approval recorded for retention periods, deletion text, legacy proof migration and public story launch.

Record tester, timestamp, deployed commit, environment label and evidence per item. Never put personal customer data, receipt images, signed URLs, tokens or secrets in the report.
