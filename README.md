# Publish App To Play Store

## 1. Package & App Preparation
- Update package name and generate new app key/signature.
- Create a new app in Google Play Console:
  - Add app name, icon, description, screenshots.
  - Add privacy policy URL, terms of service, etc.
  - Add up to 14 tester emails for internal testing.

## 2. Firebase Integration
- Create/select Firebase project.
- Add Android app and download `google-services.json`.

## 3. Google Cloud Setup
- Enable Google Play Developer API.
- Generate service account key (JSON) for RevenueCat.

## 4. RevenueCat Setup
- Connect Firebase/Google Play project.
- Upload JSON service account key.
- Create products, entitlements, and offerings.

## 5. Google Play In-App Products
- Add subscription products in Monetize → Products → Subscriptions.
- Match Product IDs with RevenueCat.
- Publish subscription (draft for testing is fine).

## 6. App Implementation
- Implement in-app purchase logic.
- Fetch entitlements and purchases via RevenueCat SDK.

## 7. Testing
- Test using the added tester emails.
- Verify subscriptions are recognized correctly in the app.
