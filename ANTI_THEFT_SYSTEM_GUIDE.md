# 🛡️ Axiom Anti-Theft System - Complete Guide

## Overview

The Axiom app has been transformed from a manual-access digital vault into a **proactive, always-on security system** with continuous background authentication, automatic lockdown, and voice-activated recovery.

---

## 🌟 New Features

### 1. **Continuous Background Authentication Service**
- **What it does**: Periodically verifies your face in the background (every 2-3 minutes)
- **How it works**: Uses the front camera to silently capture and verify your face against enrolled templates
- **Battery optimized**: Low-resolution captures with configurable intervals
- **Auto-start**: Starts on device boot if enabled

**Service**: `AxiomSecurityService`
- Runs as a foreground service
- Uses CameraX for background photo capture
- ML Kit face recognition for verification
- Updates global security state (`SecurityStateManager`)

### 2. **Unauthorized User Alert System**
- **What it does**: Automatically detects and responds to unauthorized access
- **Actions taken**:
  1. ✅ Captures high-quality photo of intruder
  2. 📍 Gets current GPS location
  3. 📱 Sends silent SMS to trusted contact with:
     - Alert message
     - Google Maps link with coordinates
     - Timestamp
  4. 🔒 Triggers lockdown mode after 2 failed verifications

**Service**: `AlertManager`
- Silent SMS sending via `SmsManager`
- Location tracking via `FusedLocationProviderClient`
- Automatic photo capture and storage

### 3. **Lockdown Mode & Sensitive App Blocking**
- **What it does**: Prevents unauthorized users from accessing protected apps
- **How it works**:
  1. User marks apps as "sensitive" (Banking, WhatsApp, Gallery, etc.)
  2. `AppMonitoringService` (AccessibilityService) monitors app launches
  3. If unauthorized user opens protected app → Instant lockdown
  4. Full-screen overlay blocks all interaction

**Service**: `AppMonitoringService`
- Monitors `TYPE_WINDOW_STATE_CHANGED` events
- Checks against protected app list from Room DB
- Launches `LockdownOverlayActivity` when triggered

**Activity**: `LockdownOverlayActivity`
- Full-screen, non-dismissible overlay
- Blocks back button, home button, status bar
- Runs over lock screen
- Only exits via voice command

### 4. **Voice-Activated Recovery**
- **What it does**: Allows only the owner to unlock during lockdown
- **How it works**:
  1. Owner sets secret code word in settings
  2. During lockdown, microphone continuously listens
  3. Android `SpeechRecognizer` processes audio
  4. Matches spoken text against secret code word
  5. Exact match → Unlocks device

**Features**:
- Continuous listening with auto-restart
- Case-insensitive matching
- Password-protected code word storage
- Visual feedback during listening

### 5. **Anti-Theft Settings UI**
- **What it does**: Centralized configuration for all security features
- **Features**:
  - ✅ Enable/disable background authentication
  - ⏱️ Configure check interval (1-10 minutes)
  - 📞 Set trusted phone number
  - 🔑 Set secret code word
  - 📱 Select sensitive apps
  - 🧪 Send test alerts
  - 📊 View required permissions

**Screen**: `AntiTheftSettingsScreen`

### 6. **Permissions Onboarding**
- **What it does**: Step-by-step permission setup
- **Permissions explained**:
  - 📷 Camera - Background face verification
  - 📍 Location - GPS tracking in alerts
  - 📱 SMS - Emergency alerts
  - 🎤 Microphone - Voice unlock
  - 🔓 Display over apps - Lockdown overlay
  - ♿ Accessibility - App monitoring

**Screen**: `PermissionsOnboardingScreen`

---

## 🏗️ Architecture

### Core Components

```
┌─────────────────────────────────────────────────────────────┐
│                    SecurityStateManager                      │
│  (Singleton - Global authorization state tracking)           │
│  - isAuthorized: Boolean                                     │
│  - consecutiveFailures: Int                                  │
│  - isLockdownMode: Boolean                                   │
└─────────────────────────────────────────────────────────────┘
                           ↑ ↓
    ┌──────────────────────┴──────────────────────┐
    │                                              │
┌───┴────────────────────┐        ┌───────────────┴──────────┐
│ AxiomSecurityService   │        │ AppMonitoringService     │
│ (Foreground Service)   │        │ (AccessibilityService)   │
│                        │        │                          │
│ • Background auth      │        │ • Monitors app launches  │
│ • Face verification    │        │ • Triggers lockdown      │
│ • Alert triggering     │        │ • Reads DB for protected │
└────────────────────────┘        │   apps                   │
           ↓                       └──────────────────────────┘
┌─────────────────────────┐                  ↓
│     AlertManager        │      ┌──────────────────────────┐
│                         │      │ LockdownOverlayActivity  │
│ • SMS alerts            │      │                          │
│ • GPS location          │      │ • Full-screen overlay    │
│ • Photo upload          │      │ • Voice recognition      │
└─────────────────────────┘      │ • Secret code matching   │
                                  └──────────────────────────┘
```

