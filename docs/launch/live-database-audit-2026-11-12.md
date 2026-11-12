# YoMart live database audit

Audit date: 12 November 2026  
Scope: active Atlas Free cluster currently used by the Render API.

## Infrastructure

- Atlas project: `YoMart-Staging` (historical name).
- Cluster: `yomart-staging`, Free tier, AWS `AP_SOUTH_1`.
- Database: `yomart_staging` (historical name retained; do not rename during cutover).
- Render API: `https://yomart-api.onrender.com`.
- Runtime: `NODE_ENV=production`, `PLATFORM=Production`.
- Live health and CORS smoke workflow [43224380086](https://github.com/kalaabalb/yomart-api/actions/runs/43224380086) passed at 12 November 2026 12:53 UTC.
- Live `GET /health`: HTTP 200, `success:true`, `environment:production`, `platform:Production`, `database:connected`, Cloudinary image storage, authenticated Cloudinary private payment-proof storage, app version `1.0.4`.
- CORS preflight returned HTTP 204 with exact allow-origin for `https://yomart.onrender.com` and `https://yomart-admin.onrender.com`.
- Production checkout is disabled using `YOMART_SALES_ENABLED=false` until real prices and stock are owner-approved.

## Database inventory

The active database contains 20 collections and approximately 1.44 MB of data.

| Collection | Documents |
|---|---:|
| products | 10 |
| categories | 4 |
| subcategories | 4 |
| brands | 2 |
| collections | 2 |
| posters | 2 |
| adminusers | 1 |
| users | 1 |
| storesettings | 1 |
| orders | 0 |
| paymentproofs | 0 |
| variants | 0 |
| ratings | 0 |
| retailercontacts | 0 |
| notifications | 0 |
| stories | 0 |
| securityaudits | 0 |
| proofcleanupjobs | 0 |
| orderidempotencies | 0 |
| varianttypes | 0 |

No customer orders or payment proofs exist. One admin account and one customer account exist; personal identifiers and credential fields are intentionally excluded from this report.

## Catalogue and checkout

- Eight products are active and two old staging test products are inactive.
- Categories now use clean customer-facing labels: Smartphones, Tablets, Audio, Computers & Wearables. The earlier visible `STAGING SAMPLE —` prefix was removed from category names.
- All eight active products have `isSearchVerified:false`.
- Their `catalogSource.note` identifies prices and stock as synthetic review data, not retail offers.
- Product price and quantity fields remain illustrative. They are not owner-approved selling prices or verified available stock.
- The checkout settings record has one enabled payment method: Cash on collection.
- It requires no payment proof. The method text says payment is made to YoMart staff when the customer receives the order.
- No bank transfer or online payment gateway is configured.
- New order creation is blocked at the API until the owner explicitly enables sales after approving prices and stock.
- Public `GET /products` and `GET /products/:id` responses omit `price`, `offerPrice`, and `quantity` while sales are disabled. Authenticated admin product responses retain these fields. Live production smoke verifies both public catalog endpoints.

## Required release treatment

Do not accept real customer orders against the current synthetic price/stock catalogue. Before public selling, the owner must approve the price and stock for each active product, confirm payment/fulfilment wording, and confirm whether the catalogue should remain visible to closed testers while approvals are pending.

Do not create synthetic customer orders, payment proofs or reviews to make the database appear populated. Closed-test activity must be genuine and clearly identified.

## Security and data handling

- The database contains no orders or payment-proof records to expose at this snapshot.
- Payment-proof retrieval controls and inventory reservation lifecycle changes are covered by the backend implementation and automated tests; live authenticated end-to-end flows still need controlled verification.
- Do not include secrets, database URIs, customer identifiers or admin credentials in screenshots or release documents.
