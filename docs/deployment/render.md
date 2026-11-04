# Render deployment reference

## API service

The published Node backend package scripts define `npm start` as `node index.js`; `npm test` runs `node --test`. For a Render Node web service, use repository root, install with `npm ci`, start with `npm start`, and configure health check path `/health`. The backend must bind to the host-provided `PORT` and listen on `0.0.0.0` (verify the deployed release configuration before changing it).

Required runtime values include database URI, JWT signing secret, bootstrap identity/password inputs where bootstrap is intentionally enabled, and the exact browser-origin CORS allowlist. Do not enable bootstrap on an existing installation just to get the service running. Keep backend credentials only in the service's secret environment configuration.

The API uses a MongoDB replica set for transactions. Render Free's filesystem is ephemeral and a free web service can sleep after inactivity; neither is suitable evidence storage. Keep proof and catalog media on durable provider storage, not local paths. Story upload's process-local concurrency guard requires one backend process while that feature is enabled.

## React admin static site

- Repository: `kalaabalb/yomart-admin`, branch `main`, root directory `.`
- Build command: `npm ci && npm run build`
- Publish directory: `dist`
- SPA rewrite: `/*` → `/index.html`
- Build variables: `YOMART_BUILD_ENV=staging`; `VITE_API_BASE_URL` and `YOMART_EXPECTED_API_ORIGIN` must be the same confirmed staging API origin; `VITE_EXPECTED_SITE_ORIGIN` must be the exact assigned admin origin. Staging denylist inputs follow the published build validator; see [environment variables](environment-variables.md).

## React customer static site

- Repository: `kalaabalb/yomart-web`, branch `main`, root directory `.`
- Published source commit verified: `f7f2350cbdb50beb4b4ad774a91b82eb4f5f666e`
- Build command: `npm ci && npm run build`
- Publish directory: `dist`
- SPA rewrite: `/*` → `/index.html` (supports React Router deep links/refresh)
- Staging variables: `YOMART_BUILD_ENV=staging`; `VITE_API_BASE_URL` equals `YOMART_EXPECTED_API_ORIGIN`; `VITE_EXPECTED_SITE_ORIGIN` equals the exact Render-assigned storefront origin. `YOMART_NO_PRODUCTION_API=true` may be used only as an explicit owner assertion where no production API exists; otherwise provide the confirmed, strict production API denylist. The production site denylist may be blank/unset only while no production website origins are confirmed. Exact details are in [the storefront README](https://github.com/kalaabalb/yomart-web/blob/main/README.md).

The requested customer origin `https://yomart.onrender.com` is a build target proposal only. Render must confirm it is assigned before using it. After assignment, add the exact storefront and admin HTTPS origins to backend `CORS_ALLOWED_ORIGINS`, rebuild static artifacts with exact site values, and verify direct route refresh. Do not use wildcard CORS.

## Free-tier constraints

Render states that Free web services sleep after 15 minutes idle, use ephemeral filesystems, and can take about a minute to wake. Free instance hours, outbound bandwidth and build pipeline usage are limited; reaching limits may suspend services/builds or cause charges depending on account/payment settings. Free static sites also count toward bandwidth and build usage. Review current workspace usage, spend limits and payment methods before activating services. A $0 plan is an availability constraint, not a production service-level guarantee. See [Render Free documentation](https://render.com/docs/free).

No Render setting or service was changed by this documentation project.
