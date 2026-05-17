# 🔗 Test Links by Severity Level

## 🟢 LOW Severity (Score: 0-39)

### **Safe Links - Score 0-10**
```
https://www.google.com
https://www.github.com
https://www.wikipedia.org
https://www.microsoft.com
https://www.amazon.com
https://www.facebook.com
https://www.apple.com
https://www.netflix.com
```
**Expected:**
- Score: 0-10
- Severity: LOW (Green)
- Message: "Link appears relatively safe, but exercise caution"
- Threats: None or minimal
- Voice Alert: None

---

### **Slightly Suspicious - Score 20-25**
```
http://www.example.com
http://legitimate-site.org
http://company-website.net
```
**Expected:**
- Score: 20 (HTTP only)
- Severity: LOW (Green)
- Threats: "Insecure connection (HTTP instead of HTTPS)"

---

### **Minor Concerns - Score 25-35**
```
https://g00gle.com
https://faceb00k.com
https://micros0ft.com
```
**Expected:**
- Score: 25 (Typosquatting only)
- Severity: LOW (Green)
- Threats: "Possible typosquatting - impersonating known brand"

---

## 🟡 MEDIUM Severity (Score: 40-59)

### **URL Shorteners - Score 55**
```
http://bit.ly/test123
http://bit.ly/bankverify
http://bit.ly/urgent-update
http://tinyurl.com/sample
http://tinyurl.com/verify-account
http://goo.gl/maps
http://goo.gl/login
http://t.co/update
http://ow.ly/secure
```
**Expected:**
- Score: 55 (Shortener 35 + HTTP 20)
- Severity: MEDIUM (Yellow)
- Message: "DO NOT CLICK - Likely phishing attempt"
- Threats: "URL shortener detected", "Insecure connection"
- Voice Alert: ✅ "Warning: Phishing link detected"

---

### **Suspicious TLDs - Score 50**
```
http://verify-account.tk
http://secure-login.ga
http://banking-alert.ml
http://update-payment.cf
http://account-verify.gq
http://urgent-update.tk
http://confirm-details.ga
http://win-prize.ml
```
**Expected:**
- Score: 50 (TLD 30 + HTTP 20)
- Severity: MEDIUM (Yellow)
- Threats: "Suspicious domain extension (.tk, .ml, .ga, etc.)", "Insecure connection"
- Voice Alert: ✅

---

### **Typosquatting - Score 45**
```
http://amaz0n.com
http://g00gle.com
http://paypa1.com
http://faceb00k.com
http://micros0ft.com
http://app1e.com
http://netf1ix.com
```
**Expected:**
- Score: 45 (Typosquatting 25 + HTTP 20)
- Severity: MEDIUM (Yellow)
- Threats: "Possible typosquatting", "Insecure connection"
- Voice Alert: ✅

---

## 🟠 HIGH Severity (Score: 60-79)

### **IP Addresses - Score 65-75**
```
http://192.168.1.1
http://192.168.1.100/login
http://10.0.0.1/secure
http://172.16.0.1/verify
http://203.0.113.0/banking
http://198.51.100.0/account
```
**Expected:**
- Score: 65-75 (IP 45 + HTTP 20 + sometimes subdomain 10)
- Severity: HIGH (Orange)
- Threats: "Direct IP address used", "Insecure connection"
- Voice Alert: ✅

---

### **URL Shortener + TLD - Score 65**
```
https://bit.ly/verify.tk
https://tinyurl.com/secure.ga
```
**Expected:**
- Score: 65+ (Shortener 35 + TLD 30)
- Severity: HIGH (Orange)
- Threats: Multiple high-risk indicators
- Voice Alert: ✅

---

### **Multiple Red Flags - Score 70-75**
```
http://secure-bank-verify.tk
http://urgent-paypal-update.ml
http://amazon-prize-winner.ga
http://microsoft-security-alert.cf
```
**Expected:**
- Score: 70-75 (TLD 30 + HTTP 20 + Typosquatting 25)
- Severity: HIGH (Orange)
- Threats: "Suspicious domain", "Typosquatting", "Insecure connection"
- Voice Alert: ✅

---

## 🔴 CRITICAL Severity (Score: 80-100)

### **IP + Suspicious Characters - Score 80+**
```
http://192.168.1.1@phishing.com
http://10.0.0.1/login?user=admin&pass=123
http://172.16.0.1/verify-account-now-urgent
```
**Expected:**
- Score: 80+ (IP 45 + HTTP 20 + Suspicious chars 15)
- Severity: CRITICAL (Red)
- Threats: Multiple critical indicators
- Voice Alert: ✅ "Critical threat detected!"

---

