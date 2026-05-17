# 📖 Axiom Security - User Guide

Welcome to **Axiom Security**, your comprehensive cybersecurity companion for Android. This guide will help you set up and use all the features effectively.

---

## 📋 Table of Contents

1. [First-Time Setup](#first-time-setup)
2. [Dashboard Overview](#dashboard-overview)
3. [Scanning Links](#scanning-links)
4. [Scanning Images](#scanning-images)
5. [Settings & Configuration](#settings--configuration)
6. [Understanding Threat Logs](#understanding-threat-logs)
7. [Voice Alerts](#voice-alerts)
8. [Emergency Alerts](#emergency-alerts)
9. [Financial Fraud Monitoring](#financial-fraud-monitoring)
10. [Troubleshooting](#troubleshooting)
11. [Best Practices](#best-practices)

---

## 🚀 First-Time Setup

### Step 1: Install the App
1. Download and install Axiom Security from the Play Store or build from source
2. Open the app for the first time

### Step 2: Grant Permissions
The app will request several permissions. Here's why each is needed:

- **📷 Camera**: Captures intruder photos on unauthorized access
- **📍 Location**: Sends GPS coordinates in emergency alerts
- **📱 SMS**: Sends emergency SMS when WhatsApp is unavailable
- **🔔 Notifications**: Displays threat alerts
- **🎤 Microphone**: For voice passphrase authentication (future feature)

**Important**: Grant all permissions for full functionality.

### Step 3: Set Up Biometric Authentication
1. The app will prompt for biometric authentication
2. Use your fingerprint or face unlock
3. If unavailable, use your device PIN/pattern

### Step 4: Configure Trusted Contact
1. Navigate to **Settings** (gear icon in top-right)
2. Enter your trusted contact's phone number
3. Include country code (e.g., +91XXXXXXXXXX for India)
4. Tap **Save Contact**
5. Test the alert system using **Test Emergency Alert**

### Step 5: Enable Notification Access (Optional but Recommended)
For financial fraud monitoring:
1. Go to device **Settings** → **Apps** → **Special App Access**
2. Select **Notification Access**
3. Enable **Axiom Security**
4. Return to the app

---

## 🏠 Dashboard Overview

The main dashboard displays:

### Status Card
- **System Secure** (Green): No active threats
- **Active Threats** (Red): Unresolved threats detected
- Shows unread vs. total threat count

### Quick Actions
- **Scan Link**: Analyze suspicious URLs
- **Scan Image**: Check images for threats

### Recent Threats
- Color-coded by severity:
  - 🔴 **Red**: Critical
  - 🟠 **Orange**: High
  - 🟡 **Yellow**: Medium
  - 🟢 **Green**: Low
- Shows threat type, description, and timestamp
- Tap checkmark to mark as read

### Floating Button
- **🔊 Speaker Icon**: Tap to hear all logs read aloud

---

## 🔗 Scanning Links

### How to Scan a Link

1. **Copy the suspicious link** from SMS, email, or messaging app
2. Open Axiom Security
3. Tap **Scan Link** on dashboard
4. Paste the link in the text field
5. Tap **Scan Link** button
6. Wait for analysis (1-2 seconds)

### Understanding Results

#### ✅ Safe Link
- Green card with checkmark
- Low threat score (<40)
- Recommendation: "Exercise caution before clicking"

#### ⚠️ Phishing Detected
- Red/Orange card with warning icon
- High threat score (60+)
- Shows detected threats:
  - URL shortener detected
  - Suspicious domain extension
  - IP address instead of domain
  - Insecure HTTP connection
  - Suspicious characters
- **Voice Alert**: "Phishing link detected"
- **Recommendation**: "DO NOT CLICK"

### What Happens After Detection
- Threat is automatically logged
- Voice alert announces the threat
- Shows detailed analysis
- Link is marked in threat log

---

## 🖼️ Scanning Images

### How to Scan an Image

1. Open Axiom Security
2. Tap **Scan Image** on dashboard
3. Tap **Select Image to Scan**
4. Choose image from gallery
5. Wait for ML analysis (2-5 seconds)

### What Gets Detected

- **Phishing Keywords**: "urgent", "verify", "suspended", "confirm", "claim prize"
- **URLs in Images**: Links embedded in screenshots
- **Phone Numbers**: Contact numbers in images
- **QR Codes**: Visual detection of QR patterns
- **OCR Text Extraction**: All readable text

### Example Use Cases

✅ **Scan WhatsApp forwards** before believing them
✅ **Check email screenshots** for legitimacy
✅ **Verify QR codes** before scanning
✅ **Analyze promotional images** for scams

---

## ⚙️ Settings & Configuration

### Emergency Contact Section

**Purpose**: This contact receives alerts when unauthorized access is detected

1. Enter phone number with country code
2. Tap **Save Contact**
3. Use **Test Emergency Alert** to verify

**What They Receive**:
- Alert message via WhatsApp (or SMS)
- Photo of intruder (if captured)
- GPS location of device
- Timestamp

### Security Features Status

All features are enabled by default:
- ✅ Real-time Phishing Detection
- ✅ Unauthorized Access Protection
- ✅ Financial Fraud Monitoring
- ✅ Voice Alerts

### Actions

**Test Emergency Alert**
- Sends a test alert to your trusted contact
- Verifies WhatsApp/SMS functionality
- Does NOT capture photo or trigger intruder protocol

**Clear All Logs**
- Permanently deletes all threat logs
- Cannot be undone
- Use to clear old/resolved threats

---

## 📊 Understanding Threat Logs

### Threat Types

1. **🔗 PHISHING_LINK**
   - Detected malicious URL
   - Common in SMS/email scams

2. **🖼️ SUSPICIOUS_IMAGE**
   - Image contains phishing content
   - QR code or malicious text detected

3. **💳 FINANCIAL_FRAUD**
   - Suspicious transaction detected
   - Large or unusual time payment

4. **🔒 UNAUTHORIZED_ACCESS**
   - Failed authentication attempt
   - Intruder detection triggered

5. **📱 MALICIOUS_APP**
   - Suspicious app behavior (future)

6. **🔐 SUSPICIOUS_PERMISSION**
   - App requesting dangerous permissions (future)

### Severity Levels

- **🔴 CRITICAL**: Immediate action required
- **🟠 HIGH**: Serious threat, needs attention
- **🟡 MEDIUM**: Monitor closely
- **🟢 LOW**: Informational

### Log Details

Tap any log to view:
- Full description
- Threat details
- Timestamp
- Related URLs/images
- Recommended action

---

## 🔊 Voice Alerts

### How Voice Alerts Work

**TTS (Text-to-Speech)** technology announces threats in real-time:

- **Critical Threats**: Interrupts current audio, speaks immediately
- **High Threats**: Queued announcement
- **Medium/Low**: Normal queue

### Example Voice Alerts

- "Critical threat detected! Phishing link detected"
- "High priority threat detected. Suspicious image"
- "Unauthorized access detected, alert sent to family"
- "Suspicious transaction detected in Google Pay"

### Reading Logs Aloud

1. Tap the **floating speaker button** on dashboard
2. Voice will read:
   - Total number of logs
   - Up to 5 recent threats
   - Threat type and description

**Accessibility**: Designed for visually impaired users

---

## 🚨 Emergency Alerts

### When Are Alerts Triggered?

1. **Failed Biometric Authentication**
   - Someone tries to unlock app with wrong biometric
   - Multiple failed attempts

2. **Manual Trigger** (future)
   - Panic button activation

### Alert Sequence

1. **Front camera activates** (silent)
2. **Photo captured** of intruder
3. **GPS location obtained**
4. **WhatsApp message sent** to trusted contact:
   ```
   🚨 SECURITY ALERT 🚨
   
   Type: UNAUTHORIZED ACCESS DETECTED
   Time: 2025-10-01 16:16:49
   Someone tried to access your device without authorization.
   
   Location: https://maps.google.com/?q=lat,long
   
   This is an automated security alert from Axiom Security.
   ```
5. **If WhatsApp fails**, SMS is sent
6. **Voice Alert**: "Unauthorized access detected, alert sent to family"
7. **Threat logged** with CRITICAL severity

### Testing the Alert System

**Recommended**: Test once after setup
1. Go to Settings
2. Tap **Test Emergency Alert**
3. Check if trusted contact receives message
4. Note: Test does NOT capture photo

---

## 💳 Financial Fraud Monitoring

### Supported Payment Apps

- Google Pay (GPay)
- PhonePe
- Paytm
- Generic UPI apps
- PayPal

### What Gets Monitored

The app reads payment app notifications and checks for:

1. **Large Amounts**: Transactions > ₹10,000
2. **Unusual Times**: 12 AM - 6 AM
3. **Failed Transactions**: Declined/unsuccessful attempts
4. **Outgoing Payments**: Sent/debited/paid transactions

### Fraud Detection Algorithm

**Suspicion Score** calculated from:
- Amount: 30 points for large transactions
- Time: 20 points for late-night
- Status: 15 points for failed
- Type: 10 points for outgoing

**Threshold**: Score ≥30 triggers alert

### Example Scenarios

✅ **Detected**: ₹15,000 debited at 2 AM
✅ **Detected**: Failed transaction attempt × 3
✅ **Detected**: ₹50,000 sent to unknown number
❌ **Not flagged**: ₹500 received during daytime

### What Happens on Detection

1. Voice alert: "Suspicious transaction detected in Google Pay"
2. Threat logged with details
3. Notification displayed
4. User can review in threat logs

**Note**: This is monitoring only, not prevention. Check your bank app for actual security.

---

## 🔧 Troubleshooting

### Voice Alerts Not Working

**Possible Causes**:
- Device muted
- TTS engine not installed
- Volume too low

**Solutions**:
1. Check device volume
2. Go to Settings → Language → Text-to-Speech
3. Download TTS data if needed
4. Test with Read Aloud feature

### Emergency Alerts Not Sending

**Possible Causes**:
- No trusted contact set
- SMS permission denied
- No internet for WhatsApp

**Solutions**:
1. Verify trusted contact in Settings
2. Grant SMS permission
3. Test with **Test Emergency Alert**
4. Ensure WhatsApp is installed

### Financial Monitoring Not Working

**Possible Causes**:
- Notification access not granted
- Payment apps not installed

**Solutions**:
1. Enable Notification Access:
   - Settings → Apps → Special Access → Notification Access
   - Enable Axiom Security
2. Make a small test transaction
3. Check if notification appears in threat logs

### Camera Not Capturing Intruder Photo

**Possible Causes**:
- Camera permission denied
- Front camera not available

**Solutions**:
1. Grant camera permission
2. Test camera with device camera app
3. Restart app

### App Crashing on Startup

**Solutions**:
1. Clear app cache
2. Reinstall app
3. Check Android version (min SDK 26)
4. Report issue on GitHub

---

## 💡 Best Practices

### 1. Regular Monitoring
- Check dashboard daily
- Review threat logs weekly
- Clear resolved logs monthly

### 2. Link Safety
- Always scan links before clicking
- Be suspicious of shortened URLs
- Verify sender before trusting links

### 3. Image Verification
- Scan all received promotional images
- Check WhatsApp forwards for phishing
- Verify QR codes before scanning

### 4. Emergency Contact
- Choose someone you trust
- Update number if they change phones
- Test alerts periodically

### 5. Permission Management
- Keep all permissions granted
- Don't revoke critical permissions
- Update app when prompted

### 6. Transaction Awareness
- Monitor payment app notifications
- Check logs after large transactions
- Report suspicious activity to bank

### 7. Device Security
- Use strong biometric authentication
- Keep device locked when not in use
- Don't share device with untrusted people

### 8. Privacy
- Photos stored locally only
- No cloud upload
- Clear old data periodically

---

## 📱 Quick Reference Card

### Emergency Numbers
- **Cyber Crime Helpline (India)**: 1930
- **Banking Fraud**: Contact your bank immediately

### Common Phishing Indicators
- ❌ "Urgent action required"
- ❌ "Your account will be suspended"
- ❌ "Verify your information now"
- ❌ "Claim your prize"
- ❌ Shortened URLs (bit.ly, etc.)
- ❌ Misspelled domains

### Safe Practices
- ✅ Verify sender identity
- ✅ Check URL carefully
- ✅ Use official apps only
- ✅ Enable 2FA everywhere
- ✅ Regular app updates
- ✅ Scan before clicking

---

## 🆘 Getting Help

### In-App Support
- Check threat log details
- Use voice readout for clarity
- Test alert system

### External Resources
- GitHub Issues: [Repository Link]
- Email: support@axiomsecurity.com
- Documentation: README.md

### Reporting Bugs
1. Note the exact error message
2. Describe steps to reproduce
3. Include Android version
4. Open GitHub issue

---

## 🎓 Additional Resources

- [README.md](README.md) - Technical documentation
- [GitHub Repository](https://github.com/yourusername/axiom-security)
- Android Security Best Practices
- Phishing Awareness Training

---

**Stay Safe, Stay Secure! 🛡️**

*Last Updated: October 2025*
*Version: 1.0*
