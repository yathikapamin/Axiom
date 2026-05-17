# 🔍 Debug Instructions - Phishing Detection Not Working

## ⚡ IMMEDIATE TEST

I've added automatic tests that run when the app starts. Follow these steps:

### **Step 1: Install the New Build**
```bash
adb install -r app/build/outputs/apk/debug/app-debug.apk
```

### **Step 2: Clear Previous Logs**
```bash
adb logcat -c
```

### **Step 3: Launch the App**
```bash
adb shell am start -n com.example.axiom/.MainActivity
```

### **Step 4: Check the Test Results**
```bash
adb logcat -s TestPhishing:D PhishingDetection:D
```

---

## 📊 What You Should See in Logs

### **If Detection is Working:**
```
TestPhishing: === STARTING PHISHING DETECTION TESTS ===

TestPhishing: --- Testing: URL Shortener ---
TestPhishing: URL: http://bit.ly/test123
PhishingDetection: Analyzing URL: http://bit.ly/test123
PhishingDetection:   +35 for URL shortener (total: 35)
PhishingDetection:   +20 for HTTP (total: 55)
TestPhishing: Score: 55 (Expected: ~55)
TestPhishing: Is Phishing: true
TestPhishing: Threats (2):
TestPhishing:   - URL shortener detected - destination unknown
TestPhishing:   - Insecure connection (HTTP instead of HTTPS)
TestPhishing: Result: ✅ PASS

[... more tests ...]

TestPhishing: === TESTS COMPLETE ===
```

### **If Detection is NOT Working:**
```
TestPhishing: Score: 0 (Expected: ~55)
TestPhishing: Is Phishing: false
TestPhishing: Threats (0):
TestPhishing: Result: ❌ FAIL
```

---

## 🐛 Possible Issues & Solutions

### **Issue 1: Score is Always 0**

**Cause:** Detection methods not being called

**Solution:** Check these logs:
```bash
adb logcat | grep "PhishingDetection"
```

If you see NO "PhishingDetection" logs, the methods aren't being called.

---

### **Issue 2: Threats Array is Empty**

**Cause:** Conditions not matching properly

**Debug:** Look for these specific messages:
```
PhishingDetection: URL shortener detected
PhishingDetection: HTTP detected
PhishingDetection: IP address detected
PhishingDetection: Suspicious domain detected
```

---

### **Issue 3: App Crashes on Start**

**Solution:**
```bash
adb logcat -s AndroidRuntime:E
```
Look for stack trace and share the error.

---

## 🧪 Manual Test in App (After Auto Tests)

1. **Open the app** (should launch automatically)
2. **Go to Link Scanner**
3. **Paste:** `http://bit.ly/test123`
4. **Tap Scan Link**
5. **Watch logcat**:
   ```bash
   adb logcat -s PhishingDetection:D
   ```

---

## 📋 Share These with Me

If it's still not working, share the output of these commands:

### **Command 1: Auto Test Results**
```bash
adb logcat -s TestPhishing:D -d
```

### **Command 2: Detection Logs**
```bash
adb logcat -s PhishingDetection:D -d
```

### **Command 3: Any Errors**
```bash
adb logcat -s AndroidRuntime:E -d
```

### **Command 4: App Info**
```bash
adb shell dumpsys package com.example.axiom | findstr "versionCode versionName"
```

---

## 🎯 Expected Test Results

| Test | URL | Expected Score | Expected Result |
|------|-----|----------------|-----------------|
| 1 | `http://bit.ly/test123` | 55 | ✅ PHISHING |
| 2 | `http://tinyurl.com/sample` | 55 | ✅ PHISHING |
| 3 | `http://192.168.1.1` | 65 | ✅ PHISHING |
| 4 | `http://verify-account.tk` | 50+ | ✅ PHISHING |
| 5 | `http://amaz0n.com` | 45 | ✅ PHISHING |
| 6 | `https://www.google.com` | 0-10 | ❌ SAFE |

---

## ⚠️ Important Notes

1. **The app MUST be rebuilt** - I've added new test code
2. **Tests run automatically** when app launches - check logs immediately
3. **If auto tests pass but UI doesn't work** - it's a UI issue, not detection
4. **If auto tests fail** - it's a detection logic issue

---

## 🔄 Full Reset (If Nothing Works)

```bash
# 1. Uninstall completely
adb uninstall com.example.axiom

# 2. Clean build
cd "d:/project by sujal/axiom"
./gradlew clean

# 3. Rebuild
./gradlew assembleDebug

# 4. Fresh install
adb install app/build/outputs/apk/debug/app-debug.apk

# 5. Clear logs
adb logcat -c

# 6. Launch and watch
adb shell am start -n com.example.axiom/.MainActivity && adb logcat -s TestPhishing:D PhishingDetection:D
```

---

**Install the new APK and check the logs - they will tell us exactly what's happening!** 🛡️
