# 🛡️ Background Protection Guide

## Overview
Axiom Security now monitors **ALL incoming messages and notifications** for phishing links, even when the app is **closed or not in use**. The app will automatically detect suspicious links from WhatsApp, SMS, Telegram, and other messaging apps and trigger voice alerts with full-screen warnings.

---

## 🔧 How to Enable Background Protection

### Step 1: Open Axiom App
Launch the Axiom Security app on your device.

### Step 2: Grant Notification Access
When you first open the app, you'll see a dialog asking to **"Enable Background Protection"**. 

1. Tap **"Enable Now"**
2. You'll be taken to Android Settings → **Notification Access**
3. Find **"Axiom"** in the list
4. Toggle it **ON**
5. Confirm the permission

**Alternative Method:**
- Open Settings screen in the app
- Tap the **"Enable Background Protection"** button
- Follow the same steps above

---

## ✅ What Gets Monitored (Even When App is Closed)

### Messaging Apps:
- ✉️ **WhatsApp** - All messages
- 📱 **SMS/MMS** - Text messages
- 🔵 **Telegram** - All chats
- 💬 **Facebook Messenger**
- 📷 **Instagram DMs**
- 👻 **Snapchat**
- 🐦 **Twitter DMs**
- 🎮 **Discord**

### Financial Apps:
- 💳 **Google Pay**
- 📱 **PhonePe**
- 💰 **Paytm**
- 🏦 **Other UPI apps**

### ALL Other Apps:
- Any app that shows notifications with links will be scanned!

---

## 🚨 What Happens When a Threat is Detected?

### 1. **Voice Alert (Immediate)**
   - Your phone will speak: *"Warning! Phishing link detected from [App Name]. Do not click the link!"*
   - This happens **instantly** when the threat is detected

### 2. **Full-Screen Red Alert**
   - Your screen will be taken over by a red warning screen
   - Shows:
     - Threat type (e.g., "Phishing Link Detected")
     - Source app (e.g., "WhatsApp")
     - The suspicious link/content
     - Warning details

### 3. **Notification (Backup)**
   - A persistent notification appears in your notification bar
   - In case you dismiss the full-screen alert

### 4. **Logged in Dashboard**
   - All threats are logged with:
     - Timestamp
     - Source app
     - Threat severity
     - Complete details

---

## 📋 Example Scenarios

### Scenario 1: WhatsApp Phishing Link
```
You receive a WhatsApp message: "Your bank account will be blocked! Click here immediately: http://bit.ly/fake-bank"

→ Axiom detects the shortened URL and suspicious text
→ Voice Alert: "Warning! Phishing link detected from WhatsApp..."
→ Full-screen red alert appears
→ Notification shows up
→ You are protected! ✅
```

### Scenario 2: SMS with Fake Banking Link
```
SMS from unknown number: "Dear customer, verify KYC: http://bankverify-123.tk/login"

→ Axiom detects suspicious TLD (.tk) and phishing patterns
→ Immediate voice warning
→ Red screen alert blocks the message
→ Logged in threat logs
```

### Scenario 3: Financial Transaction Alert
```
You receive: "Rs. 15,000 debited from your account at 3:00 AM"

→ Axiom flags: Large amount + unusual time
→ Voice alert for suspicious transaction
→ Alert saved to logs for review
```

---

## 🔋 Battery & Performance

### Battery Optimization
The background service is designed to be **extremely efficient**:
- Uses minimal CPU and RAM
- Runs as a lightweight foreground service
- Only activates when notifications arrive
- **No constant polling or background work**

### Expected Battery Impact
- **< 2-3% per day** (negligible)
- Comparable to other notification-based apps

---

## ⚙️ Important Settings

### Keep App Running
To ensure Axiom always protects you:

1. **Disable Battery Optimization** (Android 13+):
   - Settings → Apps → Axiom → Battery
   - Select "Unrestricted"

2. **Auto-Start Enabled** (On some devices):
   - Settings → Apps → Axiom → Autostart
   - Toggle ON

3. **Background Activity**:
   - Settings → Apps → Axiom → Data Usage
   - Allow "Background data"

---

## 🧪 Testing the System

### Test 1: Send Yourself a Test Message
1. Open WhatsApp Web on your computer
2. Send yourself this test link: `http://bit.ly/test-link`
3. Axiom should trigger an alert!

### Test 2: Use the Built-in Test
1. Go to Settings in Axiom
2. Tap "Test Emergency Alert"
3. Listen for voice alert and see the notification

---

## 🔒 Privacy & Security

### What Data is Collected?
- **NO DATA leaves your device**
- All scanning happens **locally** on your phone
- Threat logs are stored in **local database only**
- **No internet connection required** for detection

### What Does Axiom Access?
- ✅ **Notification Content** - Only to scan for threats
- ✅ **App Names** - To identify source of threats
- ❌ **NOT** - Your personal messages are not stored
- ❌ **NOT** - No data sent to external servers

---

## 📊 Monitoring Status

### Check if Background Protection is Active:
1. Look for persistent notification: "Axiom Security Active"
2. If you don't see it:
   - Reboot your device
   - Open Axiom app
   - Re-enable notification access

### Service Auto-Restart:
- Axiom automatically restarts after device reboot
- The `BootReceiver` ensures protection is always on

---

## 🆘 Troubleshooting

### Problem: Voice alerts not working
**Solution:**
- Go to Settings → Apps → Axiom → Permissions
- Ensure "Microphone" permission is granted
- Check device volume is not on silent

### Problem: No alerts showing
**Solution:**
- Verify Notification Access is enabled
- Check if Axiom is in battery optimization whitelist
- Restart the device

### Problem: Alerts appearing for safe links
**Solution:**
- This is a false positive
- Axiom errs on the side of caution
- You can dismiss the alert if you trust the source

---

## 🎯 Best Practices

1. **Keep Notification Access Enabled** - This is the core of background protection
2. **Don't Disable Notifications** - Allow Axiom to show alerts
3. **Review Threat Logs Regularly** - Check dashboard for patterns
4. **Set Trusted Contact** - For emergency unauthorized access alerts
5. **Test Periodically** - Ensure the system is working

---

## 🔮 What's Protected

| Feature | Protected When App is Closed |
|---------|------------------------------|
| Phishing Link Detection | ✅ YES |
| Suspicious Image Detection | ✅ YES |
| Financial Fraud Monitoring | ✅ YES |
| Voice Alerts | ✅ YES |
| Full-Screen Alerts | ✅ YES |
| Threat Logging | ✅ YES |
| Biometric Intruder Detection | ❌ NO (requires app open) |

---

## 📞 Need Help?

If background protection isn't working:
1. Check all settings mentioned in "Important Settings"
2. Review "Troubleshooting" section
3. Restart device and try again
4. Ensure you're running Android 8.0 or higher

---

## 🎉 You're Protected!

Once notification access is enabled, Axiom works **24/7 in the background** to protect you from:
- 🎣 Phishing links
- 💰 Financial fraud
- 🖼️ Suspicious images
- 🔗 URL shortener scams
- 🏦 Fake banking messages

**Stay safe! 🛡️**
