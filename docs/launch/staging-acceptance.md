# Historical staging acceptance checklist

**Status as of 12 November 2026: do not execute this checklist against the currently active database.**

The former Atlas staging project/cluster/database was converted into the active production data path on 12 November 2026. The Render API now runs with `NODE_ENV=production` and `PLATFORM=Production` against the existing Free cluster/database. There is no separate staging database or isolated staging API at this time.

This checklist is retained as a future template only. Do not run synthetic write tests, create fake customers/orders, upload test receipts, change stock, test cancellation, or run cleanup jobs against the active production database. Read-only checks only unless the owner approves a controlled live test and the effect on actual business data is understood.

## Before reusing this checklist

- [ ] Provision a separate, isolated staging database/service using only a free resource or an owner-approved budget.
- [ ] Give staging its own unique credentials, JWT secret, exact CORS origins and separate storage paths.
- [ ] Seed clearly synthetic data into staging only.
- [ ] Verify that staging builds are noindex and cannot point at the production API.
- [ ] Verify that production builds cannot point at the staging API.
- [ ] Confirm no real customer details, receipts, tokens or secrets are copied into staging.

## Identity and isolation

- [ ] API deployment commit and `NODE_ENV` confirmed; health says database connected.
- [ ] Database/cluster are the isolated staging target and only synthetic data is present.
- [ ] Staging admin/customer secrets are unique and never reused in production.
- [ ] Exact assigned API/admin/customer HTTPS origins confirmed. Backend CORS contains only approved origins. Build expected origins match deployed sites.
- [ ] Staging output has noindex/nofollow/noarchive and no production API origin.

## API and authorization

- [ ] Health and read-only checkout settings work.
- [ ] Direct API tests cover anonymous, customer owner, unrelated customer, moderator, active admin, super-admin and inactive admin.
- [ ] Customers cannot change order IDs to read another customer's order/proof; moderators cannot retrieve customer/proof data.
- [ ] Product/catalog data is server-backed; order totals/stock are server-authoritative.
- [ ] Idempotency retry returns the same order and changed payload is rejected.
- [ ] Destination phone/city requirements, optional area/coordinates, zero fee, staff-assigned collection point, Ready for Collection and Collected states are verified.

## Media and payment proof

- [ ] Use synthetic proof only; anonymous original and transformed paths denied.
- [ ] Authorized owner/permitted active admin retrieval works; unrelated customer, moderator and inactive admin are denied.
- [ ] Expired signed download denied and backend refresh tested; no URL appears in API JSON/logs.
- [ ] Failed storage/provider operations fail closed; upload limits and actual format checked.
- [ ] Cleanup is dry-run first and exact synthetic asset IDs are independently verified.
- [ ] No prefix/folder deletion or destructive cleanup without a documented retention decision.

## Customer and admin UI

- [ ] Public browsing/home/catalog/search/category/product images and missing-image states.
- [ ] Guest cart persistence; auth redirect preserves cart and intended destination; logout/session expiry protects account/order routes.
- [ ] Empty checkout methods state blocks submission without inventing payment values.
- [ ] Order history/details are customer-scoped; status/collection wording is accurate.
- [ ] Admin catalog, contacts, payment settings, orders, collection points and payment review show success/error/empty states and enforce server permissions.
- [ ] Desktop, tablet and Android layouts; direct refresh of product/order pages; console/network errors recorded.

## Operations and recovery

- [ ] Test a database backup/restore into an isolated target and reconcile media references.
- [ ] Verify monitoring, incident contact, rate limits, health and log redaction.
- [ ] Owner/legal approval recorded for retention periods, deletion text and public story launch.

Record tester, timestamp, deployed commit, environment label and evidence per item. Never put personal customer data, receipt images, signed URLs, tokens or secrets in the report.
