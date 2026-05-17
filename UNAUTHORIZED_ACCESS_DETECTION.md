# 🚨 Unauthorized Access Detection System

## Overview
The Axiom app now includes a **comprehensive unauthorized user detection system** that automatically identifies, captures, and alerts when someone other than the authorized owner tries to access protected apps.

## ✅ What Was Fixed
Previously, the app had authentication options for authorized users but **lacked detection and alerting** when unauthorized users failed authentication. This has been completely resolved.

---

## 🎯 System Components

### 1. **QuickAuthActivity** (Primary Detection Point)
**File:** `QuickAuthActivity.kt`

**Functionality:**
- Shows authentication screen when protected apps are accessed
- Uses **biometric** (fingerprint/face unlock) + **face recognition** for verification
- **NEW:** When authentication fails, automatically:
  - 📸 Captures 4 intruder photos
  - 📍 Gets GPS location
  - 📤 Sends emergency alert to trusted contact via WhatsApp
  - 🔒 Locks the device
  - 📝 Logs unauthorized access attempt

**Key Methods:**
```kotlin
lockDevice(appName: String)
  └─> detectAndAlertIntruder(appName)
       ├─> Captures 4 photos silently
       ├─> Gets GPS location
       ├─> Sends WhatsApp alert with photos
       └─> Logs the attempt
```

---

### 2. **SilentIntruderMonitorService** (Background Detection)
**File:** `SilentIntruderMonitorService.kt`

**Functionality:**
- Monitors device unlock events silently in background
- **Face Recognition Mode:** Automatically captures photo on unlock and verifies face
- If face doesn't match owner → Sends silent alert WITHOUT locking screen
- Captures 4-5 photos and sends to trusted contact
- Intruder remains unaware (no visible warnings)

**Detection Methods:**
- **Face Recognition:** Compares unlocked user's face with enrolled owner face
- **Time-based:** Checks if recent authentication within 10 minutes

---

### 3. **UnauthorizedAccessProtectionService** (App Monitoring)
**File:** `UnauthorizedAccessProtectionService.kt`

**Functionality:**
- Monitors when protected apps (banking, payment apps, etc.) are opened
- Checks if user is authorized (recent authentication)
- Launches **QuickAuthActivity** if no recent authorization
- Tracks failed authentication attempts
- After 3 failed attempts → Takes intruder photo + sends alert

---

### 4. **EmergencyLockActivity** (Lock Screen)
**File:** `EmergencyLockActivity.kt`

**Enhancements:**
- **NEW:** Displays "⚠️ INTRUDER DETECTED ⚠️" when unauthorized access occurred
- Shows "Photos sent to trusted contact" message
- Requires **voice codeword** to unlock
- After 3 failed unlock attempts → More intruder photos captured

---

## 🔄 Complete Detection Flow

### **Scenario 1: Protected App Access**
```
1. User opens protected app (e.g., Google Pay)
   ↓
2. UnauthorizedAccessProtectionService detects this
   ↓
3. Checks: Is user recently authenticated? (within 5 min)
   ↓
4a. YES → Allow access ✅
4b. NO → Launch QuickAuthActivity 🔐
   ↓
5. QuickAuthActivity shows biometric/face recognition
   ↓
6a. SUCCESS → Grant access ✅
6b. FAIL → Unauthorized user detected! 🚨
   ↓
7. Capture 4 photos + GPS location
   ↓
8. Send alert to trusted contact
   ↓
9. Lock device (EmergencyLockActivity)
```

### **Scenario 2: Device Unlock (Silent Mode)**
```
1. Device unlocked (screen on + user present)
   ↓
2. SilentIntruderMonitorService detects unlock
   ↓
3. Wait 2 seconds for face to be visible
   ↓
4. Capture photo silently (front camera)
   ↓
5. Verify face using enrolled face data
   ↓
6a. MATCH → Owner detected ✅ (no action)
6b. NO MATCH → Unauthorized person! 🚨
   ↓
7. Silently capture 4-5 photos (300ms intervals)
   ↓
8. Get GPS location
   ↓
9. Send WhatsApp alert to trusted contact
   ↓
10. Intruder remains UNAWARE (no lock screen)
```

---

## 📱 Alert Messages

### Unauthorized App Access Alert
```
🚨 *UNAUTHORIZED ACCESS DETECTED!*

Someone tried to access: *Google Pay*

⚠️ *Authentication Failed*
• Time: 05/10/2025 00:19:47
• Photos captured: 4
📍 Location: https://maps.google.com/?q=28.6139,77.2090

📱 Device has been locked!

- Axiom Security
```

### Silent Unlock Alert
```
🚨 *ALERT: UNAUTHORIZED ACCESS*

Someone has accessed your phone!

*Reason:* Unauthorized person detected (Face not recognized)
*Time:* 05/10/2025 00:19:47
*Photos:* 5 captured

📍 GPS: https://maps.google.com/?q=28.6139,77.2090

⚠️ *SILENT ALERT* - Person is unaware

- Axiom Security
```