### **URL Shortener + TLD + HTTP - Score 85**
```
http://bit.ly/verify-account.tk
http://tinyurl.com/secure-login.ga
http://goo.gl/banking-alert.ml
```
**Expected:**
- Score: 85 (Shortener 35 + TLD 30 + HTTP 20)
- Severity: CRITICAL (Red)
- Threats: "URL shortener", "Suspicious domain", "Insecure connection"
- Voice Alert: ✅ "Critical threat detected!"

---

### **IP + Typosquatting - Score 90**
```
http://192.168.1.1/amaz0n-login
http://10.0.0.1/g00gle-verify
http://172.16.0.1/paypa1-secure
```
**Expected:**
- Score: 90 (IP 45 + Typosquatting 25 + HTTP 20)
- Severity: CRITICAL (Red)
- Threats: "IP address", "Typosquatting", "Insecure connection"
- Voice Alert: ✅ "Critical threat detected!"

---

### **Maximum Risk - Score 95-100**
```
http://192.168.1.1@amaz0n.tk
http://bit.ly/secure-bank-login-verify-urgent.ml
http://10.0.0.1/paypa1-account-suspended-verify-now.ga
```
**Expected:**
- Score: 95-100 (Multiple indicators combined, capped at 100)
- Severity: CRITICAL (Red)
- Threats: All detection categories triggered
- Voice Alert: ✅ "Critical threat detected!"

---

## 📋 Quick Test Script

Copy and paste these into the Link Scanner to test each severity:

### **Test 1: LOW (Safe)**
```
https://www.google.com
```
Expected: Score 0, GREEN, "Link appears relatively safe"

### **Test 2: MEDIUM (Shortener)**
```
http://bit.ly/test123
```
Expected: Score 55, YELLOW, "DO NOT CLICK - Likely phishing"

### **Test 3: HIGH (IP Address)**
```
http://192.168.1.1
```
Expected: Score 65-75, ORANGE, "DO NOT CLICK - Likely phishing"

### **Test 4: CRITICAL (Multiple Flags)**
```
http://bit.ly/verify-account.tk
```
Expected: Score 85+, RED, "DO NOT CLICK - Likely phishing"

---

## 🎯 Scoring Reference

| Component | Points | Examples |
|-----------|--------|----------|
| **URL Shortener** | +35 | bit.ly, tinyurl.com, goo.gl |
| **Suspicious TLD** | +30 | .tk, .ml, .ga, .cf, .gq |
| **Typosquatting** | +25 | g00gle, amaz0n, paypa1 |
| **HTTP (not HTTPS)** | +20 | http:// instead of https:// |
| **IP Address** | +45 | 192.168.1.1, 10.0.0.1 |
| **Suspicious Chars** | +15 | @, excessive hyphens, unicode |
| **Multiple Subdomains** | +10 | www.secure.login.site.com |

### **Severity Thresholds:**
- **0-39**: 🟢 LOW (Green) - "Link appears relatively safe"
- **40-59**: 🟡 MEDIUM (Yellow) - "DO NOT CLICK - Likely phishing"
- **60-79**: 🟠 HIGH (Orange) - "DO NOT CLICK - Likely phishing"
- **80-100**: 🔴 CRITICAL (Red) - "DO NOT CLICK - Likely phishing"

---

## 🧪 Testing Checklist

Test one link from each category:

- [ ] **LOW**: `https://www.google.com` → Score 0-10 ✅
- [ ] **MEDIUM**: `http://bit.ly/test123` → Score 55 🟡
- [ ] **HIGH**: `http://192.168.1.1` → Score 65-75 🟠
- [ ] **CRITICAL**: `http://bit.ly/verify.tk` → Score 85+ 🔴

---

## 📱 Voice Alert Behavior

- **Score 0-39** (LOW): ❌ No voice alert
- **Score 40-59** (MEDIUM): ✅ "Warning: Phishing link detected"
- **Score 60-79** (HIGH): ✅ "High priority threat detected"
- **Score 80-100** (CRITICAL): ✅ "Critical threat detected!"

---

## 🎓 Real-World Phishing Examples

### **Common Phishing Patterns:**

**Banking Scams:**
```
http://verify-account.tk
http://banking-alert.ml
http://suspend-account.ga
```

**Tech Support Scams:**
```
http://micros0ft-support.tk
http://app1e-security.ml
```

**Prize/Lottery Scams:**
```
http://winner-claim.ga
http://prize-winner.tk
```

**Package Delivery Scams:**
```
http://fedex-delivery.ml
http://ups-package.tk
```

All these will score **50+** (MEDIUM or higher) and trigger alerts! 🛡️

---

**Use these links to thoroughly test all severity levels!**
