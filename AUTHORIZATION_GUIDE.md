# Authorization & Person Detection Guide

## Overview
Axiom uses a **two-layer security system** to detect and verify authorized users:

### 🔐 Layer 1: Biometric Authentication (Fast)
- Fingerprint sensor
- Device face unlock
- Pattern/PIN fallback
- **Speed**: < 1 second
- **Purpose**: Quick verification for authorized users

### 🎯 Layer 2: Face Recognition (Accurate)
- ML Kit face detection
- Custom face template matching
- Multi-angle verification
- **Accuracy**: 85%+ similarity threshold
- **Purpose**: Backup verification and intruder detection

---

## How Authorization Works

### 1️⃣ Initial Setup
Users must enroll their face before protection works:

```kotlin
// Navigate to Face Enrollment Screen
FaceEnrollmentScreen(
    onEnrollmentComplete = { /* Face enrolled! */ }
)
```

**Steps:**
1. User takes 5 photos from different angles
2. System extracts face templates (128-dimensional vectors)
3. Templates stored securely in SharedPreferences
4. Face recognition now active

### 2️⃣ Real-Time Monitoring
`UnauthorizedAccessProtectionService` monitors app usage:

```kotlin
// Monitors foreground apps every 1 second
private fun checkForUnauthorizedAccess() {
    // 1. Detect protected app opened
    // 2. Check if user recently authorized (< 5 min)
    // 3. If not → Launch QuickAuthActivity
}
```

### 3️⃣ Authorization Flow

When a protected app is opened:

```
Protected App Opened
        ↓
Is user authorized? (within last 5 min)
        ↓
    NO → Launch QuickAuthActivity
        ↓
    Try Biometric Auth First
        ↓
    ✅ Success → Grant Access
        ↓
    ❌ Fail → Try Face Recognition
        ↓
    Take front camera photo
        ↓
    Compare with enrolled templates
        ↓
    Match ≥ 85% → ✅ Grant Access
        ↓
    Match < 85% → ❌ Lock Device
        ↓
    Launch EmergencyLockActivity
```

---

## Key Components

### BiometricAuthManager
Handles device biometric authentication:

```kotlin
val biometricAuthManager = BiometricAuthManager(context)

biometricAuthManager.authenticate(
    activity = this,
    onSuccess = { 
        // User verified via fingerprint/face
        grantAccess()
    },
    onError = { error ->
        // Biometric failed, try face recognition
    }
)
```

### FaceRecognitionService
ML-based face verification:

```kotlin
val faceService = FaceRecognitionService(context)

// Check if face enrolled
if (faceService.isFaceEnrolled()) {
    // Verify current user
    val result = faceService.verifyFace(bitmap)
    
    if (result.isMatch) {
        // Authorized person (85%+ match)
        grantAccess()
    } else {
        // Unauthorized person
        lockDevice()
    }
}
```

### QuickAuthActivity
Combines both authentication methods:

**Features:**
- Automatically tries biometric first (fastest)
- Falls back to face recognition if biometric fails
- Shows live camera preview
- Limits to 3 failed attempts
- Locks device after max attempts

---

## Configuration

### Adjust Authorization Timeout

In `UnauthorizedAccessProtectionService.kt`:

```kotlin
// Change how long authorization lasts (default 5 minutes)
val authValidDuration = 5 * 60 * 1000L  // milliseconds

// Make it 10 minutes:
val authValidDuration = 10 * 60 * 1000L

// Make it 30 seconds (more secure):
val authValidDuration = 30 * 1000L
```

### Adjust Face Recognition Sensitivity

In `FaceRecognitionService.kt`:

```kotlin
// Change similarity threshold (default 85%)
private const val FACE_SIMILARITY_THRESHOLD = 0.85f

// More strict (fewer false positives, might reject owner):
private const val FACE_SIMILARITY_THRESHOLD = 0.90f

// More lenient (fewer false negatives, might accept intruder):
private const val FACE_SIMILARITY_THRESHOLD = 0.75f
```

### Add Protected Apps

In `UnauthorizedAccessSettings.kt`:

```kotlin
fun getDefaultProtectedApps(): List<ProtectedApp> {
    return listOf(
        ProtectedApp("Google Pay", "com.google.android.apps.nbu.paisa.user", "💳", "Payment"),
        ProtectedApp("WhatsApp", "com.whatsapp", "💬", "Messaging"),
        // Add your app:
        ProtectedApp("MyApp", "com.example.myapp", "🔒", "Custom")
    )
}
```

---

## Testing Authorization

### Test Face Enrollment
1. Go to Settings → Face Enrollment
2. Take 5 photos from different angles
3. Verify "Face enrolled successfully" message

### Test Biometric Auth
```kotlin
// From any screen:
val biometricAuth = BiometricAuthManager(context)

if (biometricAuth.isBiometricAvailable()) {
    println("✅ Biometric available")
} else {
    println("❌ No biometric hardware or not enrolled")
}
```

