# Environment variable reference

Keep secrets in backend secret configuration or restrictive local ignored env files. Values with `VITE_` are compiled into public browser assets; never put credentials there. `MAIN_URL` and Flutter `--dart-define` values are also public API/site configuration, not secrets.

## Backend

Names verified from the published backend `.env.example` and source:

| Variable | Purpose | Handling |
|---|---|---|
| `NODE_ENV` | Runtime mode; `production` activates production checks. | Public mode value. |
| `PORT` | HTTP listen port, host-injected on Render. | Public runtime setting. |
| `MONGO_URL` | MongoDB connection URI/database target. | **Secret.** Use a separate least-privilege staging DB user and synthetic database. |
| `JWT_SECRET` | Signs customer/admin JWTs. Production code requires at least 32 random bytes. | **Secret.** Unique per environment. |
| `JWT_EXPIRES_IN`, `ADMIN_JWT_EXPIRES_IN` | Customer/admin token expiry. | Public policy settings. |
| `ADMIN_USERNAME`, `ADMIN_NAME`, `ADMIN_EMAIL`, `ADMIN_PASSWORD` | Optional controlled first-admin bootstrap inputs. Bootstrap is a database write; do not enable on an existing environment or assume startup should create an account. | Identity is private; password is **secret**. |
| `CLOUDINARY_CLOUD_NAME` | Selects Cloudinary cloud. | Public identifier, but keep deployment mapping separate by environment. |
| `CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET` | Cloudinary SDK credentials. | **Secrets.** |
| `UPLOADS_DIR` | Local public/catalog upload root if configured. | Path; never rely on Render Free persistence. |
| `PAYMENT_PROOFS_DIR` | Local private proof root if configured instead of authenticated Cloudinary. | Sensitive path; must be on durable private storage. |
| `CORS_ALLOWED_ORIGINS` | Comma-separated exact browser origins for staging/production; no paths or wildcard. | Public origins; configure exact deployed sites only. |
| `STORY_MAX_DURATION_SECONDS` | Video Stories upload duration limit. Story upload fails closed when missing/invalid. `15` was approved for staging tests only, not public release. | Policy value; do not set for production without approval. |
| `PAYMENT_PROOF_ORPHAN_RETENTION_DAYS`, `PAYMENT_PROOF_SUPERSEDED_RETENTION_DAYS` | Separate evidence-cleanup retention periods. | Keep unset until owner/legal approval; never run cleanup to make a test pass. |
| `ONE_SIGNAL_APP_ID`, `ONE_SIGNAL_REST_API_KEY` | Optional push integration. | App ID public-ish; REST key **secret**. |

The repository also has tool/test-specific settings such as `MONGODB_REPLICA_TEST_URL`, `CATALOG_SEED_CONFIRM`, and Cloudinary POC acknowledgement/config inputs. Do not add them to a hosted service unless following the specific isolated tool procedure. Never run catalog import, cleanup, reconciliation, or POC tooling against production.

## Admin and React storefront build

These values are not secrets and are embedded/validated at build time:

| Variable | Purpose |
|---|---|
| `YOMART_BUILD_ENV` | Explicit `local`, `staging`, or `production` target. |
| `VITE_API_BASE_URL` | Browser API base. Must match `YOMART_EXPECTED_API_ORIGIN`. |
| `YOMART_EXPECTED_API_ORIGIN` | Required validated API origin. |
| `VITE_EXPECTED_SITE_ORIGIN` | Exact site origin expected at runtime. |
| `YOMART_PRODUCTION_API_ORIGINS` | Staging denylist; comma-separated confirmed production API origins; may be blank only under explicit staging policy. |
| `YOMART_NO_PRODUCTION_API` | `true` explicitly asserts that no production API exists; incompatible with a nonempty production API list. |
| `YOMART_PRODUCTION_SITE_ORIGINS` | Staging denylist of confirmed production site origins; blank/unset currently allowed by staging build. |
| `YOMART_STAGING_API_ORIGINS`, `YOMART_STAGING_SITE_ORIGINS` | Production-build denylists; both must be verified and nonempty for production builds. |
| `VITE_ALLOW_LOCAL_API` | Development-only local HTTP allowance; never set for a release build. |

Every supplied origin is validated as a bare HTTPS origin (no credentials, path, query, fragment, wildcard, placeholder, loopback/IP). Empty staging denylists do not detect unknown origins; populate them when owner-confirmed origins are known. API and site-origin policies are separate. Production builds remain strict. See the admin and storefront build validators in their public repositories.

## Flutter web build

`tooling/build_web.mjs` consumes public build-time values `YOMART_BUILD_ENV`, `MAIN_URL`, `YOMART_EXPECTED_API_ORIGIN`, and `PRIVACY_POLICY_URL`, then passes Dart defines and generates the deletion-page config. The API origin must match; release origins require HTTPS and cannot be placeholders/loopback. Product catalog SEO generation is a separate command and is not part of the React storefront build.
