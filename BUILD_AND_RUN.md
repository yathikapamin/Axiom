# 🚀 Build and Run Instructions

## Prerequisites Checklist

Before building, ensure you have:
- ✅ Android Studio Hedgehog (2023.1.1) or later
- ✅ JDK 11 or higher
- ✅ Android SDK 26+ installed
- ✅ Gradle 8.0+
- ✅ Physical Android device (recommended) or Emulator

---

## Step-by-Step Build Instructions

### 1. Sync Gradle Dependencies

```bash
# In Android Studio:
File → Sync Project with Gradle Files

# Or via command line:
./gradlew build --refresh-dependencies
```

**Expected Output**: All dependencies downloaded successfully

### 2. Build Debug APK

```bash
# Command line:
./gradlew assembleDebug

# Output location:
app/build/outputs/apk/debug/app-debug.apk
```

### 3. Install on Device

**Option A: Android Studio**
1. Connect device via USB
2. Enable Developer Options + USB Debugging
3. Click Run (Green play button)
4. Select device

**Option B: Command Line**
```bash
adb install app/build/outputs/apk/debug/app-debug.apk
```

**Option C: Manual Install**
1. Transfer APK to device
2. Open file manager
3. Tap APK → Install

---

## First Launch Configuration

### Step 1: Grant Permissions (CRITICAL)
App will request these permissions - **Grant ALL for full functionality**:

1. **Camera** ✅
2. **Location** ✅  
3. **SMS** ✅
4. **Notifications** ✅
5. **Biometric/Device Credential** ✅

### Step 2: Enable Notification Access (REQUIRED for Financial Fraud Monitoring)

```
Device Settings 
→ Apps 
→ Special App Access 
→ Notification Access 
→ Axiom Security 
→ Toggle ON
```

### Step 3: Set Up Trusted Contact

1. Open Axiom Security
2. Complete biometric authentication
3. Tap Settings (gear icon)
4. Enter trusted contact: **+CountryCode PhoneNumber**
   - Example: +919876543210
5. Tap **Save Contact**
6. Tap **Test Emergency Alert** to verify

### Step 4: Test Core Features

**Quick Test (2 minutes):**
```
✅ Dashboard loads
✅ Tap "Scan Link" → Paste URL → See result
✅ Tap "Scan Image" → Select image → See analysis  
✅ Tap speaker icon → Hear voice readout
✅ Settings → Test Alert → Check WhatsApp/SMS
```

---

## Troubleshooting Build Issues

### Issue: "Room database cannot find schema"
**Solution**:
```bash
Build → Clean Project
Build → Rebuild Project
```

### Issue: "KSP plugin not found"
**Solution**: Check `app/build.gradle.kts` line 5:
```kotlin
id("com.google.devtools.ksp") version "1.9.20-1.0.14"
```

### Issue: "Compose compiler version mismatch"
**Solution**: Sync Gradle and update Compose compiler version

### Issue: "Duplicate class found"
**Solution**:
```bash
./gradlew clean
./gradlew build --refresh-dependencies
```

### Issue: "AAPT2 error" or "Resource not found"
**Solution**:
```bash
# Invalidate caches
File → Invalidate Caches → Invalidate and Restart
```

---

## Runtime Troubleshooting

### Issue: "App crashes on launch"
**Check**:
1. Min SDK is 26 (Android 8.0+)
2. All dependencies synced
3. LogCat for error messages

### Issue: "Voice alerts not working"
**Solution**:
1. Increase device volume
2. Install Google TTS from Play Store
3. Test: Settings → Language & Input → Text-to-Speech

### Issue: "Camera not opening"
**Solution**:
1. Grant Camera permission manually
2. Use physical device (not emulator)
3. Check if another app is using camera

### Issue: "Emergency alert not sending"
**Solution**:
1. Verify trusted contact saved in Settings
2. Check internet connection (WhatsApp)
3. Verify SMS permission granted
4. Ensure WhatsApp installed

### Issue: "Financial fraud monitoring not working"
**Solution**:
1. Enable Notification Access (see Step 2 above)
2. Restart app after enabling
3. Make test transaction in payment app
4. Check threat logs for detection

---

## Release Build (Future)

### 1. Generate Keystore
```bash
keytool -genkey -v -keystore axiom-release.jks -keyalg RSA -keysize 2048 -validity 10000 -alias axiom
```

### 2. Configure Signing

Create `keystore.properties`:
```properties
storePassword=YourStorePassword
keyPassword=YourKeyPassword
keyAlias=axiom
storeFile=axiom-release.jks
```

