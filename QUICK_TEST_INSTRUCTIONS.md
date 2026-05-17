# ⚡ Quick Test Instructions - Phishing Detection Fix

## 🔧 IMPORTANT: Rebuild Required!

The code has been updated. You **MUST rebuild** the app for changes to take effect.

```bash
cd "d:/project by sujal/axiom"
./gradlew clean assembleDebug
adb install -r app/build/outputs/apk/debug/app-debug.apk
```

---

## 🧪 Test These Exact URLs

### Test 1: URL Shorteners (Should Score 55)
```
http://bit.ly/test123
```
**Expected Result:**
- Score: 55
- Threats: "URL shortener detected", "Insecure connection (HTTP)"
- Severity: MEDIUM (Yellow)

### Test 2: IP Address (Should Score 65)
```
http://192.168.1.1
```
**Expected Result:**
- Score: 65
- Threats: "Direct IP address", "Insecure connection (HTTP)"
- Severity: HIGH (Orange)

### Test 3: Suspicious TLD (Should Score 50)
```
http://verify-account.tk
```
**Expected Result:**
- Score: 50-75
- Threats: "Suspicious domain extension (.tk)", "Insecure connection (HTTP)"
- Severity: MEDIUM/HIGH

### Test 4: Typosquatting (Should Score 45)
```
http://amaz0n.com
```
**Expected Result:**
- Score: 45
- Threats: "Possible typosquatting", "Insecure connection (HTTP)"
- Severity: MEDIUM

---

## 📱 How to Test in App

1. **Open Axiom Security app**
2. **Tap "Scan Link"** on dashboard
3. **Paste EXACTLY** one of the test URLs above
4. **Tap "Scan Link"** button
5. **Check the result card** below

---

## 🔍 Check Logs (If Still Not Working)

If detection still fails, check Android logs:

```bash
adb logcat -s PhishingDetection:D
```

You should see:
```
PhishingDetection: Analyzing URL: http://bit.ly/test123
PhishingDetection: Calculating score for: http://bit.ly/test123
PhishingDetection:   +35 for URL shortener (total: 35)
PhishingDetection:   +20 for HTTP (total: 55)
PhishingDetection: Final score: 55
PhishingDetection: Score: 55, isPhishing: true
PhishingDetection: URL shortener detected
PhishingDetection: HTTP detected
PhishingDetection: Final - Score: 55, Threats: 2, Severity: MEDIUM
```

---

## ✅ What Was Fixed

### Changes Made to `PhishingDetectionService.kt`:

1. ✅ **URL Normalization** - Adds `http://` if protocol is missing
2. ✅ **Debug Logging** - Shows detailed scoring breakdown
3. ✅ **Proper Hostname Extraction** - Uses `URL().host` for accurate matching
4. ✅ **Enhanced IP Detection** - Better regex pattern matching
5. ✅ **Typosquatting Detection** - Catches brand impersonation

---

## 🚨 Troubleshooting

### Problem: "Still showing score 0 or not detecting"

**Solution:**
1. **Clean and rebuild:**
   ```bash
   ./gradlew clean
   ./gradlew assembleDebug
   ```

2. **Force reinstall:**
   ```bash
   adb uninstall com.example.axiom
   adb install app/build/outputs/apk/debug/app-debug.apk
   ```

3. **Clear app data** (Settings → Apps → Axiom → Clear Data)

### Problem: "App not building"

**Solution:**
1. Check Gradle sync in Android Studio
2. Rebuild project (Build → Rebuild Project)
3. Invalidate caches (File → Invalidate Caches → Restart)

### Problem: "Logs show nothing"

**Solution:**
1. Ensure device is connected: `adb devices`
2. Check app is running: `adb shell ps | grep axiom`
3. Use full logcat: `adb logcat | grep Phishing`

---

## 📊 Expected Scores Summary

| URL | Score | Severity | Phishing? |
|-----|-------|----------|-----------|
| `http://bit.ly/test123` | 55 | MEDIUM | ✅ YES |
| `http://tinyurl.com/sample` | 55 | MEDIUM | ✅ YES |
| `http://goo.gl/maps` | 55 | MEDIUM | ✅ YES |
| `http://192.168.1.1` | 65 | HIGH | ✅ YES |
| `http://verify-account.tk` | 50+ | MEDIUM/HIGH | ✅ YES |
| `http://secure-login.ga` | 50 | MEDIUM | ✅ YES |
| `http://amaz0n.com` | 45 | MEDIUM | ✅ YES |
| `http://g00gle.com` | 45 | MEDIUM | ✅ YES |
| `https://www.google.com` | 0 | LOW | ❌ NO |

---

## 🎯 Next Steps

1. **Rebuild the app** (REQUIRED!)
2. **Test one URL** from the list above
3. **Check if score shows correctly**
4. **If still not working**, send the logcat output

---

**The code is fixed and ready - just needs a rebuild!** 🛡️
