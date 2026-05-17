# 🧪 Axiom Security - Testing Guide

This guide provides comprehensive testing procedures for all features of Axiom Security.

---

## 📋 Table of Contents

1. [Pre-Testing Setup](#pre-testing-setup)
2. [Unit Testing](#unit-testing)
3. [Feature Testing](#feature-testing)
4. [Integration Testing](#integration-testing)
5. [Security Testing](#security-testing)
6. [Performance Testing](#performance-testing)
7. [Test Cases](#test-cases)

---

## 🔧 Pre-Testing Setup

### Required Test Environment
- Android device or emulator (API 26+)
- Physical device recommended for biometric/camera testing
- Active internet connection (for WhatsApp testing)
- Test phone number for SMS alerts
- Sample phishing URLs and images

### Test Data Preparation

**1. Phishing URLs**
```
# Safe URLs (should score LOW)
https://www.google.com
https://github.com
https://www.amazon.com

# Suspicious URLs (should score MEDIUM-HIGH)
http://bit.ly/test123
http://192.168.1.1/login
http://g00gle.com/verify
http://paypal-secure@phishing.com

# Critical URLs (should score HIGH-CRITICAL)
http://verify-account.tk
http://192.168.1.100/banking.php
http://ámazon.com (unicode)
http://login@suspicious-site.ml
```

**2. Test Images**
- Image with phishing text ("Urgent! Verify your account")
- Image with URL
- Image with QR code
- Image with phone number
- Clean image (no threats)

**3. Test Contact**
- Your secondary phone number
- WhatsApp installed on secondary device

---

## 🧪 Unit Testing

### Phishing Detection Service Tests

**Test 1: URL Shortener Detection**
```kotlin
@Test
fun testUrlShortenerDetection() {
    val service = PhishingDetectionService(context)
    val result = service.analyzeLink("http://bit.ly/test")
    assertTrue(result.score >= 30)
    assertTrue(result.threats.any { it.contains("shortener") })
}
```

**Test 2: IP Address Detection**
```kotlin
@Test
fun testIPAddressDetection() {
    val service = PhishingDetectionService(context)
    val result = service.analyzeLink("http://192.168.1.1/login")
    assertTrue(result.score >= 40)
}
```

**Test 3: HTTP vs HTTPS**
```kotlin
@Test
fun testInsecureConnection() {
    val service = PhishingDetectionService(context)
    val result = service.analyzeLink("http://example.com")
    assertTrue(result.score >= 20)
}
```

### Image Analysis Tests

**Test 4: Keyword Detection**
```kotlin
@Test
fun testPhishingKeywordDetection() {
    // Test with image containing "urgent" and "verify"
    val service = ImageAnalysisService(context)
    val result = runBlocking { service.analyzeImage(testImageUri) }
    assertTrue(result.score >= 30)
}
```

**Test 5: URL in Image**
```kotlin
@Test
fun testUrlInImage() {
    // Test with image containing visible URL
    val service = ImageAnalysisService(context)
    val result = runBlocking { service.analyzeImage(imageWithUrl) }
    assertTrue(result.threats.any { it.contains("URL") })
}
```

---

## 🎯 Feature Testing

### 1. Biometric Authentication

**Test Case**: Successful Authentication
- **Steps**:
  1. Launch app
  2. Use correct fingerprint/face
  3. Verify access granted
- **Expected**: 
  - Voice: "Authentication successful"
  - Dashboard loads
  - No intruder alert

**Test Case**: Failed Authentication
- **Steps**:
  1. Launch app
  2. Use incorrect biometric (or cancel)
  3. Fail 3 times
- **Expected**:
  - Voice: "Unauthorized access detected"
  - Photo captured
  - Alert sent to trusted contact
  - Threat logged

### 2. Link Scanner

**Test Case**: Scan Safe Link
- **Input**: `https://www.google.com`
- **Expected**:
  - Score: 0-20
  - Result: "✅ Link Appears Safe"
  - Severity: LOW
  - No voice alert

**Test Case**: Scan Phishing Link
- **Input**: `http://verify-account.tk/login`
- **Expected**:
  - Score: 60+
  - Result: "⚠️ PHISHING DETECTED"
  - Severity: HIGH/CRITICAL
  - Voice: "Phishing link detected"
  - Logged in threat database

**Test Case**: Scan URL Shortener
- **Input**: `http://bit.ly/abcd123`
- **Expected**:
  - Score: 30+
  - Threat: "URL shortener detected"
  - Recommendation shown

### 3. Image Scanner

**Test Case**: Scan Clean Image
- **Input**: Photo of landscape
- **Expected**:
  - Score: 0-15
  - Result: "✅ Image Appears Safe"
  - No threats listed

**Test Case**: Scan Phishing Image
- **Input**: Screenshot with "URGENT: Verify your account now! Click: http://..."
- **Expected**:
  - Score: 50+
  - Detected keywords: "urgent", "verify"
  - Detected URL
  - Voice alert triggered
  - Logged as SUSPICIOUS_IMAGE

**Test Case**: Scan QR Code
- **Input**: Image with QR code
- **Expected**:
  - QR code detected
  - Warning: "Possible QR code detected - verify before scanning"
  - Score increased

### 4. Emergency Alert System

**Test Case**: Send WhatsApp Alert
- **Steps**:
  1. Set trusted contact
  2. Trigger unauthorized access (fail biometric)
  3. Check trusted contact's WhatsApp
- **Expected**:
  - Message received within 5 seconds
  - Contains alert text
  - Contains location link
  - Photo attached (if available)

**Test Case**: SMS Fallback
- **Steps**:
  1. Disable WhatsApp
  2. Trigger alert
  3. Check SMS
- **Expected**:
  - SMS received
  - Contains alert text and location

**Test Case**: Test Alert (No Photo)
- **Steps**:
  1. Go to Settings
  2. Tap "Test Emergency Alert"
- **Expected**:
  - Test message sent
  - No photo captured
  - Voice: "Test alert sent successfully"

### 5. Voice Alerts

**Test Case**: Phishing Detection Voice
- **Trigger**: Scan phishing link
- **Expected**: "Critical threat detected! Phishing link detected"

**Test Case**: Read Logs Aloud
- **Steps**:
  1. Have 3+ threats in log
  2. Tap floating speaker button
- **Expected**: 
  - "You have X threat logs"
  - Reads first 5 logs
  - Clear audio

**Test Case**: Unauthorized Access Voice
- **Trigger**: Failed biometric
- **Expected**: "Unauthorized access detected, alert sent to family"

### 6. Financial Fraud Monitoring

**Prerequisites**: Notification access enabled

**Test Case**: Large Transaction Detection
- **Steps**:
  1. Enable notification access
  2. Create test notification: "₹15,000 debited from your account"
- **Expected**:
  - Detected as suspicious
  - Voice alert
  - Logged with FINANCIAL_FRAUD type

**Test Case**: Unusual Time Detection
- **Steps**:
  1. Create notification at 2 AM
  2. Text: "₹5,000 paid"
- **Expected**:
  - Score increased for time
  - Flagged if total score ≥30

**Test Case**: Normal Transaction (No Alert)
- **Steps**:
  1. Create notification: "₹100 received"
  2. Time: 2 PM
- **Expected**:
  - Low score
  - Not flagged
  - No alert

### 7. Threat Logging

**Test Case**: Log Creation
- **Steps**:
  1. Scan phishing link
  2. Check dashboard
- **Expected**:
  - Log appears in "Recent Threats"
  - Correct threat type
  - Timestamp accurate
  - Unread badge shows

**Test Case**: Mark as Read
- **Steps**:
  1. Have unread log
  2. Tap checkmark
- **Expected**:
  - Badge count decreases
  - Log marked as read
  - Checkmark disappears

**Test Case**: Clear All Logs
- **Steps**:
  1. Settings → Clear All Logs
  2. Confirm
- **Expected**:
  - All logs deleted
  - Dashboard shows "No threats detected"
  - Toast: "All logs cleared"

---

## 🔗 Integration Testing

### End-to-End Test: Complete Threat Detection Flow

**Scenario**: User receives suspicious WhatsApp message

1. **Receive Message**: "URGENT! Your bank account suspended. Verify: http://bit.ly/verify123"

2. **Open Axiom**: Launch app with biometric

3. **Scan Link**:
   - Copy link
   - Tap "Scan Link"
   - Paste URL
   - Scan
   - **Verify**: Phishing detected, voice alert

4. **Check Dashboard**:
   - **Verify**: New threat log
   - Unread count: 1
   - Severity: HIGH

5. **Read Aloud**:
   - Tap speaker button
   - **Verify**: Threat read via TTS

6. **Mark Read**:
   - Tap checkmark
   - **Verify**: Unread count: 0

### End-to-End Test: Unauthorized Access + Alert

**Scenario**: Someone tries to access your phone

1. **Setup**: Set trusted contact: +91XXXXXXXXXX

2. **Trigger**: Launch app, fail biometric 3 times

3. **Verify Sequence**:
   - ✅ Voice: "Unauthorized access detected"
   - ✅ Camera activates (silent)
   - ✅ Photo saved to /intruders/
   - ✅ Location obtained
   - ✅ WhatsApp message sent
   - ✅ Threat logged as CRITICAL
   - ✅ Dashboard shows new alert

4. **Trusted Contact Receives**:
   - ✅ WhatsApp notification
   - ✅ Message text with alert
   - ✅ Photo attachment
   - ✅ Google Maps link

---

## 🔒 Security Testing

### Permission Testing

**Test**: Verify minimum permissions
- Run app with all permissions denied
- **Expected**: Graceful handling, no crashes

**Test**: Essential permissions
- Deny camera → Intruder photo fails gracefully
- Deny location → Alert sent without location
- Deny SMS → WhatsApp only (no crash)

### Data Privacy

**Test**: Local storage only
- Use network monitor
- Scan links and images
- **Verify**: No external API calls for detection

**Test**: Photo storage
- Trigger intruder detection
- Check: `/data/data/com.example.axiom/files/intruders/`
- **Verify**: Photos stored locally only

### Authentication Security

**Test**: Biometric bypass attempt
- Try accessing without authentication
- **Expected**: Blocked, intruder alert triggered

---

## ⚡ Performance Testing

### Response Time Tests

**Phishing Detection**
- **Target**: < 1 second
- **Test**: Scan 10 URLs, measure avg time
- **Acceptance**: < 1.5 seconds per scan

**Image Analysis**
- **Target**: < 5 seconds
- **Test**: Scan 5 images with ML Kit
- **Acceptance**: < 7 seconds per scan

**Voice Alert**
- **Target**: Immediate (< 0.5s)
- **Test**: Trigger threat, measure time to voice
- **Acceptance**: < 1 second

**Emergency Alert**
- **Target**: < 10 seconds total
- **Test**: Trigger alert, measure to WhatsApp delivery
- **Acceptance**: < 15 seconds

### Memory Usage

**Test**: Monitor during image scanning
- **Acceptance**: < 100MB increase
- **Verify**: No memory leaks after 10 scans

### Battery Impact

**Test**: Run for 1 hour with active monitoring
- **Acceptance**: < 5% battery drain

---

## 📝 Test Cases Summary

### Critical Test Cases (Must Pass)

| ID | Feature | Test Case | Priority |
|----|---------|-----------|----------|
| TC001 | Auth | Biometric authentication success | P0 |
| TC002 | Auth | Failed auth triggers intruder alert | P0 |
| TC003 | Link | Phishing link detection (score 60+) | P0 |
| TC004 | Alert | Emergency WhatsApp alert sent | P0 |
| TC005 | Alert | SMS fallback when WhatsApp fails | P0 |
| TC006 | Voice | TTS announces critical threats | P0 |
| TC007 | Image | Suspicious image detection | P1 |
| TC008 | Fraud | Large transaction detection | P1 |
| TC009 | Logs | Threat logging and retrieval | P1 |
| TC010 | Settings | Trusted contact save/retrieve | P1 |

### Regression Test Cases (After Updates)

- All P0 critical tests
- Photo capture functionality
- Database operations
- Voice alert in all scenarios
- Location services
- WhatsApp intent handling

---

## 🐛 Known Issues & Workarounds

### Issue 1: WhatsApp Not Opening
**Cause**: WhatsApp not installed
**Workaround**: SMS automatically sent instead

### Issue 2: Camera Permission Loop
**Cause**: Android 13+ photo picker
**Solution**: Grant camera permission manually in settings

### Issue 3: TTS Not Available
**Cause**: No TTS engine on device
**Solution**: Download Google TTS from Play Store

---

## 📊 Test Results Template

```
Test Run Date: __________
Tester: __________
Device: __________
Android Version: __________

| Test ID | Result | Notes |
|---------|--------|-------|
| TC001   | ✅ Pass |       |
| TC002   | ✅ Pass |       |
| TC003   | ❌ Fail | Score was 55, expected 60+ |
| ...     | ...    | ...   |

Overall Pass Rate: ___%
Critical Issues: ___
```

---

## 🚀 Automated Testing

### Setup (Future)

```bash
# Run unit tests
./gradlew test

# Run instrumentation tests
./gradlew connectedAndroidTest

# Generate coverage report
./gradlew jacocoTestReport
```

### CI/CD Integration (Future)

```yaml
# .github/workflows/android.yml
name: Android CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Run tests
        run: ./gradlew test
```

---

## ✅ Testing Checklist

Before Release:
- [ ] All P0 tests pass
- [ ] No crashes in 1-hour stress test
- [ ] Emergency alerts work on 3+ devices
- [ ] Voice alerts clear and audible
- [ ] All permissions handled gracefully
- [ ] Privacy audit complete
- [ ] Performance benchmarks met
- [ ] User guide tested with non-technical user

---

**Happy Testing! 🧪**

*Last Updated: October 2025*
