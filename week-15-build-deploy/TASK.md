# Week 15: Build & Deploy
> **Month 4 | Week 15 of 16** | **Difficulty:** Advanced

---

## Goal
Ship your app to the App Store and Play Store.

---

## Tasks

### Day 1: EAS Build Setup
- [ ] Install EAS CLI: `npm install -g eas-cli`
- [ ] Login: `eas login`
- [ ] Initialize: `eas build:configure`
- [ ] Review `eas.json`:
  ```json
  {
    "build": {
      "development": {
        "developmentClient": true,
        "distribution": "internal"
      },
      "preview": {
        "distribution": "internal"
      },
      "production": {}
    }
  }
  ```
- [ ] Verify: `eas build:list` shows your project

### Day 2: iOS App Store Setup
- [ ] Create Apple Developer account ($99/year)
- [ ] In App Store Connect:
  - Create new app
  - Set bundle ID, name, SKU
  - Fill in basic info
- [ ] Generate certificates:
  ```bash
  eas credentials
  # Select iOS → Certificate → Generate new
  ```
- [ ] Build for iOS:
  ```bash
  eas build --platform ios --profile preview
  ```
- [ ] Verify: Build completes, binary available for download

### Day 3: Google Play Store Setup
- [ ] Create Google Play Developer account ($25 one-time)
- [ ] Create new app in Play Console
- [ ] Generate keystore:
  ```bash
  eas credentials
  # Select Android → Keystore → Generate new
  ```
- [ ] Build for Android:
  ```bash
  eas build --platform android --profile preview
  ```
- [ ] Verify: APK/AAB available for download

### Day 4: App Store Submission
- [ ] Submit iOS:
  ```bash
  eas submit --platform ios --profile preview
  ```
- [ ] Submit Android:
  ```bash
  eas submit --platform android --profile preview
  ```
- [ ] Fill in app store listings:
  - Description, keywords, screenshots
  - Privacy policy URL
  - App category
  - Content rating
- [ ] Verify: Apps appear in store review queue

### Day 5: Production Build
- [ ] Final production build:
  ```bash
  eas build --platform ios --profile production
  eas build --platform android --profile production
  ```
- [ ] Submit to production:
  ```bash
  eas submit --platform ios --profile production
  eas submit --platform android --profile production
  ```
- [ ] Set up OTA updates:
  ```bash
  eas update --branch production --message "Initial release"
  ```
- [ ] Verify: Production builds submit successfully

---

## App Store Checklist

### Before Submission
- [ ] App icon (1024x1024)
- [ ] Splash screen
- [ ] Screenshots for each device size
- [ ] App description (170 chars max for subtitle)
- [ ] Keywords
- [ ] Privacy policy URL
- [ ] Support URL
- [ ] Content rating questionnaire completed

### Technical Requirements
- [ ] Bundle ID matches
- [ ] Version number incremented
- [ ] No console.log in production
- [ ] No development URLs
- [ ] Proper permissions declared
- [ ] Works on latest 2 OS versions

---

## Verification

```bash
eas build:list          # Check build status
eas submit:list         # Check submission status

# After approval:
# 1. App appears in App Store
# 2. App appears in Play Store
# 3. Download and install works
# 4. All features work in production
# 5. OTA updates apply correctly
```
