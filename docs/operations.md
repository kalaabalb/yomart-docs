# Operations and support

## Health and logs

- Check API `GET /health`. A healthy HTTP response should include application/database status; verify the database status, not merely a reachable proxy response.
- On Render, check deployment event/commit, instance logs, restart/sleep state, environment names (never copy values into tickets), and request timestamps. Free web services may sleep after inactivity; allow for cold starts and distinguish a Render proxy 502 from an application status using Render event/runtime logs and the response headers/body available to an authorized operator.
- Correlate failures with request ID/time and route. Never log credentials, bearer tokens, database URI, raw receipt content, or signed media URLs.

## CORS and browser preflight

Backend `CORS_ALLOWED_ORIGINS` is a comma-separated exact-origin allowlist in staging/production. Include the exact HTTPS admin and customer origins (origin only: scheme/host/optional port; no path, wildcard, credentials, query or fragment). The published backend mounts global CORS before request rate limiting, and rate limiters skip OPTIONS while retaining login POST limits. An allowed-origin rate-limit response should still carry CORS headers. Do not solve a failed preflight by allowing `*`.

If a browser reports CORS plus 502, a blocked preflight means the POST was not sent and credentials were not evaluated. Check API health, Render logs around the timestamp, exact CORS origin, OPTIONS method/headers, proxy status and upstream readiness. A local rate-limit regression test does not explain or clear every hosted 502.

## Free-tier and availability monitoring

Render Free web services sleep after idle time, use ephemeral filesystems, and may be suspended when included hours/bandwidth are exhausted. Static site bandwidth/build usage also has limits. Atlas Free has limited capacity, no private endpoint, no standard automated backup, and limited monitoring. Cloudinary Free has a shared credit quota consumed by storage, transformations and bandwidth. Track provider usage and test exhaustion behavior safely with synthetic assets; never try quota exhaustion on a shared account. Current plan rules and dashboard billing configuration must be rechecked before activation. No service-level availability is promised by free tiers.

Keep $0/month: do not add a payment method, enable paid overages/backups, upgrade plans, buy a domain, or activate a paid add-on without owner approval. A free service can still stop serving once its included quota is exhausted.

## Recovery and incident response

1. Record affected service, deployed commit/build identifier, UTC/Africa-Addis timestamp, and sanitized status/error class.
2. Preserve logs and evidence under access control; redact tokens, customer contact data, private URLs, order/payment details.
3. For database recovery, restore a verified backup into an isolated target and test there first. Atlas Free generally requires manual dump/restore; current options/charges must be confirmed.
4. For media recovery, reconcile asset IDs and database references from a restricted manifest. Do not bulk delete or infer assets from folders.
5. Reconcile independent database/media recovery points before restoring writes. Keep the service read-only or unavailable if references cannot be safely reconciled.
6. Notify the owner and follow the legal/privacy incident process for any suspected receipt exposure.

## Routine operational checks

- Verify service commit and environment classification after every release.
- Confirm health, database connection, exact CORS allowlist, storage selection, and backups.
- Test restore periodically to an isolated destination, documenting recovery point and recovery time.
- Review admin accounts/roles and disable inactive accounts promptly.
- Keep payment-proof retention variables unset pending owner/legal approval. Never run cleanup without separately approved periods and a reviewed dry-run report.