### Database Schema

```kotlin
// Anti-Theft Settings (Single row)
AntiTheftSettings {
    trustedPhoneNumber: String
    secretCodeWord: String
    isBackgroundAuthEnabled: Boolean
    authCheckIntervalMinutes: Int
    isSmsAlertEnabled: Boolean
    isVoiceRecoveryEnabled: Boolean
}

// Sensitive Apps (Multiple rows)
SensitiveApp {
    packageName: String (Primary Key)
    appName: String
    isProtected: Boolean
    addedTimestamp: Long
}
```

---

## 📋 Setup Instructions

### Step 1: Initial Configuration

1. **Launch the app**
2. Navigate to **Settings** → **🔒 Voice Codeword Lock**
3. Or directly to **Anti-Theft Protection** section

### Step 2: Configure Background Authentication

1. Toggle **"Enable Continuous Monitoring"**
2. Set **Check Interval** (recommended: 3 minutes)
   - Lower = More secure but more battery usage
   - Higher = Less battery but less frequent checks

### Step 3: Set Emergency Contact

1. Enter **Trusted Phone Number** (with country code)
   - Example: `+919876543210`
2. Toggle **"Send SMS Alerts"** ON
3. Click **"Send Test Alert"** to verify

### Step 4: Configure Voice Recovery

1. Toggle **"Enable Voice Unlock"** ON
2. Set **Secret Code Word/Phrase**
   - Choose something memorable but unique
   - Examples: "my secret phrase", "unlock now guardian"
   - ⚠️ Store this safely - it's your only recovery method!

### Step 5: Select Sensitive Apps

1. Click **"Manage Sensitive Apps"**
2. Check apps you want to protect:
   - Banking apps (Google Pay, PhonePe, Paytm)
   - Social media (WhatsApp, Instagram, Facebook)
   - Gallery/Photos
   - Email clients
3. Click **"Done"**

### Step 6: Grant Permissions

1. **Camera** - Allow when prompted
2. **Location** - Allow "All the time" for background tracking
3. **SMS** - Allow for emergency alerts
4. **Microphone** - Allow for voice unlock
5. **Display over other apps**:
   - Settings → Apps → Axiom → Display over other apps → Allow
6. **Accessibility Service**:
   - Settings → Accessibility → Axiom → Enable
   - ⚠️ Critical for app monitoring!

### Step 7: Save & Test

1. Click **"Save Settings"**
2. The security service will start automatically
3. Test the system (see Testing section below)

---

## 🧪 Testing the System

### Test 1: Background Authentication
```
1. Enable background authentication
2. Wait for the configured interval (e.g., 3 minutes)
3. Check notification: "Device secured - Owner verified"
4. Check logs: "✅ Background auth PASSED"
```

### Test 2: Unauthorized Access Detection
```
1. Temporarily enroll a different face or skip face enrollment
2. Background check will fail
3. You should receive:
   - SMS alert to trusted number
   - GPS location
   - Intruder photo captured
4. Check logs: "❌ UNAUTHORIZED USER DETECTED!"
```

### Test 3: Sensitive App Lockdown
```
1. Mark an app as sensitive (e.g., Calculator)
2. Set SecurityStateManager.markUnauthorized() programmatically for testing
3. Try to open the protected app
4. Lockdown overlay should appear immediately
```

### Test 4: Voice Recovery
```
1. Trigger lockdown mode
2. Speak your secret code word clearly
3. Device should unlock
4. Check logs: "✅ SECRET CODE WORD MATCHED!"
```

### Test 5: SMS Alert
```
1. Click "Send Test Alert" in settings
2. Trusted contact should receive SMS with:
   - Test message
   - Timestamp
```

---

## 🔧 Troubleshooting

### Background Authentication Not Working