### 3. Update build.gradle.kts
```kotlin
android {
    signingConfigs {
        create("release") {
            // Load keystore
            storeFile = file("axiom-release.jks")
            storePassword = "YourStorePassword"
            keyAlias = "axiom"
            keyPassword = "YourKeyPassword"
        }
    }
    buildTypes {
        release {
            signingConfig = signingConfigs.getByName("release")
            isMinifyEnabled = true
            proguardFiles(...)
        }
    }
}
```

### 4. Build Release APK
```bash
./gradlew assembleRelease
```

### 5. Verify Signature
```bash
jarsigner -verify -verbose -certs app/build/outputs/apk/release/app-release.apk
```

---

## Performance Optimization

### Enable R8 (Minification)
```kotlin
buildTypes {
    release {
        isMinifyEnabled = true
        isShrinkResources = true
    }
}
```

### ProGuard Rules
Already configured in `proguard-rules.pro`

---

## Deployment Checklist

Before releasing to users:

- [ ] All permissions tested
- [ ] Biometric authentication works
- [ ] Phishing detection accurate
- [ ] Image scanning functional
- [ ] Emergency alerts delivered
- [ ] Voice alerts audible
- [ ] Financial monitoring active
- [ ] Database operations stable
- [ ] No crashes in 1-hour test
- [ ] Memory leaks checked
- [ ] Battery drain acceptable (<5%/hr)
- [ ] All documentation reviewed
- [ ] Privacy policy created (if needed)
- [ ] Play Store listing ready

---

## APK Distribution

### Internal Testing
1. Build debug APK
2. Share via email/drive
3. Users enable "Install from Unknown Sources"
4. Install and test

### Play Store (Future)
1. Build release APK
2. Create Play Console account
3. Upload APK/AAB
4. Complete store listing
5. Submit for review

### Direct Distribution
1. Host APK on website
2. Generate QR code for download
3. Provide installation instructions

---

## Version Management

### Update Version
In `app/build.gradle.kts`:
```kotlin
defaultConfig {
    versionCode = 2  // Increment for each release
    versionName = "1.1"  // Semantic versioning
}
```

---

## Monitoring After Deployment

### Check These Metrics
- Crash rate (target: <0.1%)
- ANR rate (target: <0.01%)
- Permission grant rate
- Feature usage (phishing scans, image scans)
- Alert delivery success rate
- User retention

### Firebase Crashlytics (Optional Future)
```kotlin
// Add to dependencies
implementation("com.google.firebase:firebase-crashlytics")
```

---

## Support Resources

**Documentation**:
- [README.md](README.md) - Technical overview
- [USER_GUIDE.md](USER_GUIDE.md) - End user instructions
- [TESTING_GUIDE.md](TESTING_GUIDE.md) - QA procedures
- [QUICKSTART.md](QUICKSTART.md) - 5-minute setup

**Debugging**:
```bash
# View logs
adb logcat | grep "AxiomSecurity"

# Clear app data
adb shell pm clear com.example.axiom

# Check installed version
adb shell dumpsys package com.example.axiom | grep versionName
```

---

## Final Verification Script

Run this after building:
```bash
#!/bin/bash
echo "🔍 Verifying Axiom Security Build..."

# Check APK exists
if [ -f "app/build/outputs/apk/debug/app-debug.apk" ]; then
    echo "✅ APK built successfully"
else
    echo "❌ APK not found"
    exit 1
fi

# Check APK size (should be 15-25 MB)
size=$(wc -c < "app/build/outputs/apk/debug/app-debug.apk")
echo "📦 APK Size: $((size / 1024 / 1024)) MB"

# Install on connected device
echo "📱 Installing on device..."
adb install -r app/build/outputs/apk/debug/app-debug.apk

if [ $? -eq 0 ]; then
    echo "✅ Installation successful"
    echo "🚀 Launch Axiom Security and test features"
else
    echo "❌ Installation failed"
    exit 1
fi

echo "✅ Build verification complete!"
```

---

## Quick Reference Commands

```bash
# Build
./gradlew assembleDebug

# Install
adb install -r app/build/outputs/apk/debug/app-debug.apk

# Launch
adb shell am start -n com.example.axiom/.MainActivity

# View logs
adb logcat | grep "Axiom"

# Uninstall
adb uninstall com.example.axiom

# Check device
adb devices
```

---

**You're ready to build and deploy Axiom Security! 🎉**

For questions or issues, refer to the documentation or check LogCat output.

**Happy Building! 🛠️**
