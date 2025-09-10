# Publish App To Play Store 🚀

![In-App Purchase Flowchart](ChatGPT%20Image%20Sep%209%2C%202025%2C%2011_14_42%20AM.png)

![Version](https://img.shields.io/badge/version-1.0-blue)
![Platform](https://img.shields.io/badge/platform-Android-green)
![License](https://img.shields.io/badge/license-MIT-yellow)
![Last Updated](https://img.shields.io/badge/last%20updated-September%202025-brightgreen)

Complete step-by-step guide for publishing Android apps to Google Play Store with in-app purchases using RevenueCat integration.

## 📋 Table of Contents

1. [Package & App Preparation 🏷️](#1-package--app-preparation-🏷️)
2. [Firebase Integration 🔥](#2-firebase-integration-🔥)
3. [Google Cloud Setup ☁️](#3-google-cloud-setup-☁️)
4. [RevenueCat Setup 💰](#4-revenuecat-setup-💰)
5. [Google Play In-App Products 📦](#5-google-play-in-app-products-📦)
6. [App Implementation 🛠️](#6-app-implementation-🛠️)
7. [Build & Sign App 📱](#7-build--sign-app-📱)
8. [Upload to Play Store 🏪](#8-upload-to-play-store-🏪)
9. [Testing ✅](#9-testing-✅)
10. [Additional Commands 💻](#10-additional-commands-💻)
11. [Troubleshooting 🔧](#11-troubleshooting-🔧)
12. [Resources 🔗](#12-resources-🔗)

---

## 1. Package & App Preparation 🏷️

### 1.1 Update Package Name

Edit `app/build.gradle`:
```gradle
android {
    defaultConfig {
        applicationId "com.yourcompany.yourappname"
    }
}
```

### 1.2 Generate New Keystore

```bash
# Generate new keystore
keytool -genkey -v -keystore my-release-key.keystore \
    -alias my-key-alias \
    -keyalg RSA \
    -keysize 2048 \
    -validity 10000

# Check existing keystore
keytool -list -v -keystore my-release-key.keystore
```

### 1.3 Configure Signing

Add to `app/build.gradle`:
```gradle
android {
    signingConfigs {
        release {
            storeFile file('my-release-key.keystore')
            storePassword 'your-store-password'
            keyAlias 'my-key-alias'
            keyPassword 'your-key-password'
        }
    }
    buildTypes {
        release {
            signingConfig signingConfigs.release
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
}
```

### 1.4 Google Play Console Setup

**URL:** [Google Play Console](https://play.google.com/console)

#### Create New App:
1. Click **"Create app"**
2. **App name:** Your App Name
3. **Default language:** English (US)
4. **App or game:** Select appropriate option
5. **Free or paid:** Select option

#### App Details:
- **App name:** Your App Name (30 characters max)
- **Short description:** Brief app description (80 characters)
- **Full description:** Detailed description (4000 characters max)

#### Graphics Requirements:
- **App icon:** 512 x 512 px (PNG, no transparency)
- **Feature graphic:** 1024 x 500 px
- **Screenshots:** At least 2 screenshots per device type
  - Phone: 320-3840px (16:9 or 9:16 ratio)
  - Tablet: 1200-3840px

#### Internal Testing:
```
Go to: Testing → Internal testing → Testers
Add up to 14 email addresses for testing
```

> ⚠️ **Tip:** Make sure your package name is unique and matches Firebase and RevenueCat setup.

---

## 2. Firebase Integration 🔥

### 2.1 Firebase CLI Setup

```bash
# Install Firebase CLI
npm install -g firebase-tools

# Login to Firebase
firebase login
```

### 2.2 Create Firebase Project

**URL:** [Firebase Console](https://console.firebase.google.com)

1. Click **"Add app"** → **Android**
2. Enter package name: `com.yourcompany.yourappname`
3. Download `google-services.json`
4. Place file in `app/` directory

### 2.3 Configure Firebase

**Project-level `build.gradle`:**
```gradle
buildscript {
    dependencies {
        classpath 'com.google.gms:google-services:4.3.15'
    }
}
```

**App-level `build.gradle`:**
```gradle
plugins {
    id 'com.google.gms.google-services'
}

dependencies {
    implementation platform('com.google.firebase:firebase-bom:32.2.2')
    implementation 'com.google.firebase:firebase-analytics'
}
```

### 2.4 Verify Integration

```bash
# Build project to verify integration
./gradlew build
```

> ⚠️ **Tip:** Double-check that the package name in Firebase matches your app's package name exactly.

---

## 3. Google Cloud Setup ☁️

### 3.1 Enable Google Play Developer API

**URL:** [Google Cloud Console](https://console.cloud.google.com)

```bash
# Using gcloud CLI (optional)
gcloud services enable androidpublisher.googleapis.com
```

**Manual Steps:**
1. Select your Firebase project
2. Go to **"APIs & Services"** → **"Library"**
3. Search **"Google Play Developer API"**
4. Click **"Enable"**

### 3.2 Create Service Account

1. **Navigate to:** "IAM & Admin" → "Service Accounts"
2. **Create Service Account:**
   - **Service account name:** PlayStore-RevenueCat
   - **Service account ID:** playstore-revenuecat
   - **Description:** Service account for RevenueCat integration
3. **Assign Role:** "Project" → "Editor"

### 3.3 Generate Service Account Key

1. Click on created service account
2. Go to **"Keys"** tab
3. Click **"Add Key"** → **"Create new key"**
4. Select **"JSON"** format
5. Download and **securely store** the JSON file

### 3.4 Link to Google Play Console

1. **Go to:** Google Play Console → "Users and permissions"
2. **Invite new users** → Enter service account email
3. **Permissions:** Select "Admin (all permissions)"

> ⚠️ **Tip:** Only generate one service account key per project to avoid conflicts.

---

## 4. RevenueCat Setup 💰

### 4.1 Create RevenueCat Account

**URL:** [RevenueCat Dashboard](https://app.revenuecat.com)

### 4.2 Create New Project

```
Project name: Your App Name
Platform: Android
```

### 4.3 Google Play Integration

1. **Go to:** "Project Settings" → "Integrations"
2. **Google Play:**
   - Upload the JSON service account key from Step 3.3
   - Package name: `com.yourcompany.yourappname`

### 4.4 Create Products

**Navigate to:** "Products" section

**Example Product Configuration:**
```
Product ID: premium_monthly
Type: Subscription
Description: Premium Monthly Subscription
```

### 4.5 Create Entitlements

**Navigate to:** "Entitlements" section

```
Entitlement ID: premium
Description: Premium features access
Associated Products: premium_monthly, premium_yearly
```

### 4.6 Create Offerings

**Navigate to:** "Offerings" section

```
Offering ID: default
Description: Default subscription offering
Packages:
  - monthly (premium_monthly)
  - yearly (premium_yearly)
```

### 4.7 Get API Keys

**Navigate to:** "Project Settings" → "API Keys"

```bash
# Add to your app's configuration
REVENUECAT_PUBLIC_KEY=your_public_api_key
```

> ⚠️ **Tip:** Ensure Product IDs in RevenueCat exactly match the IDs in Google Play Console.

---

## 5. Google Play In-App Products 📦

### 5.1 Navigate to Monetization

**Go to:** Google Play Console → "Monetize" → "Products" → "Subscriptions"

### 5.2 Create Subscription Product

1. **Click "Create subscription"**
2. **Product Configuration:**
   ```
   Product ID: premium_monthly (must match RevenueCat)
   Name: Premium Monthly
   Description: Access to all premium features
   ```

3. **Base Plans & Offers:**
   ```
   Base plan ID: monthly
   Billing period: 1 month
   Price: $9.99 USD
   ```

4. **Subscription Benefits:**
   ```
   Add benefits that users will receive
   Example: "Ad-free experience", "Premium content access"
   ```

### 5.3 Create Multiple Products

Repeat for different subscription tiers:
- `premium_yearly` - $99.99/year
- `premium_weekly` - $2.99/week

### 5.4 Activate Subscriptions

1. Review all details
2. Click **"Activate"** (can remain in draft for testing)

> ⚠️ **Tip:** Use internal testing before rolling out to production.

---

## 6. App Implementation 🛠️

### 6.1 Add RevenueCat SDK

**App-level `build.gradle`:**
```gradle
dependencies {
    implementation 'com.revenuecat.purchases:purchases:6.9.0'
    implementation 'com.revenuecat.purchases:purchases-ui:6.9.0' // Optional UI components
}
```

### 6.2 Initialize RevenueCat

**Create `Application.kt`:**
```kotlin
class MyApplication : Application() {
    override fun onCreate() {
        super.onCreate()
        
        Purchases.debugLogsEnabled = BuildConfig.DEBUG
        Purchases.configure(
            PurchasesConfiguration.Builder(this, "your_revenuecat_public_key")
                .appUserID(null) // Optional: pass user ID
                .build()
        )
    }
}
```

**Add to `AndroidManifest.xml`:**
```xml
<application
    android:name=".MyApplication"
    ...>
```

### 6.3 Implement Purchase Logic

**Create `PurchaseManager.kt`:**
```kotlin
class PurchaseManager {
    
    fun checkSubscriptionStatus(callback: (Boolean) -> Unit) {
        Purchases.sharedInstance.getCustomerInfo { customerInfo, error ->
            if (error != null) {
                callback(false)
                return@getCustomerInfo
            }
            
            val isSubscribed = customerInfo?.entitlements?.get("premium")?.isActive == true
            callback(isSubscribed)
        }
    }
    
    fun purchaseSubscription(activity: Activity, packageToPurchase: Package) {
        Purchases.sharedInstance.purchaseWith(
            PurchaseParams.Builder(activity, packageToPurchase).build()
        ) { storeTransaction, customerInfo ->
            if (customerInfo?.entitlements?.get("premium")?.isActive == true) {
                // User is now subscribed
                showPremiumFeatures()
            }
        }
    }
    
    fun getOfferings(callback: (Offerings?) -> Unit) {
        Purchases.sharedInstance.getOfferings { offerings, error ->
            callback(offerings)
        }
    }
}
```

### 6.4 Create Subscription UI

**`SubscriptionActivity.kt`:**
```kotlin
class SubscriptionActivity : AppCompatActivity() {
    private val purchaseManager = PurchaseManager()
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_subscription)
        
        loadSubscriptionOptions()
    }
    
    private fun loadSubscriptionOptions() {
        purchaseManager.getOfferings { offerings ->
            offerings?.current?.availablePackages?.forEach { package ->
                // Display subscription options
                createSubscriptionButton(package)
            }
        }
    }
    
    private fun createSubscriptionButton(package: Package) {
        // Create UI button for subscription
        val button = Button(this)
        button.text = "${package.storeProduct.title} - ${package.storeProduct.price}"
        button.setOnClickListener {
            purchaseManager.purchaseSubscription(this, package)
        }
    }
}
```

### 6.5 Handle Subscription Status

**`MainActivity.kt`:**
```kotlin
class MainActivity : AppCompatActivity() {
    private val purchaseManager = PurchaseManager()
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        
        checkSubscriptionStatus()
    }
    
    private fun checkSubscriptionStatus() {
        purchaseManager.checkSubscriptionStatus { isSubscribed ->
            if (isSubscribed) {
                showPremiumUI()
            } else {
                showFreeUI()
            }
        }
    }
}
```

> ⚠️ **Tip:** Test both active and expired subscriptions to handle edge cases.

---

## 7. Build & Sign App 📱

### 7.1 Clean and Build

```bash
# Clean previous builds
./gradlew clean

# Build release APK
./gradlew assembleRelease

# Build App Bundle (recommended for Play Store)
./gradlew bundleRelease
```

### 7.2 Verify Signature

```bash
# Verify APK is signed
jarsigner -verify -verbose -certs app/build/outputs/apk/release/app-release.apk

# Check App Bundle signature
jarsigner -verify -verbose -certs app/build/outputs/bundle/release/app-release.aab
```

### 7.3 Test Locally

```bash
# Install APK on device for testing
adb install app/build/outputs/apk/release/app-release.apk
```

---

## 8. Upload to Play Store 🏪

### 8.1 Upload App Bundle

**Navigate to:** Google Play Console → "Release" → "Internal testing"

1. **Create new release**
2. **Upload App Bundle:** `app-release.aab`
3. **Release name:** `1.0 (1)`
4. **Release notes:**
   ```
   Initial release with:
   - Core app functionality
   - Premium subscription features
   - Bug fixes and improvements
   ```

### 8.2 Complete Store Listing

**Navigate to:** "Store presence" → "Main store listing"

**Required fields:**
- App name: Your App Name
- Short description: One-line app description (80 chars)
- Full description: Detailed description (up to 4000 chars)
- App icon: 512x512 PNG
- Feature graphic: 1024x500 PNG
- Screenshots: Minimum 2 per device type

### 8.3 Content Rating

**Navigate to:** "Policy" → "Content rating"

1. Complete questionnaire about app content
2. Generate content rating
3. Apply rating to your app

### 8.4 Target Audience

**Navigate to:** "Policy" → "Target audience"

```
Primary target age: Select appropriate age group
Secondary target age: Optional
```

### 8.5 Privacy Policy & Data Safety

**Required URLs:**
- Privacy Policy: `https://yourwebsite.com/privacy`
- Terms of Service: `https://yourwebsite.com/terms` (if applicable)

**Data Safety Section:**
- Complete data collection questionnaire
- Specify what user data you collect
- Explain how data is used and shared

---

## 9. Testing ✅

### 9.1 Internal Testing

```bash
# Add test users in Play Console
# Go to: Testing → Internal testing → Testers
# Add emails: test1@example.com, test2@example.com, etc.
```

### 9.2 Test Subscription Flow

**Test Scenarios:**

1. **New user subscription**
   - Install app
   - Start subscription flow
   - Complete purchase
   - Verify premium features unlock

2. **Subscription management**
   - Cancel subscription
   - Verify grace period handling
   - Test subscription renewal

3. **Edge cases**
   - Poor network connection
   - Payment failure
   - Account switching

### 9.3 RevenueCat Testing

- RevenueCat provides test mode
- Use sandbox environment for testing
- Verify webhook events are firing correctly

### 9.4 Device Testing

**Test on multiple devices:**
- Different Android versions
- Different screen sizes
- Different manufacturers (Samsung, Google, OnePlus, etc.)

### 9.5 Performance Testing

- Monitor app performance
- Check memory usage
- Test startup time
- Verify smooth animations

---

## 10. Additional Commands 💻

### Gradle Commands

```bash
# Check dependencies
./gradlew dependencies

# Run lint checks
./gradlew lint

# Generate proguard mapping
./gradlew assembleRelease

# Check for unused resources
./gradlew shrinkReleaseRes
```

### ADB Commands

```bash
# Install APK
adb install app-release.apk

# Clear app data
adb shell pm clear com.yourcompany.yourapp

# View app logs
adb logcat | grep "YourAppTag"

# Take screenshot
adb shell screencap /sdcard/screenshot.png
adb pull /sdcard/screenshot.png
```

### Firebase CLI Commands

```bash
# Deploy Firebase functions (if used)
firebase deploy --only functions

# View Firebase logs
firebase functions:log

# Test Firebase locally
firebase emulators:start
```

---

## 11. Troubleshooting 🔧

### Issue 1: Google Play API Not Working

**Solutions:**
- Verify service account permissions
- Check if API is enabled in Google Cloud Console
- Ensure JSON key file is correctly uploaded to RevenueCat

### Issue 2: In-App Purchases Not Working

**Check these items:**

Verify product IDs match exactly between:
- Google Play Console
- RevenueCat Dashboard  
- Your app code

Additional checks:
- Check if subscriptions are activated in Play Console
- Ensure app is signed with release key

### Issue 3: Build Issues

```bash
# Clear gradle cache
./gradlew clean
rm -rf ~/.gradle/caches/

# Update gradle wrapper
./gradlew wrapper --gradle-version 8.0
```

---

## 12. Resources 🔗

### Documentation
- [RevenueCat Docs](https://www.revenuecat.com/docs)
- [Google Play Console](https://play.google.com/console)
- [Firebase Documentation](https://firebase.google.com/docs)
- [Android Developer Guides](https://developer.android.com/guide)
- [Google Play Billing](https://developer.android.com/google/play/billing)

### Sample Code
- [RevenueCat Sample Apps](https://github.com/RevenueCat/purchases-android/tree/main/examples)

---

## ✅ Pre-Publishing Checklist

- [ ] App package name is unique and consistent across all platforms
- [ ] Keystore is generated and securely stored
- [ ] Firebase project is properly configured
- [ ] Google Cloud service account has proper permissions
- [ ] RevenueCat integration is working correctly
- [ ] In-app products are created and activated in Google Play
- [ ] App is thoroughly tested with real purchase flow
- [ ] Store listing is complete with all required assets
- [ ] Content rating and data safety sections are completed
- [ ] Privacy policy and terms of service are accessible
- [ ] Internal testing is completed successfully
- [ ] App bundle is signed and ready for upload

---

## 🎉 Success!

**Congratulations! Your app is now ready for the Google Play Store!**

If you found this guide helpful, please ⭐ star this repository and share it with other developers.

---

## 📝 License

This guide is available under the [MIT License](LICENSE).

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the [issues page](../../issues).

---

## 👨‍💻 Author

Created with ❤️ by developers, for developers.

**Last Updated:** September 2025