---

## 🎮 Settings & Configuration

### Enable/Disable Features
**Location:** Settings → Unauthorized Access Protection

1. **🤫 Silent Alert Mode** (SilentIntruderMonitorService)
   - Silently monitors unlocks and sends alerts
   - No visible warnings to intruder
   - Recommended: **ENABLED** (first line of defense)

2. **🔒 Voice Lock Protection** (UnauthorizedAccessProtectionService)
   - Locks device when protected apps accessed
   - Requires voice codeword to unlock
   - Recommended: **ENABLED** (second line of defense)

3. **👤 Face Enrollment**
   - Required for face recognition verification
   - Takes 5 photos from different angles
   - Recommended: **COMPLETE ENROLLMENT**

4. **🎤 Voice Codeword**
   - Custom phrase to unlock emergency lock screen
   - Default: "axiom unlock"
   - Can be changed to any 2-4 word phrase

### Protected Apps List
Default protected apps:
- 💳 Google Pay, PhonePe, Paytm, BHIM UPI
- 📱 Banking apps (ICICI, SBI, HDFC, Axis)
- 💬 WhatsApp
- 📧 Gmail
- ⚙️ Settings
- 📸 Instagram, Facebook

**You can add/remove apps from the list.**

---

## 🔧 Technical Details

### Photo Capture
- **Method:** Silent front camera capture
- **Count:** 4-5 photos per incident
- **Delay:** 300-500ms between captures
- **Storage:** Internal app files (`/intruders/` folder)
- **Format:** JPEG with timestamp filename

### Location Tracking
- **Method:** GPS + Network location
- **Format:** Google Maps link
- **Fallback:** "Location unavailable" if no permission

### Alert Delivery
- **Primary:** WhatsApp direct message
- **Fallback:** SMS if WhatsApp unavailable
- **Attachments:** Photos sent as separate messages
- **Delay:** 1-2 seconds between messages

### Logging
All unauthorized access attempts are logged to:
- **LogCat:** `THREAT_LOG` tag
- **SharedPreferences:** Tracks attempt count and timestamps

---

## 🧪 Testing

### Test Lock Screen
**Settings → Unauthorized Access Protection → "Test Lock Screen" button**

This simulates an unauthorized access attempt without triggering alerts.

### Test Face Recognition
**Settings → Face Enrollment → Re-enroll face**

Ensures face recognition is working correctly.

---

## 🔐 Security Features

### Protection Layers
1. **Layer 1:** Silent monitoring (intruder unaware)
2. **Layer 2:** App-level authentication
3. **Layer 3:** Emergency lock screen
4. **Layer 4:** Remote alert to owner

### Privacy
- Photos stored locally (not uploaded to cloud)
- Only sent to **trusted contact**
- Can be deleted anytime
- No third-party access

### Anti-Tampering
- Back button disabled on lock screen
- Requires voice codeword to unlock
- Service runs in background (survives app closure)
- Auto-restarts on device boot

---

## 📊 Tracking Stats

### Stored Data
- `unauthorized_attempts`: Total failed authentication attempts
- `last_unauthorized_attempt`: Timestamp of last attempt
- `last_auth_time`: Last successful authentication time

### View Stats
Check logcat for detailed logs:
```bash
adb logcat | grep -E "QuickAuth|SilentMonitor|UnauthorizedAccess|THREAT_LOG"
```

---

## 🚀 Future Enhancements

Potential improvements:
- [ ] Cloud backup of intruder photos
- [ ] Multiple trusted contacts
- [ ] Real-time location tracking
- [ ] Video recording instead of photos
- [ ] AI-based behavior analysis
- [ ] Remote device wipe capability

---

## 🐛 Troubleshooting

### Alerts Not Sending
- ✅ Check if trusted contact is set (Settings)
- ✅ Verify WhatsApp is installed
- ✅ Grant notification and SMS permissions
- ✅ Test with "Test Lock Screen" button

### Face Recognition Not Working
- ✅ Re-enroll your face
- ✅ Ensure good lighting during enrollment
- ✅ Grant camera permission
- ✅ Check if face data is enrolled (Settings)

### Service Not Running
- ✅ Enable "Silent Alert Mode" in settings
- ✅ Grant battery optimization exemption
- ✅ Restart the app or device
- ✅ Check logcat for service status

---

## 📝 Summary

The unauthorized access detection system is now **fully operational** with:
- ✅ Automatic intruder detection
- ✅ Photo capture (4-5 photos per incident)
- ✅ GPS location tracking
- ✅ WhatsApp/SMS alerts to trusted contact
- ✅ Device locking
- ✅ Comprehensive logging
- ✅ Silent mode (intruder unaware)
- ✅ Voice codeword unlock

**The system works automatically in the background. No manual intervention needed.**

---

*Last Updated: 2025-10-05*
*Version: 2.0*
