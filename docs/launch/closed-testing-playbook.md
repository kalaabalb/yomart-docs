# YoMart Google Play closed-testing playbook

**Planning target:** 14 real testers, 15 calendar days of active closed testing.  
**Purpose:** validate YoMart with real users and collect honest evidence before requesting production access.

## Google Play minimum

For personal developer accounts created after 13 November 2023, Google requires a closed test with at least 12 testers opted in continuously for at least 14 days before applying for production access. A tester who opts out and opts back in restarts their consecutive period. Recruiting 14 gives a buffer, but all qualifying testers must remain opted in. Confirm the account type and current Play Console eligibility because the rule is account-specific.

Official references:
- [Testing requirements for new personal developer accounts](https://support.google.com/googleplay/android-developer/answer/14151465)
- [Set up an open, closed, or internal test](https://support.google.com/googleplay/android-developer/answer/9845334)

## Before inviting testers

- [ ] Upload the signed release AAB to the closed track (not a debug APK).
- [ ] Verify package ID, version, signing certificate, release notes, and countries.
- [ ] Complete Play Console app-content declarations and store listing required for the track.
- [ ] Provide a working feedback email or URL.
- [ ] Ensure privacy policy and account-deletion URL are publicly reachable and accurate.
- [ ] Ensure the app uses the intended staging/test backend for the test period, not a production customer database.
- [ ] Ensure staging catalog contains accurate, owner-approved test listings and the payment method is clearly described as cash on collection.
- [ ] Ensure testers know not to submit real bank details, real receipts, passwords, or sensitive personal information.
- [ ] Prepare a bug-report form/channel and a device/OS/version log.
- [ ] Recruit 14 people who represent likely customers and use a range of Android devices and network conditions.

## Tester onboarding

1. Add all 14 Google accounts to the closed-test tester list or an eligible Google Group.
2. Publish the closed-test release and share the opt-in URL.
3. Each tester must open the opt-in URL with the Google account they use on their Android device, opt in, and install through Google Play.
4. Record the actual date/time each tester successfully opted in. The 14-day clock is individual; do not infer it from invitation date.
5. Ask testers to remain opted in continuously through the full 15-day window, even if they uninstall temporarily or pause use.
6. Confirm Play Console still shows at least 12 qualifying opted-in testers before applying for production access.

## 15-day test plan

| Day | Test focus | Evidence to collect |
|---|---|---|
| 1 | Opt-in, install, first launch, onboarding | Opt-in timestamp, device/Android version, install result, first-launch screenshot |
| 2 | Browse home, categories, brands, search | Screenshots, broken image/report, search relevance |
| 3 | Product detail, images, price, stock, variants | Product screenshots, incorrect/missing data |
| 4 | Cart add/remove/update and persistence | Cart screenshots, totals and persistence result |
| 5 | Checkout details and validation | Checkout screenshots, field/validation issues |
| 6 | Cash on collection explanation and order placement | Confirmation screenshot, no unintended payment |
| 7 | Order list, detail, status/tracking | Screenshots, status consistency |
| 8 | Cancellation and unavailable/out-of-stock behavior | Expected vs actual result, any stock inconsistency |
| 9 | Login/logout/session expiry and account flows | Screenshots, auth failures, privacy concerns |
| 10 | Slow/unstable network, retry, app resume | Screen recording or screenshots of error/recovery states |
| 11 | Small-screen and large-screen layout/accessibility | Device screenshots, clipping/tap-target issues |
| 12 | Repeat core purchase journey on a second device/network | Device comparison, reproducibility |
| 13 | Customer storefront comparison with mobile app | Desktop/mobile web screenshots, consistency issues |
| 14 | Regression of reported issues and retest fixes | Before/after evidence, unresolved bugs |
| 15 | Final full journey, feedback survey, opt-in verification | Final feedback summary, qualifying tester count |

Testers should use their own accounts. Do not create fictitious people, opt-ins, reviews, orders, or engagement to satisfy Play requirements.

## Daily feedback template

For each issue, capture:
- Tester ID (use a non-identifying code such as T01; do not store passwords).
- Date/time and app version.
- Device model and Android version.
- Screen/flow and exact steps to reproduce.
- Expected result and actual result.
- Severity: blocker / major / minor / visual.
- Screenshot or short recording with personal data removed.
- Whether it reproduces after restart or on another device.
- Resolution commit/version and retest outcome.

## Tester roster tracker

Keep this file private; never commit tester email addresses or personal details to the public documentation repository.

| Tester code | Invited | Opted in at (UTC) | Device / Android | Day 1 used | Day 7 used | Day 14 used | Day 15 used | Still opted in? | Notes |
|---|---|---|---|---|---|---|---|---|---|
| T01 | | | | | | | | | |
| T02 | | | | | | | | | |
| T03 | | | | | | | | | |
| T04 | | | | | | | | | |
| T05 | | | | | | | | | |
| T06 | | | | | | | | | |
| T07 | | | | | | | | | |
| T08 | | | | | | | | | |
| T09 | | | | | | | | | |
| T10 | | | | | | | | | |
| T11 | | | | | | | | | |
| T12 | | | | | | | | | |
| T13 | | | | | | | | | |
| T14 | | | | | | | | | |

## Screenshot evidence checklist

Capture real application screenshots; do not fabricate mockups as test evidence.

- [ ] Android: splash, home, category, brand, product detail, cart, checkout, payment method, order confirmation, order list, order detail/tracking, account.
- [ ] Admin: login, dashboard, products list/detail/edit, category/brand, orders list/detail, payment review, settings, responsive navigation.
- [ ] Storefront: home, product list/detail, cart, checkout, order state, mobile and desktop layouts.
- [ ] Each image has filename with date, surface, screen, viewport/device, and app commit/version.
- [ ] Blur customer names, phone numbers, addresses, emails, tokens, receipts, and order identifiers before sharing.
- [ ] Keep original evidence in a private project folder; only publish sanitized screenshots approved by the owner.

## Production-access application

After the test window:
1. Verify at least 12 testers have remained opted in continuously for the preceding 14 days.
2. Review actual usage and feedback; resolve blockers and critical defects.
3. Summarize which features testers used, how usage compared with expected customer behavior, feedback themes, and fixes shipped.
4. Answer Play Console's production-access questions honestly and submit.
5. Continue testing while Google reviews; the review may take a week or longer and approval is not guaranteed.
