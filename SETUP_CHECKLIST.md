# 🚀 Axiom Authorization Setup Checklist

## Before First Use

### ✅ Step 1: Device Biometric Setup
- [ ] Go to Android Settings → Security
- [ ] Add fingerprint(s) to device
- [ ] Or enable face unlock (if available)
- [ ] Test biometric by unlocking your phone

### ✅ Step 2: Grant Permissions
Open Axiom and grant these permissions:
- [ ] 📷 **Camera** - For face recognition
- [ ] 🔐 **Usage Access** - For monitoring protected apps
- [ ] 🎤 **Microphone** - For voice codeword unlock
- [ ] 📱 **Notifications** - For threat alerts
- [ ] 🔋 **Battery Optimization OFF** - Keep services running

To grant Usage Access:
1. Settings → Apps → Special app access → Usage access
2. Find "Axiom" → Enable

### ✅ Step 3: Enroll Your Face in Axiom
- [ ] Open Axiom app
- [ ] Go to Settings → Face Enrollment
- [ ] Follow the 5-photo capture process:
  - Photo 1: Face forward
  - Photo 2: Turn slightly left
  - Photo 3: Turn slightly right
  - Photo 4: Look up slightly
  - Photo 5: Smile naturally
- [ ] Wait for "Face enrolled successfully" message
- [ ] Verify face templates are saved

**Tips for better enrollment:**
- Use good lighting (natural light is best)
- Remove glasses if possible
- Keep face centered in camera
- Don't move too quickly between captures

### ✅ Step 4: Configure Protection Settings
- [ ] Open Settings → Unauthorized Access Protection
- [ ] Enable "🤫 Silent Alert Mode" (optional - for silent monitoring)
- [ ] Enable "🔒 Voice Lock Protection"
- [ ] Review/edit voice codeword (default: "axiom unlock")
- [ ] Select apps to protect (Google Pay, WhatsApp, etc.)

### ✅ Step 5: Test the System
- [ ] Click "Test Lock Screen" button
- [ ] Verify emergency lock screen appears
- [ ] Test voice codeword unlock
- [ ] Try opening a protected app
- [ ] Verify QuickAuthActivity appears
- [ ] Test biometric authentication
- [ ] Test face recognition authentication

---

## How Authorization Works

### When You (Owner) Open a Protected App:

```
1. App detects protected app opened
2. Checks: "Was I authorized in last 5 minutes?"
   
   YES → ✅ Allow access (no interruption)
   
   NO → Show QuickAuthActivity:
        - Try fingerprint first (< 1 sec)
        - If fails → Try face recognition
        - If matches → ✅ Allow access for 5 minutes
        - If doesn't match → 🚨 Lock device
```

### When Someone Else Opens a Protected App:

```
1. App detects protected app opened
2. Not authorized (never authenticated or > 5 min)
3. QuickAuthActivity appears
4. Biometric fails (not their fingerprint)
5. Face recognition fails (not your face)
6. 🚨 DEVICE LOCKED with EmergencyLockActivity
7. Only your voice codeword can unlock
8. Failed attempts → Intruder photo captured
9. Alert sent to trusted contact
```

---

## Quick Configuration Options

### Security Level: HIGH (Recommended)
```
Authorization timeout: 2 minutes
Face similarity: 85%
Max attempts: 3
```

### Security Level: MEDIUM (Balanced)
```
Authorization timeout: 5 minutes (default)
Face similarity: 80%
Max attempts: 3
```

### Security Level: LOW (Convenient)
```
Authorization timeout: 10 minutes
Face similarity: 75%
Max attempts: 5
```

---

## Troubleshooting

### ❌ "No authentication method available"
**Problem:** Neither biometric nor face enrolled

**Fix:**
1. Enroll fingerprint in Android Settings, OR
2. Enroll face in Axiom app
3. Need at least ONE method

### ❌ "Face not recognized" (but it's you!)
**Problem:** Face template mismatch

**Fix:**
1. Go to Face Enrollment
2. Re-enroll in current lighting conditions
3. Ensure camera is clean
4. Take clear, steady photos

### ❌ QuickAuthActivity doesn't appear
**Problem:** Service not running or permissions missing

**Fix:**
1. Check Usage Access permission is granted
2. Restart the app
3. Enable protection in settings
4. Check logs: `adb logcat | grep UnauthorizedAccess`

