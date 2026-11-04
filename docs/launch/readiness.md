# Launch readiness snapshot

**Updated:** 5 November 2026. **Disposition:** repository publication is not launch approval. This document records repository and prior local-test evidence plus owner-reported deployment state. It does not claim a fresh hosted acceptance run.

## Current publication/deployment status

| Item | Status/evidence |
|---|---|
| API repository | Published at `kalaabalb/yomart-api`, `main` commit `0a55c155708d7a3e1218116909e751b92ef976c2`; owner reports service deployed and working at `https://yomart-api.onrender.com`. Current runtime DB/environment not independently checked in this docs task. |
| Admin repository | Published at `kalaabalb/yomart-admin`, `main` commit `3f724dc42fa718a510b4dba9c41df8b2e657c9d1`; owner reports admin deployed at `https://yomart-admin.onrender.com`. Do not infer which database/environment it targets. |
| React customer source | Published at `kalaabalb/yomart-web`, `main` commit `f7f2350cbdb50beb4b4ad774a91b82eb4f5f666e`. Local source build/test passed; no Render deployment verified. Requested `https://yomart.onrender.com` is not confirmed assigned. |
| Flutter Android | Public source repository exists. Signing, final production API build, Play listing, account-specific testing/production access, and Play approval remain separate release tasks. |
| Docs | This documentation repository is being published separately. |

## Automated local evidence

Recorded latest checks available to this publication work (not all rerun in the docs copy):

- React storefront publication copy: `npm test` 47 passed, 0 failed; staging build/noindex artifact verification passed in a temporary output directory; `npm audit` reported zero vulnerabilities; Gitleaks found no leaks in scanned source, generated artifact and staged tree; `git diff --cached --check` passed.
- Backend publication copy: `npm test` 219 passed, 2 skipped, 0 failed (221 total); production and full dependency audits reported zero known vulnerabilities. Skips are optional database-replica integration gates; this count is from the latest local run and may change with source.
- Admin local publication source: `npm test` 16 passed, 0 failed; production and full audits reported zero known vulnerabilities. Admin remote commit was verified; a new admin deployment was not triggered.
- Flutter client: `flutter analyze --no-pub` had no issues and `flutter test --no-pub` reported 30 passing tests in the 4 November local work. No current Android production AAB/Play-installed acceptance is claimed here.

These results are dated local evidence, not a promise that the tests still pass at another commit or that hosted services are healthy. Re-run the relevant CI checks against the exact candidate release.

## Critical and high blockers

### Critical: legacy public payment-proof assets

Older Cloudinary receipt URLs may remain anonymously accessible. The backend no longer exposes/fetches legacy public URLs through customer order serialization/retrieval, but that does not revoke provider/CDN copies. A preservation-first inventory and approved restriction/copy migration, synthetic provider verification (original and derived assets), authorization checks, cache analysis, rollback and evidence reconciliation are required before collecting real receipts or public launch.

### High: privacy/legal and retention

Account deletion anonymizes and retains order/payment evidence; public wording must be reviewed and must not promise full order deletion. Orphan/superseded proof retention durations are not approved and remain unset. Do not run destructive cleanup.

### High: hosted staging and billing

- Verify exact API/admin host, deploy commit, runtime environment and DB target from provider dashboards/health logs.
- Confirm customer static-site hostname assignment; requested `yomart.onrender.com` is not confirmed.
- Set exact CORS origins; no wildcard. Resolve any browser preflight/upstream 502 using API and Render evidence.
- Run full role/ownership, synthetic order, proof, stories, media expiry, deep-link, mobile/browser and SEO acceptance on staging.
- Confirm $0 billing controls/quotas for Render, Atlas and Cloudinary. Free tiers can suspend, sleep, lose local files, or incur optional/overage charges depending on plan/settings. No cost configuration was checked here.
- Establish and rehearse database/media backup and restore. Atlas Free has no standard backup feature; manually managed dumps need access controls and a tested restore path.

## Other known gaps

- No production customer/admin domains or complete production API/site denylist have been independently confirmed. Do not copy old or test domains into production settings.
- Variant-specific price/stock and ownership validation are missing; configured variants are blocked from customer purchase.
- React storefront is an SPA. Local staging noindex is checked, but SEO crawlable product/category HTML/sitemap from live verified data has not been completed for this React site. Search indexing/ranking is never guaranteed.
- Storefront includes no payment gateway, password recovery, email verification, profile-edit or full account-deletion UI. Do not claim otherwise.
- Story upload is restricted until `STORY_MAX_DURATION_SECONDS` is configured; 15 seconds was approved for staging tests only. Public story publication is not approved. One API process is required while the process-local story guard is in use.
- Status numbers and audits must be rerun at release commit. Tests cannot establish Cloudinary, Atlas, Render, legal or billing behavior.

## Release sequence

1. Verify runtime service commit/environment/health and exact database isolation.
2. Resolve the interrupted synthetic Cloudinary POC asset from existing local recovery evidence before any new provider asset is created; never list assets to guess.
3. Execute the signed media and role authorization tests on one synthetic staging asset; document provider evidence and clean up only by exact ID.
4. Complete direct API role tests, transactions/idempotency, destination collection, payment methods, customer flows, admin workflows, and desktop/mobile route refreshes on synthetic data.
5. Verify exact origin/CORS configuration, staging noindex, robots/sitemap, backup/restore, logs/alerts and $0 usage controls.
6. Obtain owner/legal decisions on retention, account-deletion language, legacy receipt migration and public stories.
7. Only then prepare a separate production design/release review. No production deployment is authorized by this checklist.
