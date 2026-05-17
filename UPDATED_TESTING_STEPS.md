# ✅ Updated Testing Steps - Face Enrollment Now Available!

## 🎉 What's New

Face Enrollment is now integrated into the app! Here's where to find it:

**Path:** Dashboard → Settings → Unauthorized Access Protection → **Enroll Your Face**

---

## 📱 Step-by-Step Testing (Updated)

### ✅ Step 1: Build & Install
```bash
cd "D:\project by sujal\axiom"
./gradlew assembleDebug
adb install -r app/build/outputs/apk/debug/app-debug.apk
```

### ✅ Step 2: Grant Permissions
1. Open Axiom app
2. Allow all permissions when prompted
3. **CRITICAL:** Grant Usage Access
   - Settings → Apps → Special app access → Usage access → Axiom → Enable
   - OR via ADB:
   ```bash
   adb shell appops set com.example.axiom PACKAGE_USAGE_STATS allow
   ```

### ✅ Step 3: Navigate to Face Enrollment ⭐ NEW!
1. From Dashboard, tap **"Settings"**
2. Scroll down, tap **"Unauthorized Access Protection"**
3. Find **"👤 Face Recognition"** section
4. Tap the **"Enroll Your Face"** card

![Navigation Path]
```
Axiom App
  └─ Settings (gear icon)
      └─ Unauthorized Access Protection
          └─ Face Recognition
              └─ [Tap] Enroll Your Face
```

### ✅ Step 4: Enroll Your Face
1. **Grant Camera Permission** when prompted
2. **Capture 5 photos:**
   - 📸 Photo 1: Face forward, centered
   - 📸 Photo 2: Turn head left (~15°)
   - 📸 Photo 3: Turn head right (~15°)
   - 📸 Photo 4: Tilt head up slightly
   - 📸 Photo 5: Smile naturally

3. **Watch progress bar:** Should show 1/5, 2/5, etc.

4. **Complete enrollment:**
   - Tap **"Complete Enrollment"** button
   - Wait for processing (2-5 seconds)
   - Look for success message

5. **Verify success:**
   - ✅ Toast: "Face enrolled successfully!"
   - ✅ Card now shows: "✅ Face enrolled - Tap to re-enroll"

### ✅ Step 5: Check Logs (Optional)
```bash
adb logcat | grep FaceRecognition

# Expected output:
# FaceRecognition: Enrolling owner's face with 5 photos
# FaceRecognition: Processing photo 1/5
# FaceRecognition: Extracted template from photo 1
# ...
# FaceRecognition: ✅ Face enrollment successful! 5 templates stored
```

### ✅ Step 6: Enable Protection
Still in **Unauthorized Access Protection** screen:

1. **Enable Voice Lock Protection:**
   - Toggle **"🔒 Voice Lock Protection"** → ON
   - Should see "Service started" in logs

2. **Set Voice Codeword** (optional):
   - Tap "Current Codeword"
   - Change if desired (default: "axiom unlock")
   - Save

3. **Select Protected Apps:**
   - Scroll to "🛡️ Protected Apps"
   - Toggle ON for apps you want to protect:
     - ✅ Google Pay
     - ✅ WhatsApp  
     - ✅ Gmail
     - ✅ Settings
     - etc.

### ✅ Step 7: Test Biometric Authentication
1. **Exit Axiom** (press Home button)
2. **Open a protected app** (e.g., Settings)
3. **QuickAuthActivity should appear:**
   - Blue/white screen
   - "Verify Your Identity"
   - Shows app name
   - **Biometric prompt appears automatically**

4. **Test fingerprint:**
   - Touch fingerprint sensor
   - **Expected:** "✅ Biometric verified" → App opens

5. **Check logs:**
```bash
adb logcat | grep -E "QuickAuth|BiometricAuth"
# Look for: "Authentication succeeded!"
```

### ✅ Step 8: Test Face Recognition
1. **Wait 5+ minutes** OR clear authorization:
```bash
# To test immediately, clear auth manually
adb shell pm clear com.example.axiom
# Then reopen app and re-enable protection
```

