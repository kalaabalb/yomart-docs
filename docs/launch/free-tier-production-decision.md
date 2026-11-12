# YoMart production infrastructure decision

Decision: use the existing MongoDB Atlas Free cluster. No paid M10, Flex, or other paid database resource is approved or required.

## Active infrastructure

- Atlas project: YoMart-Staging (historical name).
- Atlas cluster: yomart-staging (Free tier, AWS AP_SOUTH_1).
- Database: yomart_staging (historical name; now active production data store).
- API: https://yomart-api.onrender.com
- Storefront: https://yomart.onrender.com
- Admin: https://yomart-admin.onrender.com

## Runtime conversion and live verification

The Render API was changed to `NODE_ENV=production` and `PLATFORM=Production` using a merge-only environment update. Existing secrets and unrelated variables were not replaced.

- Production checkout protection was deployed, with `YOMART_SALES_ENABLED=false`.
- Live production smoke workflow [43222001537](https://github.com/kalaabalb/yomart-api/actions/runs/43222001537) passed.
- Live `GET /health`: HTTP 200, `success:true`, environment `production`, platform `Production`, database `connected`, Cloudinary image storage and authenticated private payment-proof storage, version `1.0.4`.
- Exact CORS preflights: HTTP 204 for both `https://yomart.onrender.com` and `https://yomart-admin.onrender.com`.

The existing database is now the active production data store. Its historical name may remain `yomart_staging`; do not rename it solely for appearance or create a paid replacement.

## Current business and release boundary

- Eight active products contain synthetic illustrative prices/stock, not approved retail offers.
- Production checkout is disabled and returns HTTP 503 until the owner approves real prices and stock.
- Cash on collection is the only configured method; no online gateway or bank/wallet method exists.
- There are zero orders and zero payment-proof records. No fake orders or receipts were created.
- Customer storefront and admin remain staging/noindex builds while catalogue and legal details are pending.
- Real desktop/mobile screenshots for storefront and admin have been captured. Android emulator screenshot workflow is still in progress.

## Free-tier limitations

Atlas Free has constrained storage, compute, backup and availability. Monitor usage and export important data regularly. Do not enable paid resources, auto-overages or paid add-ons.

## Remaining owner decisions and release gates

- Approve actual ETB price, confirmed available stock, exact model/condition/warranty, and sale availability for each active product.
- Confirm whether cash collection means shop pickup, staff handover/delivery, or both.
- Supply legal publisher/business name, monitored support email and public business contact details.
- Approve account deletion/retention language. Current implementation pseudonymizes some order/payment evidence; it does not erase every record.
- Verify the release keystore certificate against the Play Console upload-key certificate. Keep private key/password local.
- Build a signed production AAB, test physical devices, complete Play listing/declarations and publish the closed-test track.
- Confirm Play developer account type and creation date to establish whether the 12-tester/14-day gate applies.

## Explicit non-actions

- No paid Atlas cluster was created.
- No existing Free cluster or database was deleted.
- The unused empty YoMart-Production project remains untouched; it has no cluster and no database compute charge.
- No credentials were printed, rotated or replaced as part of this environment-mode change.