### ❌ Authorization expires too quickly
**Fix:** In `UnauthorizedAccessProtectionService.kt`, increase:
```kotlin
val authValidDuration = 10 * 60 * 1000L // 10 minutes
```

### ❌ Biometric not working
**Problem:** Device biometric not set up

**Fix:**
1. Go to Android Settings → Security
2. Add fingerprint or enable face unlock
3. Test by locking/unlocking phone
4. Restart Axiom app

---

## Daily Usage

### First time opening app each day:
1. Open protected app (e.g., Google Pay)
2. QuickAuthActivity appears
3. Use fingerprint (fastest)
4. Access granted for 5 minutes
5. Open other protected apps freely

### After 5 minutes of inactivity:
- Need to authenticate again
- Quick fingerprint scan
- Back to full access

### If someone else tries to use your phone:
- Their fingerprint won't work
- Their face won't match
- Device locks immediately
- Only voice codeword unlocks

---

## Protected Apps (Default List)

Pre-configured protected apps:
- 💳 Google Pay
- 💰 PhonePe
- 💵 Paytm
- 💸 BHIM UPI
- 💬 WhatsApp
- 📧 Gmail
- ⚙️ Settings
- 📸 Instagram
- 👥 Facebook

**To add more apps:**
1. Go to Unauthorized Access Settings
2. Click "+ Add App"
3. Select app from list
4. Toggle protection ON

---

## Understanding the Logs

### Good logs (everything working):
```
✅ Face enrolled successfully!
✅ User authorized (verified 45s ago)
✅ Biometric authentication succeeded
✅ Face verified (confidence: 89%)
✅ Access granted
```

### Warning logs (need attention):
```
⚠️ Sensitive app detected: com.google.android.apps.nbu.paisa.user
⚠️ No recent authorization - verification required
⚠️ Face not recognized (similarity: 72%)
```

### Error logs (security threat):
```
🚨 LOCKING DEVICE - Unauthorized access
❌ Biometric authentication failed
❌ Face recognition failed
❌ Max attempts reached
🚨 Taking intruder photo
```

---

## Best Practices

### ✅ DO:
- Enroll face in good lighting
- Keep fingerprint sensor clean
- Set authorization timeout to 2-5 minutes
- Enable both biometric AND face recognition
- Test system regularly
- Update face templates if appearance changes (glasses, beard, etc.)

### ❌ DON'T:
- Share voice codeword with anyone
- Set timeout too long (> 10 min)
- Lower face similarity below 70%
- Disable battery optimization (services need to run)
- Skip testing after setup

---

## Security Tips

### Maximum Security:
1. Authorization timeout: **30 seconds**
2. Face similarity: **90%**
3. Enable Silent Alert Mode
4. Add trusted contact for alerts
5. Enable intruder photo capture
6. Protect Settings app

### Balanced Security (Recommended):
1. Authorization timeout: **5 minutes**
2. Face similarity: **85%**
3. Biometric + face recognition
4. Protect financial apps only

### Convenient Security:
1. Authorization timeout: **10 minutes**
2. Face similarity: **75%**
3. Biometric only
4. Protect critical apps only

---

## Getting Help

### Check Logs:
```bash
# All authorization logs
adb logcat | grep -E "QuickAuth|BiometricAuth|FaceRecognition|UnauthorizedAccess"

# Face recognition only
adb logcat | grep FaceRecognition

# Authorization checks only
adb logcat | grep UnauthorizedAccess
```

### Debug Mode:
Enable verbose logging by checking debug output in Android Studio's Logcat.

### Common Issues:
1. **App not working** → Check all permissions granted
2. **Face not recognized** → Re-enroll face
3. **Biometric fails** → Re-enroll fingerprint in Android Settings
4. **Service stops** → Disable battery optimization for Axiom

---

## Summary

✅ **You're ready when:**
- [ ] Fingerprint enrolled in Android Settings
- [ ] Face enrolled in Axiom app
- [ ] All permissions granted
- [ ] Protection enabled in settings
- [ ] Protected apps selected
- [ ] Test successful

🎯 **How it protects you:**
- Monitors protected apps 24/7
- Verifies your identity via biometric + face
- Locks device if unauthorized person detected
- Captures intruder photos
- Sends alerts to trusted contact
- Only voice codeword can unlock after detection

🔒 **Your phone is now secured!**
