# ⚡ Axiom Security - Quick Start Guide

Get Axiom Security up and running in 5 minutes!

---

## 🚀 Quick Setup (Developers)

### Prerequisites
- Android Studio Hedgehog or later
- JDK 11+
- Android SDK 26+

### Step 1: Clone & Open
```bash
git clone https://github.com/yourusername/axiom-security.git
cd axiom-security
```
Open in Android Studio → Wait for Gradle sync

### Step 2: Run the App
1. Connect Android device or start emulator
2. Click **Run** (Green play button)
3. Grant permissions when prompted
4. Set up biometric authentication

### Step 3: Test Core Features

**Test Phishing Detection (30 seconds)**
```
1. Open app
2. Tap "Scan Link"
3. Paste: http://bit.ly/test
4. See detection result + voice alert
```

**Test Image Scanner (1 minute)**
```
1. Create test image with text: "URGENT! Verify account now"
2. Tap "Scan Image"
3. Select the image
4. See ML-powered detection
```

**Test Emergency Alert (2 minutes)**
```
1. Go to Settings
2. Enter your phone: +91XXXXXXXXXX
3. Tap "Test Emergency Alert"
4. Check WhatsApp/SMS on that number
```

---

## 📱 Quick Setup (End Users)

### Install & Configure
1. **Install** Axiom Security APK
2. **Grant Permissions** (Camera, Location, SMS)
3. **Set Trusted Contact** in Settings
4. **Enable Notification Access**: 
   - Settings → Apps → Special Access → Notification Access → Enable Axiom

### First Scan
- Copy suspicious link → Open Axiom → Scan Link
- Voice tells you if it's dangerous!

---

## 🔧 Development Tips

### Project Structure
```
app/src/main/java/com/example/axiom/
├── data/           # Database models & DAOs
├── services/       # Core detection services
├── viewmodel/      # MVVM state management
├── ui/screens/     # Compose UI screens
├── receivers/      # Broadcast receivers
└── MainActivity.kt # Entry point
```

### Key Files to Modify

**Add new threat type:**
- `data/ThreatLog.kt` → Add to `ThreatType` enum

**Modify detection logic:**
- `services/PhishingDetectionService.kt` → Update scoring algorithm

**Change UI:**
- `ui/screens/DashboardScreen.kt` → Modify layout

### Build Variants
```bash
# Debug build
./gradlew assembleDebug

# Release build (sign first)
./gradlew assembleRelease
```

---

## 🐛 Common Issues

**Issue**: "Room database error"
**Fix**: Rebuild project (Build → Rebuild Project)

**Issue**: "Camera not working on emulator"
**Fix**: Use physical device for camera features

**Issue**: "WhatsApp intent fails"
**Fix**: Ensure WhatsApp installed on device

**Issue**: "Voice not working"
**Fix**: Check volume & install Google TTS

---

## 📚 Next Steps

- Read [README.md](README.md) for architecture details
- Check [USER_GUIDE.md](USER_GUIDE.md) for features
- See [TESTING_GUIDE.md](TESTING_GUIDE.md) for testing

---

## 🎯 Quick Test Script

Run all core features in 5 minutes:

```bash
✅ Launch app (biometric works)
✅ Scan safe URL: https://google.com (Low score)
✅ Scan phishing URL: http://verify.tk (High score, voice alert)
✅ Scan image with "urgent verify" text (Detection works)
✅ Settings → Save contact → Test alert (Message received)
✅ Read logs aloud (Voice reads threats)
✅ Mark log as read (Badge decreases)
```

**All working? You're ready to go! 🎉**

---

## 🆘 Need Help?

- **Bugs**: Open GitHub issue
- **Questions**: Check USER_GUIDE.md
- **Contributions**: See CONTRIBUTING.md (future)

---

**Start protecting users today! 🛡️**
