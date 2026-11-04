# Cloudinary media configuration and privacy

## Backend configuration

The standard SDK settings are `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, and `CLOUDINARY_API_SECRET`. Configure an isolated staging cloud/account mapping in the staging backend only. The owner previously confirmed the staging cloud name matches the staging dashboard; this does not establish provider privacy behavior or isolation of every credential/resource. Never publish secrets in `VITE_*`, docs, shell output, screenshots, or commits.

The backend uses Cloudinary for catalog media when configured; new payment proofs use authenticated delivery and are retrieved through an authorized backend route that returns short-lived signed download bytes/URL internally. The browser should receive image bytes only through the API. No provider URL or storage reference belongs in customer order serialization.

## Acceptance required before real receipts

Use one synthetic asset only in the authorized staging account. Do not enumerate, search, or mutate existing assets. Record the asset identity in a protected recovery manifest before changing access. Verify:

1. Upload delivery type, resource type, immutable asset ID and intended synthetic public ID.
2. Anonymous original retrieval fails.
3. Representative derived/transformed retrieval fails anonymously.
4. Backend-authorized owner and permitted active admin retrieval succeeds; unrelated customer, moderator and inactive admin fail.
5. Signed retrieval succeeds within its expiry and fails after expiry; backend refresh behavior is bounded and no link is stored in a browser response longer than necessary.
6. Restriction/restoration and cache invalidation behavior using only the test asset, followed by deletion only after identity and recovery checks.
7. Provider/quota/timeout failure is closed and does not fall back to a public URL.

The POC interrupted after creating a synthetic asset in a shared environment; local recovery evidence must be resolved before any new upload. Do not use asset search to guess its identity. No real legacy receipt should be fetched for testing.

Cloudinary documents `upload`, `private`, and `authenticated` delivery types; `authenticated` protects originals and derived assets. Signed private downloads and token/cookie access are distinct mechanisms. Token/cookie-based access requires Advanced or higher according to Cloudinary's media-access documentation; the backend's signed-download approach still needs a real synthetic test. Free plan currently has 25 monthly credits across transformations/storage/bandwidth; monitor usage and verify account billing before tests. Sources: [upload delivery types](https://cloudinary.com/documentation/upload_parameters), [media access](https://cloudinary.com/documentation/control_access_to_media), [billing](https://cloudinary.com/documentation/billing_and_plans).

## Catalog media and filesystem

Public catalog images may be publicly delivered; receipt media must not share a public delivery type or static path. Render Free filesystems are ephemeral, so uploads must use durable provider storage. Separate catalog folders from private payment-proof namespaces, but remember that folder names do not create access control.
