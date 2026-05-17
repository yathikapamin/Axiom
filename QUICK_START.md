# 🚀 Quick Start Guide - Authorization System

## ✅ What You Now Have

Your Axiom app now includes a **complete two-layer authorization system**:

1. **🔐 Biometric Authentication** - Fingerprint/device face unlock (fast)
2. **👤 Face Recognition** - ML-based face matching (accurate)
3. **🎤 Voice Codeword** - Emergency unlock fallback

---

## 📱 How to Access Face Enrollment

### Method 1: Through Settings (Recommended)
```
1. Open Axiom app
2. Tap "Settings" from dashboard
3. Scroll down and tap "Unauthorized Access Protection"
4. Find section "👤 Face Recognition"
5. Tap "Enroll Your Face" card
6. Follow the 5-photo enrollment process
```

### Method 2: Direct Navigation
The Face Enrollment screen is now integrated into the main navigation flow.

---

## 📸 Step-by-Step Face Enrollment

### 1. Navigate to Face Enrollment
- **Axiom Dashboard** → **Settings** → **Unauthorized Access Protection** → **Enroll Your Face**

### 2. Grant Camera Permission
- When prompted, tap **"Allow"** for camera access
- This is required to capture your face photos

### 3. Capture 5 Photos
Follow the on-screen instructions:

**Photo 1: Face Forward**
- Look straight at camera
- Center your face
- Neutral expression
- ✅ Tap camera button

**Photo 2: Turn Left**
- Rotate head ~15° to the left
- Keep eyes on camera
- ✅ Tap camera button

**Photo 3: Turn Right**
- Rotate head ~15° to the right
- Keep eyes on camera
- ✅ Tap camera button

**Photo 4: Look Up**
- Tilt head slightly upward
- Maintain centered position
- ✅ Tap camera button

**Photo 5: Smile**
- Natural smile
- Face forward
- ✅ Tap camera button

### 4. Complete Enrollment
- Progress bar shows: 5/5 photos
- Tap **"Complete Enrollment"**
- Wait for processing
- ✅ Success message: "Face enrolled successfully with X templates!"

### 5. Verification
Look for these confirmations:
- ✅ Toast message: "Face enrolled successfully!"
- ✅ In settings: "Face enrolled - Tap to re-enroll"
- ✅ Face data stored locally

---

## 🧪 Quick Testing Guide

### Test 1: Verify Face Enrollment ✅
```bash
# Check logs to confirm enrollment
adb logcat | grep FaceRecognition

# Should see:
# "Face enrollment successful! X templates stored"
# "face_enrolled = true"
```

### Test 2: Test Biometric Authentication ✅
1. Enable protection in settings
2. Select a protected app (e.g., Settings)
3. Exit Axiom (press Home)
4. Open the protected app
5. **Expected:** QuickAuthActivity appears
6. Use fingerprint → Should grant access

### Test 3: Test Face Recognition ✅
1. Open protected app again (after clearing auth)
2. When QuickAuthActivity appears:
   - Cancel biometric prompt
   - Tap **"Verify Face"** button
3. Look at front camera
4. **Expected:** "✅ Face verified" → Access granted

### Test 4: Test Lock Screen ✅
1. In Unauthorized Access Settings
2. Scroll to bottom
3. Tap **"Test Lock Screen"**
4. Emergency lock appears
5. Tap microphone
6. Say: "axiom unlock"
7. **Expected:** Device unlocks

---

## 🎯 Complete Setup Checklist

### ✅ Device Setup
- [ ] Fingerprint enrolled in Android Settings
- [ ] USB Debugging enabled (for testing)
- [ ] Device connected via ADB

### ✅ App Permissions
- [ ] Camera - Granted
- [ ] Microphone - Granted
- [ ] Location - Granted
- [ ] Notifications - Granted
- [ ] Usage Access - **CRITICAL** ✅
- [ ] Battery Optimization - Disabled for Axiom

### ✅ Face Enrollment
- [ ] Navigate to Face Enrollment screen
- [ ] Camera permission granted
- [ ] 5 photos captured successfully
- [ ] Enrollment completed
- [ ] Confirmation message received

### ✅ Protection Setup
- [ ] Voice Lock Protection - Enabled
- [ ] Voice codeword - Set (default: "axiom unlock")
- [ ] Protected apps - Selected (Google Pay, WhatsApp, etc.)
- [ ] Service running - Verified

### ✅ Testing
- [ ] Test Lock Screen - Working
- [ ] Voice unlock - Working
- [ ] Biometric auth - Working
- [ ] Face recognition - Working
- [ ] Protected app triggers auth - Working

---

## 🔧 Troubleshooting

### ❌ "Face Enrollment option not showing"
**Solution:** 
- Pull latest code changes
- Rebuild the app
- Face Enrollment is now in: Settings → Unauthorized Access Protection

### ❌ "Camera permission denied"
**Solution:**
```bash
# Grant camera permission via ADB
adb shell pm grant com.example.axiom android.permission.CAMERA
```

### ❌ "Face enrollment fails"
**Possible causes:**
1. Poor lighting → Move to brighter area
2. Camera not clean → Clean camera lens
3. Moving too fast → Take photos slower
4. Face not centered → Center face in preview

