# 📋 Phishing Detection Test Results Checklist

## ✅ Installation Steps

- [ ] Run: `adb install -r app/build/outputs/apk/debug/app-debug.apk`
- [ ] Open Axiom Security app
- [ ] Grant all permissions
- [ ] Navigate to Link Scanner screen

---

## 🧪 Test Case Results

### Test 1: `http://bit.ly/test123`
- [ ] **Score**: _____ (Expected: 55)
- [ ] **Threats Detected**: 
  - [ ] URL shortener detected
  - [ ] Insecure connection (HTTP)
- [ ] **Severity**: _____ (Expected: MEDIUM - Yellow)
- [ ] **Voice Alert**: _____ (YES/NO)
- [ ] **Logged to Dashboard**: _____ (YES/NO)

**Status**: ⬜ PASS / ⬜ FAIL

---

### Test 2: `http://tinyurl.com/sample`
- [ ] **Score**: _____ (Expected: 55)
- [ ] **Threats Detected**: 
  - [ ] URL shortener detected
  - [ ] Insecure connection (HTTP)
- [ ] **Severity**: _____ (Expected: MEDIUM)

**Status**: ⬜ PASS / ⬜ FAIL

---

### Test 3: `http://192.168.1.1`
- [ ] **Score**: _____ (Expected: 65)
- [ ] **Threats Detected**: 
  - [ ] Direct IP address used
  - [ ] Insecure connection (HTTP)
- [ ] **Severity**: _____ (Expected: HIGH - Orange)

**Status**: ⬜ PASS / ⬜ FAIL

---

### Test 4: `http://verify-account.tk`
- [ ] **Score**: _____ (Expected: 50-75)
- [ ] **Threats Detected**: 
  - [ ] Suspicious domain extension (.tk)
  - [ ] Insecure connection (HTTP)
- [ ] **Severity**: _____ (Expected: MEDIUM/HIGH)

**Status**: ⬜ PASS / ⬜ FAIL

---

### Test 5: `http://amaz0n.com`
- [ ] **Score**: _____ (Expected: 45)
- [ ] **Threats Detected**: 
  - [ ] Possible typosquatting
  - [ ] Insecure connection (HTTP)
- [ ] **Severity**: _____ (Expected: MEDIUM)

**Status**: ⬜ PASS / ⬜ FAIL

---

### Test 6: `http://g00gle.com`
- [ ] **Score**: _____ (Expected: 45)
- [ ] **Threats Detected**: 
  - [ ] Possible typosquatting
  - [ ] Insecure connection (HTTP)
- [ ] **Severity**: _____ (Expected: MEDIUM)

**Status**: ⬜ PASS / ⬜ FAIL

---

### Test 7: `https://www.google.com` (SAFE LINK)
- [ ] **Score**: _____ (Expected: 0-10)
- [ ] **Threats Detected**: _____ (Expected: None or minimal)
- [ ] **Severity**: _____ (Expected: LOW - Green)
- [ ] **Shows as SAFE**: _____ (YES/NO)

**Status**: ⬜ PASS / ⬜ FAIL

---

## 🔍 Logcat Output (If Test Fails)

Run this command and paste output below:
```bash
adb logcat -s PhishingDetection:D -d
```

**Output:**
```
[Paste logcat output here]
```

---

## 📊 Summary

**Total Tests**: 7  
**Passed**: _____  
**Failed**: _____  

**Overall Status**: ⬜ ALL PASS / ⬜ SOME FAILED

---

## 🐛 If Tests Fail

### Check These:

1. **APK Version**
   ```bash
   adb shell dumpsys package com.example.axiom | grep versionCode
   ```

2. **Clear App Data**
   ```bash
   adb shell pm clear com.example.axiom
   ```

3. **Reinstall Fresh**
   ```bash
   adb uninstall com.example.axiom
   adb install app/build/outputs/apk/debug/app-debug.apk
   ```

4. **Check Logs in Real-Time**
   ```bash
   adb logcat -c  # Clear logs
   # Then test a link in the app
   adb logcat -s PhishingDetection:D
   ```

---

## 📝 Notes

Write any observations here:
- 
- 
- 

---

**Date Tested**: __________  
**Device**: __________  
**Android Version**: __________