**Problem**: Service not running
**Solution**:
```kotlin
// Check if service is running
adb shell dumpsys activity services | grep AxiomSecurityService

// Manually start service for testing
val intent = Intent(context, AxiomSecurityService::class.java)
intent.action = AxiomSecurityService.ACTION_START_SERVICE
context.startForegroundService(intent)
```

### App Monitoring Not Detecting Launches

**Problem**: AccessibilityService not enabled
**Solution**:
1. Go to Settings → Accessibility
2. Find "Axiom" in the list
3. Enable it
4. Grant all requested permissions

### Voice Recognition Not Working

**Problem**: Microphone permission denied
**Solution**:
1. Settings → Apps → Axiom → Permissions
2. Grant Microphone permission
3. Restart lockdown mode

### SMS Not Sending

**Problem**: Permission or SIM issues
**Solution**:
1. Check SMS permission granted
2. Verify device has active SIM
3. Test with shorter message
4. Check logs for error details

---

## 🔐 Security Considerations

### Data Storage
- Face templates: Encrypted in SharedPreferences
- Secret code word: Plain text in Room DB (consider encryption)
- Intruder photos: Stored in app's private directory

### Privacy
- Camera only activated during auth checks
- No data sent to external servers
- All processing done locally
- User can disable features anytime

### Battery Impact
- Estimated: 2-5% additional battery drain per day
- Configurable check intervals to optimize
- Service uses WAKE_LOCK sparingly

### Permissions Justification
| Permission | Why Required | When Used |
|------------|--------------|-----------|
| CAMERA | Face verification | Every auth check |
| LOCATION | GPS in alerts | On unauthorized access |
| SEND_SMS | Emergency alerts | On unauthorized access |
| RECORD_AUDIO | Voice unlock | During lockdown |
| SYSTEM_ALERT_WINDOW | Lockdown overlay | During lockdown |
| ACCESSIBILITY | App monitoring | Continuous |
| FOREGROUND_SERVICE | Background auth | Continuous |

---

## 📱 User Flow Example

### Scenario: Owner Using Phone Normally
```
1. ✅ Background check (every 3 min) → Face matches → Authorized
2. ✅ Owner opens WhatsApp → Authorized → Access granted
3. ✅ Background check → Face matches → Still authorized
4. ⏰ System runs 24/7 in background
```

### Scenario: Thief Steals Phone
```
1. ❌ Background check → Unknown face → Unauthorized
2. 📸 Capture intruder photo
3. 📍 Get GPS location
4. 📱 Send SMS: "ALERT: Unauthorized access at [GPS link]"
5. ⏱️ After 2 failures → Enter lockdown mode
6. 🔒 Thief tries to open WhatsApp (marked sensitive)
7. 🚫 Full-screen lockdown overlay appears
8. 🎤 Microphone starts listening
9. ❌ Thief cannot unlock (doesn't know code word)
10. ✅ Owner receives alert, speaks code word remotely, or uses Find My Device
```

---

## 🚀 Advanced Features (Future Enhancements)

- [ ] Image upload to cloud (Imgur API integration)
- [ ] Remote lockdown via SMS command
- [ ] Fingerprint as alternative to voice unlock
- [ ] Geofencing (auto-lock when outside safe zone)
- [ ] Stealth mode (hide app icon)
- [ ] Panic gesture (shake to lock)
- [ ] Admin remote wipe

---

## 📊 Logs & Debugging

### Key Log Tags
```
AxiomSecurity - Main service logs
SecurityState - Authorization state changes
AppMonitoring - Sensitive app detection
Lockdown - Lockdown mode events
AlertManager - SMS and location logs
```

### View Logs
```bash
# All Axiom logs
adb logcat | grep -E "Axiom|Security|Lockdown|Alert"

# Just auth checks
adb logcat | grep "Background auth"

# Service status
adb shell dumpsys activity services com.example.axiom
```

---

## ✅ Verification Checklist

Before deploying:
- [ ] Face enrollment completed
- [ ] Trusted contact saved and tested
- [ ] Secret code word set
- [ ] At least 3 sensitive apps marked
- [ ] All permissions granted
- [ ] Background auth enabled
- [ ] Test alert successful
- [ ] Accessibility service enabled
- [ ] Service survives app force-stop
- [ ] Service restarts on reboot

---

## 📞 Support

For issues or questions:
1. Check logs using LogCat
2. Verify all permissions granted
3. Test each feature individually
4. Review this guide thoroughly

**Remember**: This is a powerful security system. Test thoroughly before relying on it for actual security!
