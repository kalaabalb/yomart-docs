# Owner inputs required to finish YoMart closed testing and sales launch

This checklist contains business/account facts that must come from the owner. They must not be invented or inferred from the database.

## Closed testing can stay preview-only

The current mobile release workflow defaults to preview_catalog=true. In that mode testers can install, register/sign in, browse the live catalogue and review the interface. Prices and stock are hidden and ordering is deliberately unavailable. This is the safe path while catalogue data is not approved.

A real transactional test or public sales launch additionally requires owner-approved product prices, stock, exact variants/condition and warranty. Complete [catalog-owner-approval.csv](catalog-owner-approval.csv) before enabling sales. Do not use the illustrative values as offers.

## Legal publisher and support details

Confirmed owner information:

- **Yonas Ambelu** is the legal owner and retailer responsible for YoMart. Use this exact name in the legal publisher/retailer identity fields, subject to the owner's final review of the published page.

Still required before publication:

- Customer support email that is monitored.
- Public business contact details/address and the country/jurisdiction the business operates under.
- Owner-approved retention period and legal basis for keeping pseudonymized order/payment evidence after account deletion.
- Confirmation that the described deletion behavior is acceptable: sign-in is disabled; direct profile/contact/delivery identifiers and reviews are removed; some order/payment records may remain pseudonymized; the customer loses access to order history and receipts.

Do not infer or invent any of these remaining details.

Google Play requires a public, globally accessible privacy policy linked both in the Play listing and inside the app. It also requires an accessible account-deletion path for apps with account creation, and associated user data must be deleted except data retained for legitimate reasons that are clearly disclosed. Review the official [Google Play User Data policy](https://support.google.com/googleplay/android-developer/answer/10144311). This is why the current pseudonymized order/payment retention needs your explicit legal/business approval before the pages are final.

Do not publish the pages as final until these details and statements are approved.

## Android signing and Play Console

Never paste keystore passwords, the keystore file, access tokens or Play Console credentials into chat or public documentation.

In the kalaabalb/yomart-mobile GitHub repository, add these under **Settings → Secrets and variables → Actions → Secrets**. Generate the keystore secret locally from the existing ignored keystore file with the command base64 -w 0 /path/to/the-existing-release-keystore.jks, then paste that output directly into the GitHub secret field. Do not paste it into chat, terminal screenshots, or a committed file:

- ANDROID_KEYSTORE_BASE64
- ANDROID_STORE_PASSWORD
- ANDROID_KEY_PASSWORD
- ANDROID_KEY_ALIAS

Add this repository variable:

- YOMART_PRIVACY_POLICY_URL=https://yomart.onrender.com/privacy-policy.html

The existing local upload keystore must match the upload certificate registered in the friend's Play Console. Compare its SHA-256 fingerprint with **Play Console → Setup → App integrity → Upload key certificate**. Do not upload an AAB signed with a different key to an already-registered package.

Confirm whether the friend's developer account is **Personal** or **Organization**, and when it was created. New personal accounts created after 13 November 2023 need at least 12 testers opted in continuously for 14 days before applying for production access. Fourteen available testers provide a buffer, but the actual opt-in timestamps must be tracked. See Google's official [testing requirements](https://support.google.com/googleplay/android-developer/answer/14151465).

The Android package ID is currently com.yomobiles.client. Confirm that this is the package ID intended for the friend's Play Console app, whether any version has already been uploaded, and the highest version code already used (the current source version is 1.0.0+5).

## Owner-assisted acceptance evidence

The hosted web screenshot pack captures the storefront and admin sign-in page. The authenticated admin dashboard and protected pages cannot be captured without an authorized session.

The split-ABI preview APKs are available from [mobile CI run 43234715460](https://github.com/kalaabalb/yomart-mobile/actions/runs/43234715460), artifact 13951718520. Choose app-arm64-v8a-debug.apk for most modern Android phones, app-armeabi-v7a-debug.apk for older 32-bit ARM phones, and app-x86_64-debug.apk for emulator use. These are debug-signed sideload builds, not a signed Play AAB. They connect to the production API and keep prices/stock hidden and ordering disabled. The GitHub-hosted Android emulator screenshot job failed because the emulator never reported sys.boot_completed; no screenshot artifact was produced. Capture app screenshots on a physical Android device instead. Do not use the older debug artifact from run 43229794520; that build used the local 127.0.0.1 API default and is not suitable for external-device testing.

Sign in yourself at [yomart-admin.onrender.com](https://yomart-admin.onrender.com) and provide sanitized screenshots of the dashboard, product list/detail, order list, payment settings and other important admin pages. Do not send passwords or customer personal data. Also install the arm64 preview APK on a real Android device and report any launch, login, navigation or layout failures.

## Safe handling

- The existing Atlas Free database is the active production database, despite its historical name yomart_staging. Do not use it for dummy orders, stock mutations, fake receipts or destructive tests.
- Production checkout stays disabled until the owner approves real prices and stock.
- No paid Atlas, Render or Cloudinary resource is required for this preview/testing path.
