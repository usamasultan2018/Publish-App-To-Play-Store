# Publish App To Play Store 🚀

![In-App Purchase Flowchart](In-App Purchase Integration Flowchart.png)

![Version](https://img.shields.io/badge/version-1.0-blue)
![Platform](https://img.shields.io/badge/platform-Android-green)

---

## Table of Contents
1. [Package & App Preparation 🏷️](#1-package--app-preparation-🏷️)
2. [Firebase Integration 🔥](#2-firebase-integration-🔥)
3. [Google Cloud Setup ☁️](#3-google-cloud-setup-☁️)
4. [RevenueCat Setup 💰](#4-revenuecat-setup-💰)
5. [Google Play In-App Products 📦](#5-google-play-in-app-products-📦)
6. [App Implementation 🛠️](#6-app-implementation-🛠️)
7. [Testing ✅](#7-testing-✅)
8. [Resources 🔗](#resources-🔗)

---

## 1. Package & App Preparation 🏷️
- Update package name and generate new app key/signature.
- Create a new app in Google Play Console:
  - Add app name, icon, description, screenshots.
  - Add privacy policy URL, terms of service, etc.
  - Add up to 14 tester emails for internal testing.

> ⚠️ Tip: Make sure your package name is unique and matches your Firebase and RevenueCat setup.

---

## 2. Firebase Integration 🔥
- Create or select a Firebase project.
- Add your Android app and download `google-services.json`.
- Place `google-services.json` in the `app/` folder of your Android project.

> ⚠️ Tip: Double-check that the package name in Firebase matches your app's package name.

---

## 3. Google Cloud Setup ☁️
- Enable **Google Play Developer API**.
- Generate a service account key (JSON) for RevenueCat.
- Keep the JSON key safe; it will be used for purchase verification.

> ⚠️ Tip: Only generate one service account key per project to avoid conflicts.

---

## 4. RevenueCat Setup 💰
- Connect your Firebase/Google Play project to RevenueCat.
- Upload the JSON service account key.
- Create products, entitlements, and offerings in RevenueCat corresponding to your in-app purchases.

> ⚠️ Tip: Ensure Product IDs in RevenueCat exactly match the IDs in Google Play Console.

---

## 5. Google Play In-App Products 📦
- Go to **Monetize → Products → Subscriptions** in Google Play Console.
- Add subscription products and configure offers, pricing, and durations.
- Match Product IDs with RevenueCat.
- Publish subscription (can remain draft for testing).

> ⚠️ Tip: Use internal testing before rolling out to production.

---

## 6. App Implementation 🛠️
- Implement in-app purchase logic in your app.
- Fetch entitlements and purchases via RevenueCat SDK.
- Ensure subscription entitlements in RevenueCat match your app logic.

> ⚠️ Tip: Test both active and expired subscriptions to handle edge cases.

---

## 7. Testing ✅
- Test using the tester emails added in step 1.
- Verify that subscriptions are recognized correctly in the app.
- Test on different devices if possible.

> ⚠️ Tip: RevenueCat has a sandbox environment for safer testing.

---

## Resources 🔗
- [RevenueCat Docs](https://www.revenuecat.com/docs)
- [Google Play Console](https://play.google.com/console)
- [Firebase Documentation](https://firebase.google.com/docs)
