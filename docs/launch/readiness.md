# YoMart launch readiness

**Updated:** 12 November 2026  
**Disposition:** staging is deployed; Android release and tester launch are not yet cleared. This is an evidence-based status, not a blanket NO-GO or an assumption that every component is production-ready.

## Verified deployment state

| Component | Current evidence | Meaning |
|---|---|---|
| API | Render deploy `dep-dbqed32vcj2c73eqphl0` is live at commit `0371cbd01fb7b40df05391c49cedd58f3ea1736f`. API health previously returned HTTP 200, environment `staging`, database connected. | Backend code is deployed to staging; not production. |
| Customer storefront | Render static service `yomart`, URL `https://yomart.onrender.com`, auto-deploy from `kalaabalb/yomart-web`. | Hosting is configured. Browser acceptance must still cover real user journeys. |
| Admin | Render static service `yomart-admin`, URL `https://yomart-admin.onrender.com`, auto-deploy from `kalaabalb/yomart-admin`. | Hosting is configured. Role-based workflows must still be accepted end-to-end. |
| Mobile | `kalaabalb/yomart-mobile`; checkout legacy-cart fix PR #11. Latest CI has passed formatting, analysis, and tests; Android debug APK build is running. | Not yet merged or a signed Play release. |
| Database | Atlas project `YoMart-Staging`, database `yomart_staging`. Catalog currently includes a staging Phones category, Smartphones subcategory, and two synthetic products; brands collection is empty. | Staging catalog data exists; not production catalog data. |
| Payments | Staging store settings contain enabled Cash on collection, no proof requirement, and zero delivery fee. | Manual cash collection/handover only; no gateway. |
| Cloudinary | Current account has uploaded product/category images. Private/authenticated proof inventory was empty in the latest review. | Does not establish successful end-to-end private proof upload/retrieval. Test with a synthetic proof before accepting real receipts. |

## Current work in progress

### Android checkout compatibility

PR #11 protects checkout from older persisted cart items with an empty variants array. It avoids reading `variants.first` and omits optional client price when no variant price exists; the API remains authoritative. The formatter and analyzer issue has been corrected. Do not merge until the current workflow completes successfully and the resulting artifact is verified.

## Release gates still required

### Android / Google Play

- [ ] Current PR #11 CI green, including Android build and artifact upload.
- [ ] Merge only the verified PR head; then run and download the build artifact.
- [ ] Confirm the release keystore is the intended Play upload key; compare its certificate fingerprint with Play Console's upload-key certificate. Do not expose the keystore or passwords.
- [ ] Build a signed release Android App Bundle (`.aab`) against the approved API origin and correct release version.
- [ ] Install and exercise the release candidate on physical Android devices; debug APK success is not enough.
- [ ] Complete Play Console app access, store listing, app content, data safety, target audience, content rating, ads declaration, countries, and testing track setup.
- [ ] Publish a closed test and recruit real people with Google accounts. For a personal developer account created after 13 November 2023, Google requires 12 opted-in testers continuously for 14 days before the production-access application. Recruit 14 to provide a buffer. Track actual opt-in dates and engagement; never fabricate testers or activity.
- [ ] Review and answer Play's production-access questionnaire honestly after the closed test.

### Catalog and staging data

- [ ] Replace synthetic placeholder listings with owner-approved product data, accurate ETB prices, real stock counts, correct categories/brands/variants, and appropriate product imagery.
- [ ] Keep the staging environment visually useful but unmistakably separate from production. Do not mark synthetic listings as search verified.
- [ ] Test the customer app, web storefront, and admin against the same staging catalog.
- [ ] Do not create fake customer accounts or fake order activity to simulate testers. Testers should register themselves; use clearly marked synthetic fixture data only for admin workflow coverage.

### End-to-end functional acceptance

- [ ] Customer: first launch, browse, category/brand/collection, product details, cart persistence, checkout, payment method, proof flow where required, order confirmation, order list/detail, tracking, cancellation, logout/login.
- [ ] Admin: sign-in/session expiry, roles, dashboard, catalog CRUD, image upload, stock/price changes, payment methods, order review, proof review, fulfilment/collection, customer support, settings, notifications, responsive navigation.
- [ ] API: authentication/authorization, owner isolation, CORS, rate limits, inventory reservation/release, idempotency, proof authorization, payment transitions, public catalog filtering.
- [ ] Web: direct route refresh/deep links, loading/empty/error/retry states, mobile/tablet/desktop layouts, keyboard access, image failures, network loss.
- [ ] Media: verify authenticated proof upload, authorized retrieval, unauthorized denial, expiry, and cleanup with exact synthetic asset IDs.
- [ ] Collect screenshots at phone and desktop breakpoints for every major journey and record device, viewport, app version/commit, and test result. Do not claim screenshot acceptance until the screenshots exist and have been reviewed.

### Legal, privacy, operations

- [ ] Publish owner-approved privacy policy and account deletion information at stable public URLs before Play submission.
- [ ] Confirm deletion behavior and wording: order/payment evidence may be retained for legal/operational reasons and anonymized rather than erased.
- [ ] Approve proof retention durations before setting cleanup variables or running destructive cleanup.
- [ ] Verify backup and restore procedure for Atlas and media; rehearse recovery.
- [ ] Confirm provider billing limits, alerts, and service plans.
- [ ] Confirm API origins/CORS allowlist and environment separation for the actual Play release. Current API environment is staging.

## Launch decision rule

A green CI build proves code compiles and automated checks pass. It does not prove payment proof privacy, real catalog quality, Play signing, legal compliance, tester engagement, device compatibility, or production readiness. Clear each gate with dated evidence before public production release.
