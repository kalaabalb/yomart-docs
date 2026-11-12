# YoMart production infrastructure decision

Decision: use the existing MongoDB Atlas Free cluster. No paid M10, Flex, or other paid database resource is approved or required.

## Active infrastructure

- Atlas project: YoMart-Staging (historical name).
- Atlas cluster: yomart-staging (Free tier, AWS AP_SOUTH_1).
- Database: yomart_staging (historical name; now the active production data store).
- API: https://yomart-api.onrender.com
- Storefront: https://yomart.onrender.com
- Admin: https://yomart-admin.onrender.com

## Runtime conversion and live verification

The Render API was changed to `NODE_ENV=production` and `PLATFORM=Production` using merge-only environment updates. Existing secrets and unrelated variables were not replaced.

- `YOMART_SALES_ENABLED=false` is explicitly set in Render; new order creation remains disabled until actual prices and stock are owner-approved.
- Production deployment `dep-dbqscvjm8hqs73eo6p50` confirmed production runtime and MongoDB connection. The latest documentation-only API deployment `dep-dbqsegivcj2c73fcukh0` also went live and startup logs again confirmed production and MongoDB connected.
- Live production smoke [workflow 43233076575](https://github.com/kalaabalb/yomart-api/actions/runs/43233076575) passed after the explicit sales-flag deployment.
- Live `GET /health`: HTTP 200, `success:true`, environment `production`, platform `Production`, database `connected`, Cloudinary image storage and authenticated private payment-proof storage, version `1.0.4`.
- Public catalogue smoke verified 8 products and confirmed illustrative price/stock fields are omitted from both list and detail responses.
- Public checkout settings verified one enabled method: cash at shop or handover; proof not required; delivery fee zero.
- Exact CORS preflights returned HTTP 204 for both `https://yomart.onrender.com` and `https://yomart-admin.onrender.com`.
- Backend suite: 253 tests, 251 passed, 0 failed, 2 optional tests skipped.

The existing database is now the active production data store. Its historical name may remain `yomart_staging`; do not rename it solely for appearance or create a paid replacement.

## Current business and release boundary

- Eight active products contain synthetic illustrative prices/stock, not approved retail offers.
- Production checkout is disabled until the owner approves real prices and stock.
- Cash at shop or handover is the only configured method; no online gateway or bank/wallet method exists.
- There are zero orders and zero payment-proof records. No fake orders or receipts were created.
- Customer storefront and admin remain staging/noindex builds while catalogue and legal details are pending.
- Genuine hosted desktop/mobile storefront and admin sign-in screenshots are captured in the [web CI artifact from run 43229601660](https://github.com/kalaabalb/yomart-web/actions/runs/43229601660). Authenticated admin pages are not included because an authorized owner session is required.
- Android preview build and emulator screenshot are being rebuilt in [mobile CI run 43232961622](https://github.com/kalaabalb/yomart-mobile/actions/runs/43232961622). The CI now targets the production API in preview-only mode and bounds optional emulator screenshot capture time.

## Free-tier usage snapshot — 12 November 2026

- MongoDB Atlas: existing cluster is Free tier; no M10/Flex or paid cluster was created.
- Render API service: `plan=free`, one instance. Customer storefront and admin are Render static sites.
- Cloudinary: account plan `Free`; usage report dated 11 November shows 0.21 of 25 credits used (0.84%), 75 assets, approximately 178.6 MB storage, approximately 18.5 MB bandwidth, 170 requests and 19 transformations.
- No paid resource or paid add-on was enabled as part of this cutover.

## Free-tier limitations

Atlas Free has constrained storage, compute, backup and availability; Render Free may sleep/cold-start; Cloudinary Free has finite credit, storage and bandwidth allowances. Monitor usage and export important data regularly. Do not enable paid resources, auto-overages or paid add-ons.

## Remaining owner decisions and release gates

- Approve actual ETB price, confirmed available stock, exact model/condition/warranty, and sale availability for each active product before enabling sales.
- Supply legal publisher/business name, monitored support email and public business contact details.
- Approve account deletion/retention language. Current implementation pseudonymizes some order/payment evidence; it does not erase every record.
- Add the Android signing secrets and final privacy URL variable described in [owner inputs required](owner-inputs-required.md); compare the resulting certificate with the Play Console upload-key certificate.
- Confirm package ID/version availability in the friend's Play Console.
- Confirm Play developer account type and creation date to establish whether the 12-tester/14-day rule applies.
- Install the build on a physical Android device and complete owner-assisted admin acceptance.

## Explicit non-actions

- No paid Atlas cluster was created.
- No existing Free cluster or database was deleted.
- The unused empty YoMart-Production project remains untouched; it has no cluster and no database compute charge.
- No credentials were printed, rotated or replaced as part of this environment-mode change.
