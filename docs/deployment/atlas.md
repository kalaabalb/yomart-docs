# MongoDB Atlas staging and recovery

## Staging isolation

Use the dedicated `YoMart-Staging` project/cluster and database `yomart_staging`, with a separate least-privilege application database user. The intended staging user name is `yomart_staging_app`; usernames are not credentials. Confirm the cluster and database from the local URI parser or provider UI without printing the URI. Never copy production users or records. Stage only synthetic catalog, customers, orders and receipts.

The backend's transaction-protected order/payment-proof flows require a replica set. Atlas Free clusters are replica sets, but have strict storage/connection/command limits, no private endpoints/network peering, no Atlas backups by default, and limited observability. Render-to-Atlas access may rely on shared public outbound ranges. Keep the access list narrow; never use `0.0.0.0/0` as a connectivity workaround.

## Safe connection checks

1. Inspect backend health at `GET /health`; do not treat an HTTP response as proof of the intended DB target.
2. Confirm the service's configured DB name/cluster through a sanitized configuration check, and verify an application-level health result reports connected.
3. Before an order acceptance test, use the read-only index verifier documented in the API repository to confirm required unique idempotency indexes; do not create an index by submitting a test order.
4. Test transaction behavior only with synthetic staging data after an approved test window.

## Backup and restore

Free Atlas clusters do not include the standard automated backup feature. MongoDB documents `mongodump`/`mongorestore` as a manual alternative. Store backups encrypted, access-controlled and outside the source repository; never put database dumps in this public docs repo. Record a backup checksum, database/cluster identity, timestamp, operator, and restore test. Restore only to a separate non-production target, then validate synthetic record counts and app health. Independent database and Cloudinary recovery points can mismatch: preserve a reconciliation manifest for media references and test both data sets before accepting writes.

Atlas also documents an optional Free-cluster backup preview that is billed per day and requires a payment method; it is not compatible with the strict $0/month policy. Recheck the current Atlas dashboard and docs before any change. Free-cluster limits and backup choices can change: [limits](https://www.mongodb.com/docs/atlas/reference/free-shared-limitations/), [backup/restore](https://www.mongodb.com/docs/atlas/backup-restore-cluster/), [Free backups](https://www.mongodb.com/docs/atlas/backup/cloud-backup/free-cluster-backup/).

Do not access production data, run migration/bootstrap/seed jobs, or use staging credentials in production.
