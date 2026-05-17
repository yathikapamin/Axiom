# 🚨 EMERGENCY FIX - App Not Working

## ✅ **I Just Fixed a Critical Bug!**

**Problem**: Missing icon resource causing build failure  
**Fixed**: Changed `R.drawable.ic_launcher_foreground` → `android.R.drawable.ic_dialog_alert`

---

## 🔧 **DO THIS NOW (5 Minutes):**

### **STEP 1: Sync Gradle** ⚠️ CRITICAL
```
Click "Sync Now" in the yellow bar at top
OR
File → Sync Project with Gradle Files
```

### **STEP 2: Clean Build**
```
Build → Clean Project
(Wait for it to finish)
```

### **STEP 3: Rebuild**
```
Build → Rebuild Project
(Wait 2-3 minutes)
```

### **STEP 4: Check for Errors**
Look at the **Build** tab at bottom. Should say:
```
BUILD SUCCESSFUL in Xs
```

If you see errors, **tell me the EXACT error message!**

---

## 📱 **If Build Succeeds, Run the App:**

### **STEP 5: Install**
```
Click Run ▶️ button
```

### **STEP 6: First-Time Setup**

**A. Grant Permissions:**
When app opens, tap "Allow" for:
- Camera
- Location
- SMS
- Notifications

**B. Biometric:**
- Use fingerprint or face to unlock
- OR tap "Use PIN" and enter device PIN

**C. Set Trusted Contact:**
1. Tap Settings (gear icon top-right)
2. Enter phone: `+919876543210`
3. Tap Save

---

## 🧪 **Quick Test (30 seconds):**

### **Test 1: Link Scanner**
1. From dashboard, tap **"Scan Link"**
2. Paste: `http://bit.ly/test123`
3. Tap **"Scan Link"** button

**Expected:**
- ✅ Shows threat score
- ✅ Voice says: "Phishing link detected" or "Link appears safe"
- ✅ Log appears in dashboard

**If this works, your app is functional!**

---

## 🔍 **If Still Not Working, Tell Me:**

### **What exactly is not working?**

**Option A: App won't build/install**
- What error do you see in Build tab?
- Copy and paste the EXACT error message

**Option B: App crashes on open**
- Does it show any error screen?
- Check LogCat: `adb logcat | grep FATAL`

**Option C: App opens but features don't work**
Which specific feature?
- [ ] Link scanner
- [ ] Image scanner  
- [ ] Settings
- [ ] Dashboard
- [ ] Background detection

**Option D: Background features don't work**
- Did you enable Notification Access?
  ```
  Settings → Apps → Special Access → Notification Access → Axiom → ON
  ```

**Option E: Voice alerts don't work**
- Is volume up?
- Is TTS installed? (Google Text-to-Speech from Play Store)

---

## 📋 **Diagnostic Commands**

Run these and tell me the output:

### **Check if app is installed:**
```bash
adb shell pm list packages | grep axiom
```
Should show: `package:com.example.axiom`

### **Check app logs:**
```bash
adb logcat | grep -i axiom
```

### **Check for crashes:**
```bash
adb logcat | grep -E "FATAL|AndroidRuntime"
```

---

## 🎯 **Most Common Issues & Fixes:**

### **Issue 1: "Unresolved reference"**
**Fix:** 
```
File → Invalidate Caches → Invalidate and Restart
```

### **Issue 2: "Gradle sync failed"**
**Fix:**
```bash
./gradlew --stop
./gradlew clean build
```

### **Issue 3: "App crashes immediately"**
**Fix:** Check if all services exist. Run:
```bash
adb logcat | grep "ClassNotFoundException"
```

### **Issue 4: "Features work in app but not in background"**
**Fix:** Enable notification access (see Step 6B above)

### **Issue 5: "No voice alerts"**
**Fix:**
1. Install Google TTS from Play Store
2. Check volume
3. Test: Settings → Language → Text-to-Speech

---

## ⚡ **Quick Checklist:**

**Before testing, verify:**
- [ ] Gradle synced successfully
- [ ] Build completed without errors
- [ ] App installed on device
- [ ] All permissions granted
- [ ] Trusted contact saved
- [ ] Notification access enabled (for background features)
- [ ] Volume is up (for voice alerts)

---

## 💬 **Tell Me Specifically:**

**What's the problem?**

1. **Build issue:** "I see error: [paste error here]"
2. **Crash issue:** "App crashes when I [do what?]"
3. **Feature issue:** "[Which feature?] doesn't work"
4. **Nothing happens:** "I tap [what?] and nothing happens"

**The more specific you are, the faster I can fix it!**

---

## 🚀 **Expected Working State:**

After following steps above, you should be able to:

✅ **Open app** → See dashboard with "No threat logs" or existing logs
✅ **Tap "Scan Link"** → Enter URL → See results + voice alert
✅ **Tap "Scan Image"** → Select image → See results
✅ **Tap Settings** → Enter phone → Save → See toast "Contact saved"
✅ **Tap "Test Alert"** → WhatsApp opens with message

**If ANY of these work, the app is functional!**

---

## 📱 **Alternative: Start Fresh**

If nothing works, try:

```bash
# 1. Uninstall completely
adb uninstall com.example.axiom

# 2. Clean everything
./gradlew clean

# 3. Rebuild
./gradlew assembleDebug

# 4. Install
adb install app/build/outputs/apk/debug/app-debug.apk

# 5. Open app
adb shell am start -n com.example.axiom/.MainActivity
```

---

## 🆘 **I'm Here to Help!**

**Tell me EXACTLY what's happening:**
- What did you do?
- What did you expect?
- What actually happened?
- Any error messages?

**I'll fix it immediately!** 🔧