### Test Protected App Access
1. Enable protection in UnauthorizedAccessSettings
2. Add an app to protected list
3. Open the app
4. Should show QuickAuthActivity
5. Verify with fingerprint or face
6. Should grant access for 5 minutes

### Test Lock Screen
```kotlin
// Trigger test lock screen
testLockScreen(context)
```

---

## Troubleshooting

### "No authentication method available"
**Cause:** Neither biometric nor face enrolled

**Solution:**
1. Enroll fingerprint in device settings
2. OR enroll face in Axiom app
3. Need at least one method

### "Face not recognized"
**Cause:** Face templates don't match (< 85% similarity)

**Solutions:**
1. Re-enroll face with better lighting
2. Take photos from more angles
3. Lower similarity threshold (less secure)
4. Ensure front camera is clean

### "Biometric authentication failed"
**Cause:** Fingerprint not recognized by device

**Solutions:**
1. Clean fingerprint sensor
2. Re-enroll fingerprint in device settings
3. Use face recognition as backup

### Authorization expires too quickly
**Solution:** Increase `authValidDuration` in service

### Authorization lasts too long (security risk)
**Solution:** Decrease `authValidDuration` to 1-2 minutes

---

## Security Best Practices

### ✅ Recommended Settings
- Authorization timeout: **2-5 minutes**
- Face similarity threshold: **0.85** (85%)
- Max failed attempts: **3**
- Enroll face in good lighting
- Take photos from 5+ angles

### ⚠️ Important Notes
1. **Face enrollment is critical** - Without it, only biometric works
2. **Lighting matters** - Enroll and verify in similar lighting conditions
3. **Camera quality** - Better camera = better recognition
4. **Privacy** - Face templates stored locally, never sent to server
5. **Fallback** - Always have biometric as backup method

### 🔒 Hardening Security
For maximum security:
- Set timeout to 30 seconds
- Increase threshold to 0.90
- Require both biometric AND face recognition
- Enable intruder photo capture
- Send alerts to trusted contact

---

## API Reference

### BiometricAuthManager

```kotlin
// Check availability
fun isBiometricAvailable(): Boolean

// Check enrollment
fun isBiometricEnrolled(): Boolean

// Authenticate
fun authenticate(
    activity: FragmentActivity,
    title: String = "Verify Identity",
    subtitle: String = "Authenticate to access",
    onSuccess: () -> Unit,
    onError: (String) -> Unit,
    onFailed: () -> Unit
)
```

### FaceRecognitionService

```kotlin
// Check enrollment
fun isFaceEnrolled(): Boolean

// Enroll face
suspend fun enrollOwnerFace(photos: List<Bitmap>): EnrollmentResult

// Verify face
suspend fun verifyFace(photo: Bitmap): FaceVerificationResult

// Quick check
suspend fun quickFaceCheck(photo: Bitmap): Boolean

// Delete enrolled face
fun deleteEnrolledFace()
```

### UnauthorizedAccessProtectionService

```kotlin
// Start service
val intent = Intent(context, UnauthorizedAccessProtectionService::class.java)
context.startService(intent)

// Stop service
context.stopService(intent)

// Check if running
if (UnauthorizedAccessProtectionService.isServiceRunning) {
    // Service active
}
```

---

## Flow Diagram

```
┌─────────────────────────────────────────┐
│  User opens protected app (e.g., GPay) │
└────────────────┬────────────────────────┘
                 │
                 ▼
    ┌────────────────────────┐
    │ Is user authorized?    │
    │ (last auth < 5 min)    │
    └────────┬───────────────┘
             │
      ┌──────┴──────┐
      │ YES         │ NO
      │             │
      ▼             ▼
  ┌────────┐   ┌──────────────────┐
  │ ALLOW  │   │ QuickAuthActivity│
  └────────┘   └─────────┬────────┘
                         │
                         ▼
              ┌────────────────────┐
              │ Try Biometric Auth │
              └─────────┬──────────┘
                        │
                 ┌──────┴──────┐
                 │ SUCCESS     │ FAIL
                 │             │
                 ▼             ▼
            ┌────────┐   ┌─────────────────┐
            │ ALLOW  │   │ Try Face Recog  │
            └────────┘   └────────┬────────┘
                                  │
                          ┌───────┴────────┐
                          │ MATCH ≥85%     │ < 85%
                          │                │
                          ▼                ▼
                     ┌────────┐    ┌──────────────┐
                     │ ALLOW  │    │ LOCK DEVICE  │
                     └────────┘    │ Emergency    │
                                   │ Lock Screen  │
                                   └──────────────┘
```

---

## Next Steps

1. ✅ **Enroll your face** - Go to Settings → Face Enrollment
2. ✅ **Set up biometric** - Add fingerprint in device settings  
3. ✅ **Enable protection** - Turn on Unauthorized Access Protection
4. ✅ **Add protected apps** - Select apps to protect
5. ✅ **Test the system** - Try opening a protected app
6. ✅ **Configure timeout** - Adjust based on your security needs

---

**Need help?** Check the logs:
```bash
adb logcat | grep -E "BiometricAuth|FaceRecognition|UnauthorizedAccess|QuickAuth"
```