**Solution:**
- Retry in good natural lighting
- Keep face centered
- Don't move between captures

### ❌ "Face not recognized after enrollment"
**Check similarity score:**
```bash
adb logcat | grep "Similarity"
# Should show: "Similarity: 0.XX"
```

**If similarity < 0.85:**
- Re-enroll in same lighting as you'll use
- Ensure camera is clean
- Lower threshold if needed (FaceRecognitionService.kt line 43)

### ❌ "QuickAuthActivity doesn't appear"
**Check service status:**
```bash
adb logcat | grep UnauthorizedAccess
# Should show: "Service started", "Started monitoring"
```

**Grant Usage Access:**
```bash
adb shell appops set com.example.axiom PACKAGE_USAGE_STATS allow
```

---

## 📊 Expected Flow

### First Time User Experience:

```
1. Install app
   ↓
2. Grant permissions
   ↓
3. Go to Settings → Unauthorized Access Protection
   ↓
4. Tap "Enroll Your Face"
   ↓
5. Capture 5 photos
   ↓
6. ✅ Face enrolled
   ↓
7. Enable protection
   ↓
8. Select protected apps
   ↓
9. Open protected app
   ↓
10. QuickAuthActivity appears
    ↓
11. Use fingerprint (fast) OR face recognition
    ↓
12. ✅ Access granted for 5 minutes
    ↓
13. Repeat auth after 5 minutes
```

### Returning User (Already Enrolled):

```
1. Open protected app
   ↓
2. QuickAuthActivity appears
   ↓
3. Touch fingerprint sensor
   ↓
4. ✅ Instant access (< 1 second)
   ↓
5. No interruption for 5 minutes
```

### Unauthorized Person Attempt:

```
1. Someone else opens protected app
   ↓
2. QuickAuthActivity appears
   ↓
3. Their fingerprint → ❌ Fails
   ↓
4. Their face → ❌ Doesn't match (< 85% similarity)
   ↓
5. After 3 attempts → 🚨 Device locked
   ↓
6. EmergencyLockActivity appears
   ↓
7. Only voice codeword can unlock
   ↓
8. Failed attempts → Intruder photo captured
   ↓
9. Alert sent to trusted contact
```

---

## 🎬 Video Walkthrough Steps

If creating a demo video:

1. **App Launch** (0:00-0:10)
   - Show Axiom dashboard

2. **Navigate to Face Enrollment** (0:10-0:20)
   - Settings → Unauthorized Access Protection
   - Show Face Recognition section

3. **Enrollment Process** (0:20-1:00)
   - Tap "Enroll Your Face"
   - Capture all 5 photos
   - Show progress bar
   - Success message

4. **Enable Protection** (1:00-1:20)
   - Toggle Voice Lock Protection ON
   - Select protected apps
   - Show service running

5. **Test Authentication** (1:20-2:00)
   - Open protected app
   - Show QuickAuthActivity
   - Demonstrate fingerprint auth
   - Show face recognition

6. **Test Lock Screen** (2:00-2:30)
   - Trigger test lock
   - Show emergency screen
   - Voice unlock demo

---

## 🚀 Build & Deploy

### Build APK:
```bash
cd "D:\project by sujal\axiom"
./gradlew assembleDebug
```

### Install:
```bash
adb install -r app/build/outputs/apk/debug/app-debug.apk
```

### Monitor Logs:
```bash
# All authorization events
adb logcat | grep -E "QuickAuth|BiometricAuth|FaceRecognition|UnauthorizedAccess"

# Just face recognition
adb logcat | grep FaceRecognition
```

---

## 📞 Support

### Check Logs for Issues:
```bash
# Real-time monitoring
adb logcat -c && adb logcat | grep -E "Axiom|Face|Auth|Unauthorized"

# Save logs to file
adb logcat > axiom_logs.txt
```

### Common Success Messages:
```
✅ Face enrolled successfully! 5 templates stored
✅ User authorized (verified 45s ago)
✅ Authentication succeeded!
✅ Face verified (confidence: 89%)
✅ Access granted
```

### Common Error Messages:
```
❌ No face detected in photo
❌ Need at least 3 clear face photos
❌ Face not recognized
🚨 LOCKING DEVICE - Unauthorized access
```

---

## 🎉 You're All Set!

Your authorization system is now complete with:
- ✅ Face Enrollment screen integrated
- ✅ Two-layer authentication (biometric + face)
- ✅ Voice codeword emergency unlock
- ✅ Protected app monitoring
- ✅ Intruder detection & lockdown

**Next:** Just build, install, and test! 🚀

---

## 📝 Navigation Path Reference

```
Axiom Dashboard
    └── Settings
        └── Unauthorized Access Protection
            ├── 🤫 Silent Alert Mode (toggle)
            ├── 🔒 Voice Lock Protection (toggle)
            ├── 👤 Face Recognition
            │   └── Enroll Your Face → FaceEnrollmentScreen
            ├── 🎤 Voice Codeword (edit)
            ├── 🛡️ Protected Apps (select)
            └── Test Lock Screen (button)
```

**Path to Face Enrollment:**
`Dashboard → Settings → Unauthorized Access Protection → Enroll Your Face`

That's it! Face enrollment is now fully integrated and accessible. 🎊