2. **Open protected app again**
3. **When QuickAuthActivity appears:**
   - Click **"Cancel"** on biometric prompt
   - Face recognition section should activate
   
4. **Verify face:**
   - Camera preview appears
   - Tap **"Verify Face"** button
   - Look at front camera
   - Wait for capture & verification

5. **Expected Result:**
   - "✅ Face verified" message
   - App opens
   - Access granted for 5 minutes

6. **Check logs:**
```bash
adb logcat | grep FaceRecognition
# Look for: "Similarity: 0.XX, Match: true"
# Similarity should be ≥ 0.85
```

### ✅ Step 9: Test Lock Screen
1. **In Unauthorized Access Protection screen**
2. **Scroll to bottom**
3. **Tap "Test Lock Screen"** button
4. **Emergency lock screen appears** (red background)
5. **Test voice unlock:**
   - Tap microphone button
   - Grant microphone permission if needed
   - Say clearly: **"axiom unlock"**
   - **Expected:** "✅ Codeword verified! Unlocking..."

### ✅ Step 10: Test Failed Auth (Optional)
1. Have someone else try:
   - Open a protected app
   - Their fingerprint → ❌ Fails
   - Their face → ❌ < 85% match
2. After 3 attempts → Device locks
3. Emergency screen appears
4. Only voice codeword unlocks

---

## 🎯 Complete Verification Checklist

### Setup Complete ✅
- [ ] App built successfully
- [ ] App installed on device
- [ ] All permissions granted
- [ ] Usage Access enabled
- [ ] Battery optimization disabled

### Face Enrollment Complete ✅
- [ ] Can navigate to Face Enrollment screen
- [ ] Camera permission granted
- [ ] 5 photos captured
- [ ] Enrollment completed successfully
- [ ] "✅ Face enrolled" shows in settings
- [ ] Logs confirm: "Face enrollment successful!"

### Protection Enabled ✅
- [ ] Voice Lock Protection toggled ON
- [ ] Voice codeword set
- [ ] Protected apps selected
- [ ] Service running (check logs)

### Authentication Works ✅
- [ ] Biometric prompt appears when opening protected app
- [ ] Fingerprint authentication works
- [ ] Face recognition works
- [ ] Authorization lasts 5 minutes
- [ ] Re-authentication required after timeout

### Lock Screen Works ✅
- [ ] Test lock screen appears
- [ ] Voice recognition works
- [ ] Codeword unlocks device
- [ ] Failed attempts trigger lockdown

---

## 🐛 Troubleshooting

### ❌ "Face Enrollment option not visible"
**Fix:** You need the latest code changes
```bash
git pull origin main  # If using git
# Then rebuild and reinstall
./gradlew clean assembleDebug
adb install -r app/build/outputs/apk/debug/app-debug.apk
```

### ❌ "Status still shows 'Tap to enroll' after enrolling"
**Fix:** This is now fixed! The status checks the correct SharedPreferences
- Rebuild app
- Re-enroll face
- Should show "✅ Face enrolled - Tap to re-enroll"

### ❌ "Camera permission denied"
**Fix:**
```bash
adb shell pm grant com.example.axiom android.permission.CAMERA
```

### ❌ "Face not recognized (similarity too low)"
**Check similarity in logs:**
```bash
adb logcat | grep "Similarity"
# Output: Similarity: 0.XX
```

**If < 0.85:**
- Re-enroll in better lighting
- Ensure camera is clean
- Try different environment
- OR lower threshold temporarily:
```kotlin
// FaceRecognitionService.kt line 43
private const val FACE_SIMILARITY_THRESHOLD = 0.75f  // More lenient
```

### ❌ "QuickAuthActivity doesn't appear"
**Check if service is running:**
```bash
adb logcat | grep UnauthorizedAccess
# Should see: "Service started", "Started monitoring"
```

**Grant Usage Access:**
```bash
adb shell appops set com.example.axiom PACKAGE_USAGE_STATS allow
```

**Restart service:**
```bash
adb shell am force-stop com.example.axiom
# Open app again
```

---

## 📊 Success Metrics

