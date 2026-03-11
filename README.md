# Bharat Vistaar — React Native App

A React Native WebView app built with Expo that loads the Bharat Vistaar chat interface for both iOS and Android.

---

## Tech Stack

- **Framework:** React Native + Expo (latest)
- **Routing:** Expo Router
- **WebView:** `react-native-webview@13.16.1`
- **Language:** TypeScript
- **Target URL:** `https://chat-vistaar.da.gov.in/chat`

---

## Project Structure

```
bharat-react-native-app/
├── android/                  # Android native project (generated)
├── ios/                      # iOS native project (generated)
├── assets/                   # App icons, splash screen
├── App.tsx                   # Main app entry — WebView wrapper
├── index.ts                  # Entry point
├── app.json                  # Expo configuration
├── package.json
└── tsconfig.json
```

---

## Prerequisites

- Node.js v20+
- Xcode 15+ (for iOS builds)
- Android Studio (for Android builds)
- Apple Developer Account
- CocoaPods (`sudo gem install cocoapods`)
- Expo CLI (`npm install -g expo-cli`)

---

## Getting Started

### 1. Clone & Install

```bash
git clone <repo-url>
cd bharat-react-native-app
npm install
```

### 2. Generate Native Folders

```bash
npx expo prebuild --clean
```

### 3. Start Dev Server

```bash
npx expo start --clear
```

---

## iOS Build & TestFlight

### Run Locally

```bash
npx expo run:ios
```

### Archive & Upload to TestFlight

1. Open Xcode:
```bash
open ios/BharatVistaar.xcworkspace
```

2. In Xcode:
   - Select **Any iOS Device (arm64)** from the device picker
   - Go to **Signing & Capabilities** → set your Team
   - Verify Bundle ID: `com.gov.in.vistaar`

3. Archive:
   - Menu → **Product → Archive**
   - Wait for build to complete

4. Distribute:
   - **Xcode Organizer** → select archive
   - Click **Distribute App → App Store Connect → Upload**

5. TestFlight:
   - Go to [appstoreconnect.apple.com](https://appstoreconnect.apple.com)
   - Your App → **TestFlight** tab
   - Wait ~15 minutes for processing
   - Add testers and send invite

> **Note:** Bump `buildNumber` in `app.json` for each new upload.

---

## Android Build

### Run Locally

```bash
npx expo run:android
```

### Generate APK

```bash
cd android
./gradlew assembleRelease
```

APK location:
```
android/app/build/outputs/apk/release/app-release.apk
```

---

## App Configuration (`app.json`)

```json
{
  "expo": {
    "name": "Bharat Vistaar",
    "slug": "bharat-react-native-app",
    "version": "1.0.0",
    "ios": {
      "bundleIdentifier": "com.gov.in.vistaar",
      "buildNumber": "2",
      "infoPlist": {
        "NSMicrophoneUsageDescription": "This app needs microphone access for voice chat.",
        "NSAppTransportSecurity": {
          "NSAllowsArbitraryLoads": true,
          "NSAllowsArbitraryLoadsInWebContent": true
        }
      }
    },
    "android": {
      "package": "com.gov.in.vistaar",
      "versionCode": 1,
      "permissions": ["RECORD_AUDIO", "MODIFY_AUDIO_SETTINGS"]
    }
  }
}
```

---

## Permissions

| Permission | Platform | Reason |
|---|---|---|
| `NSMicrophoneUsageDescription` | iOS | Voice chat in WebView |
| `RECORD_AUDIO` | Android | Voice chat in WebView |
| `MODIFY_AUDIO_SETTINGS` | Android | Audio configuration for voice |

---

## Known Issues & Fixes

### TLS Error -1200 on iOS
**Cause:** iOS ATS rejects non-standard SSL certificates (common with `.gov.in` domains).  
**Fix:** Add `NSAppTransportSecurity` with `NSAllowsArbitraryLoads: true` in `app.json` `infoPlist`.

### `expo-router/internal/routing` not found
**Cause:** Version mismatch between `expo-router` and `@expo/cli`.  
**Fix:**
```bash
rm -rf node_modules package-lock.json
npm install
npx expo install --fix
npx expo start --clear
```

### Watchman Recrawl Warning
**Fix:**
```bash
watchman watch-del '/path/to/project' ; watchman watch-project '/path/to/project'
```

### WebView name changed after prebuild
**Cause:** Expo uses `name` field in `app.json` to generate the iOS folder name.  
**Fix:** Keep `name` consistent in `app.json` before running `prebuild`.

---

## Releasing New Versions

1. Bump `version` in `app.json` (e.g. `1.0.1`)
2. Bump `buildNumber` for iOS (e.g. `3`)
3. Bump `versionCode` for Android (e.g. `2`)
4. Run:
```bash
npx expo prebuild --clean
```
5. Archive in Xcode and upload to TestFlight

---

## Team

- **Organization:** Ministry of Agriculture and Farmer Welfare
- **Apple Team:** Ministry Of Agriculture and Farmer Welfare
- **Bundle ID:** `com.gov.in.vistaar`
- **Android Package:** `com.gov.in.vistaar`