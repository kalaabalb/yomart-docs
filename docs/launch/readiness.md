# YoMart launch readiness

**Updated:** 12 November 2026  
**Disposition:** API runtime is now configured as production on the existing Atlas Free cluster. The static web clients remain staging/noindex builds. Android debug CI passes; a signed Play AAB and Play Console test release do not yet exist. Real sales are blocked by synthetic catalogue prices/stock and unfinished legal owner details.

## Verified state

| Area | Evidence | Status |
|---|---|---|
| API deployment | Render deployment `dep-dbqqjq6i0phs73df2uj0` succeeded at 11:59 UTC. Boot logs show `Environment: production`, MongoDB connected, and subsequent public catalogue GET requests. | Runtime switch verified. A direct live `GET /health` JSON response still needs to confirm `success:true`, `environment:production`, and `database:connected`. |
| Database | Existing Atlas Free cluster `yomart-staging`, project `YoMart-Staging`, database `yomart_staging`. 20 collections; 1.44 MB. No paid cluster created and no database deleted. | This is now the active production data store. Historical names remain. There is no separate staging database. |
| Data | 10 products (8 active, 2 inactive), 4 categories, 4 subcategories, 2 brands, 2 collections, 2 posters, 1 admin account, 1 customer account, 1 settings record, 0 orders, 0 payment proofs. | No real order/payment evidence yet. No synthetic customers or orders were created for this audit. |
| Catalogue | All 8 active products have `isSearchVerified:false` and source notes explicitly mark their prices/stock as synthetic closed-test data. | Do not take real customer orders or mark as sale offers until owner approves actual prices and stock. |
| Payment | One enabled method: Cash on collection; no proof required. | Manual cash payment at handover; no online gateway or bank/wallet method. |
| Admin | Existing admin login was reported working before the runtime switch. One admin record exists. | Re-test login and role boundaries against the production runtime before owner access is relied on. |
| Storefront | Render static site deployed at `https://yomart.onrender.com`, commit `e4c443c1b7d4049f9a5812d4956d797d348934cc`. | Still a staging/noindex build. |
| Admin web | Render static site deployed at `https://yomart-admin.onrender.com`, commit `5151cbcfd7d51f7e6eccfe4df0894faa9e11da65`. | Still a staging build. |
| Mobile | Flutter main CI run `43210871913` passed; debug APK artifact uploaded. App ID `com.yomobiles.client`, version `1.0.0+5`, target SDK 36. | Debug APK is not a signed Play AAB. Latest change adds real emulator screenshot capture to CI; verify run before claiming artifact. |
| Backend | Inventory reservation lifecycle commit `0371cbd01fb7b40df05391c49cedd58f3ea1736f` is live; backend CI run `43108691199` passed. | Automated tests pass; live transactional checkout not yet exercised against production. |
| Docs | Free-tier decision, corrected cutover runbook, live database audit and product approval CSV are committed. | This readiness document tracks remaining gates. |

## Immediate technical gates

- [ ] Obtain live `GET /health` response confirming production, database connected, and success true.
- [ ] Verify exact CORS preflight for `https://yomart.onrender.com` and `https://yomart-admin.onrender.com` after runtime change.
- [ ] Verify admin login, customer auth, catalog endpoints, and admin role restrictions on production.
- [ ] Verify storefront and admin are intentionally configured for closed testing and remain noindex until legal/catalog approvals. Do not accidentally publish sample data to search engines.
- [ ] Verify live browser flows at desktop/mobile widths. CI now captures real deployed web screenshots; verify its artifact before marking this complete.
- [ ] Verify Android emulator screenshot CI artifact. Physical-device acceptance is still required.
- [ ] Verify private payment-proof upload/retrieval authorization using a synthetic proof only after a safe test plan exists. Do not use real receipts for testing.
- [ ] Confirm free-tier storage/availability constraints and export/restore process; do not create paid resources.

## Owner inputs required before real selling or Play submission

- [ ] Approve actual ETB price and available stock for each active product using [catalog-owner-approval.csv](catalog-owner-approval.csv).
- [ ] Confirm product authenticity, condition, warranty, exact model/storage/color and whether each product is actually available for sale.
- [ ] Provide the legal publisher/business name, monitored support email, and public business contact/address to complete privacy and Play listing details.
- [ ] Approve privacy/account deletion retention language. Current implementation pseudonymizes some order/payment records; it does not erase every record.
- [ ] Confirm whether cash on collection means shop pickup, staff handover/delivery, or both; confirm any service area and whether delivery is offered.
- [ ] Provide the friend’s Play Console account type/creation date and confirm the app package ID is available to that account.
- [ ] Compare the release keystore certificate with the Play Console upload-key certificate. The private key/password must remain local and must never be sent in chat.
- [ ] Approve whether the eight synthetic products should remain visible to closed testers while prices/stock are pending. They must not be sold as real offers.

## Play release gates

- [ ] Build signed production AAB using verified production API and privacy URL.
- [ ] Confirm package ID, incremented version code, target SDK, release manifest, and upload signing certificate.
- [ ] Install Play-delivered internal/closed test build on physical Android devices.
- [ ] Complete accurate store listing, feature graphic, phone screenshots, support details, app access, content rating, target audience, ads and Data Safety declarations.
- [ ] Publish closed-testing track only after catalogue and checkout behavior are safe for real users.
- [ ] If the account is a new personal developer account created after 13 November 2023, maintain at least 12 opted-in testers continuously for 14 days before applying for production access. Verify account-specific applicability in Play Console.

## Safety boundary

The existing Atlas Free cluster is the production database now. Do not run synthetic write tests, stock mutations, dummy orders, fake payment uploads, or destructive cleanup against it. Use read-only verification unless a controlled live test has been explicitly planned and approved. No paid infrastructure is authorized.