### Enrollment Success Rate
- **Target:** 90%+ first-time enrollment success
- **Monitor:** Logs should show all 5 photos processed
- **Issue if:** < 3 photos processed → lighting/camera issue

### Authentication Speed
- **Biometric:** < 1 second ✅
- **Face Recognition:** 2-3 seconds ✅
- **Service Detection:** ~1 second ✅

### Recognition Accuracy
- **Owner:** Should match 90%+ of the time (similarity ≥ 0.85)
- **Others:** Should reject 95%+ of the time (similarity < 0.85)
- **False Positive Rate:** < 5%
- **False Negative Rate:** < 10%

---

## 🎬 Demo Script

For a complete demo:

**1. Introduction (0:00-0:30)**
- "Axiom now has face recognition"
- "Two-layer authentication"
- "Let me show you how to set it up"

**2. Navigate to Face Enrollment (0:30-1:00)**
- Open app
- Settings → Unauthorized Access Protection
- Show Face Recognition section
- "Here's the Enroll Your Face option"

**3. Enrollment Process (1:00-2:00)**
- Tap Enroll Your Face
- Grant camera permission
- Capture 5 photos (show each angle)
- Complete enrollment
- Show success message

**4. Enable Protection (2:00-2:30)**
- Toggle Voice Lock Protection ON
- Select protected apps
- "Now let's test it"

**5. Test Authentication (2:30-3:30)**
- Exit app
- Open protected app
- Show QuickAuthActivity
- Demonstrate fingerprint
- Show face recognition
- "Access granted for 5 minutes"

**6. Test Lock Screen (3:30-4:00)**
- Trigger test lock
- Show emergency screen
- Voice unlock demo
- "Only my voice can unlock"

**7. Wrap Up (4:00-4:30)**
- Recap features
- Show logs (optional)
- "Your device is now protected"

---

## 📱 Where to Find Face Enrollment

### Visual Navigation:
```
┌─────────────────────────┐
│   Axiom Dashboard       │
│                         │
│  [Scan Link]            │
│  [Scan Image]           │
│  [Settings] ← Click     │
└─────────────────────────┘
            ↓
┌─────────────────────────┐
│   Settings              │
│                         │
│  • Trusted Contact      │
│  • Clear Logs           │
│  • Test Alert           │
│  • Notification Access  │
│  • Unauthorized Access  │  
│    Protection ← Click   │
└─────────────────────────┘
            ↓
┌─────────────────────────┐
│ Unauthorized Access     │
│ Protection              │
│                         │
│ 🤫 Silent Alert Mode    │
│ 🔒 Voice Lock Protection│
│                         │
│ 👤 Face Recognition     │
│ ┌─────────────────────┐ │
│ │ Enroll Your Face    │ │← Click
│ │ Tap to enroll...    │ │
│ └─────────────────────┘ │
│                         │
│ 🎤 Voice Codeword       │
│ 🛡️ Protected Apps       │
│ [Test Lock Screen]      │
└─────────────────────────┘
            ↓
┌─────────────────────────┐
│  Face Enrollment        │
│                         │
│  Step 1 of 5            │
│  Face forward           │
│                         │
│  [Camera Preview]       │
│                         │
│  [Capture Photo] ← Tap  │
└─────────────────────────┘
```

---

## ✅ Final Checklist

Before considering setup complete:

- [ ] ✅ Face Enrollment screen accessible
- [ ] ✅ 5 photos captured successfully
- [ ] ✅ Enrollment confirmation received
- [ ] ✅ Face enrolled status shows in settings
- [ ] ✅ Protection enabled
- [ ] ✅ Protected apps selected
- [ ] ✅ Biometric authentication works
- [ ] ✅ Face recognition works
- [ ] ✅ Voice unlock works
- [ ] ✅ Service survives app restart
- [ ] ✅ Logs show no errors

---

## 🎉 You're Done!

Your authorization system is now **fully functional** with:
- ✅ Face Enrollment integrated and accessible
- ✅ Two-layer authentication (biometric + face)
- ✅ Voice codeword emergency unlock
- ✅ Real-time app monitoring
- ✅ Automatic intruder detection

**Happy testing! 🚀**
