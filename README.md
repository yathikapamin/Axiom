# Axiom Security - Comprehensive Cybersecurity Android App

![Version](https://img.shields.io/badge/version-1.0-blue)
![Platform](https://img.shields.io/badge/platform-Android-green)
![Min SDK](https://img.shields.io/badge/min%20SDK-26-orange)

A comprehensive cybersecurity Android application designed to protect users from phishing attacks, unauthorized access, financial fraud, and other digital threats with real-time voice alerts.

---

## 🛡️ Features

### 1. **Real-Time Threat Detection**

#### Phishing Link Detection
- Scans URLs for phishing indicators
- Analyzes suspicious domains and URL patterns
- Detects URL shorteners and IP-based addresses
- Checks for HTTPS/HTTP security
- Real-time threat scoring (0-100)
- Voice alerts for detected threats

#### Suspicious Image Detection
- OCR-based text extraction from images
- Detects phishing keywords in images
- QR code detection and warning
- Phone number and URL detection in images
- ML-powered image analysis

#### Financial Fraud Alerts
- Monitors payment app notifications (GPay, PhonePe, Paytm, etc.)
- Detects suspicious transaction patterns
- Alerts for large or unusual time transactions
- Real-time transaction monitoring
- Failed transaction attempt tracking

### 2. **Unauthorized Access Protection**

#### User Verification
- Biometric authentication (fingerprint/face)
- Device credential fallback
- Voice passphrase support

#### Immediate Security Response
- Automatic device lock on failed authentication
- Intruder photo capture using front camera
- Sensitive app protection

#### Emergency Alert System
- **Primary**: WhatsApp alert with:
  - Alert message
  - Intruder photo
  - Live GPS location
  - Timestamp
- **Fallback**: SMS alert if WhatsApp unavailable
- Automatic trusted contact notification

### 3. **Voice-First Alerts (TTS)**
- Real-time voice announcements for all threats
- Severity-based alert prioritization
- Hands-free operation
- Accessibility support for visually impaired users
- Customizable voice alerts

### 4. **Logging & Monitoring**
- Real-time threat log database
- Severity classification (Low, Medium, High, Critical)
- Threat categorization
- Timestamp and details tracking
- Voice readout of logs
- Mark logs as read/unread
- Export and review capabilities

### 5. **Offline Functionality**
- Works without internet connection
- SMS fallback for alerts
- Local threat detection
- Offline database storage

---

## 📱 Screenshots

*(Add screenshots of your app here)*

---

## 🚀 Installation

### Prerequisites
- Android Studio Hedgehog or later
- Android SDK 26 or higher
- Gradle 8.0+
- Kotlin 1.9+

### Setup Instructions

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/axiom-security.git
   cd axiom-security
   ```

2. **Open in Android Studio**
   - Launch Android Studio
   - Select "Open an Existing Project"
   - Navigate to the cloned repository

3. **Sync Gradle**
   - Wait for Gradle sync to complete
   - Resolve any dependency issues

4. **Configure Trusted Contact**
   - Run the app
   - Navigate to Settings
   - Add your trusted contact's phone number

5. **Grant Permissions**
   - Camera (for intruder photo)
   - Location (for GPS tracking)
   - SMS (for SMS alerts)
   - Notification Access (for financial fraud monitoring)
   - Biometric/Device Credential

---

## 🔧 Configuration

### Enable Notification Listener (Required for Financial Fraud Detection)

1. Open device Settings
2. Navigate to Apps → Special App Access → Notification Access
3. Enable "Axiom Security"

### Set Up Trusted Contact

1. Open Axiom Security app
2. Go to Settings
3. Enter phone number with country code (e.g., +91XXXXXXXXXX)
4. Click "Save Contact"
5. Test the alert system

---

## 📚 Architecture

### Tech Stack
- **Language**: Kotlin
- **UI Framework**: Jetpack Compose
- **Architecture**: MVVM (Model-View-ViewModel)
- **Database**: Room
- **Async**: Kotlin Coroutines & Flow
- **Camera**: CameraX
- **ML**: ML Kit (Text Recognition)
- **Location**: Google Play Services Location
- **Authentication**: Biometric API

### Project Structure
```
app/
├── data/
│   ├── ThreatLog.kt          # Data models
│   ├── ThreatLogDao.kt       # Database DAO
│   ├── AppDatabase.kt        # Room database
│   └── Converters.kt         # Type converters
├── services/
│   ├── VoiceAlertService.kt              # TTS alerts
│   ├── PhishingDetectionService.kt       # Link scanning
│   ├── ImageAnalysisService.kt           # Image scanning
│   ├── EmergencyAlertService.kt          # WhatsApp/SMS alerts
│   ├── BiometricAuthService.kt           # Authentication
│   ├── IntruderDetectionService.kt       # Photo capture
│   └── FinancialFraudDetectionService.kt # Transaction monitoring
├── viewmodel/
│   └── SecurityViewModel.kt  # App state management
├── ui/
│   └── screens/
│       ├── DashboardScreen.kt      # Main dashboard
│       ├── LinkScannerScreen.kt    # Link scanner UI
│       ├── ImageScannerScreen.kt   # Image scanner UI
│       └── SettingsScreen.kt       # Settings UI
├── receivers/
│   └── BootReceiver.kt       # Boot completion receiver
└── MainActivity.kt           # Main entry point
```

---

## 🎯 Usage

### Scanning a Suspicious Link
1. Open Axiom Security
2. Tap "Scan Link" on dashboard
3. Paste the URL
4. Tap "Scan Link"
5. Review the threat analysis
6. Voice alert will announce if threat detected

### Scanning a Suspicious Image
1. Open Axiom Security
2. Tap "Scan Image" on dashboard
3. Select image from gallery
4. Wait for analysis
5. Review detected threats and text
6. Voice alert will announce findings

### Monitoring Financial Transactions
- Enable Notification Access (see Configuration)
- App automatically monitors payment notifications
- Voice alert triggers for suspicious transactions
- Check dashboard for transaction logs

### Unauthorized Access Protection
- Set up trusted contact first
- Enable biometric authentication
- On failed authentication:
  - Front camera captures photo
  - GPS location obtained
  - WhatsApp/SMS alert sent to trusted contact
  - Voice alert: "Unauthorized access detected"

---

## 🔐 Security & Privacy

- **Local Processing**: All threat detection happens on-device
- **No Cloud Storage**: Photos and data stored locally
- **Encrypted Storage**: Room database with encryption support
- **Minimal Permissions**: Only essential permissions requested
- **Open Source**: Full transparency in code

---

## 🛠️ Dependencies

```kotlin
// Core Android
implementation("androidx.core:core-ktx:1.12.0")
implementation("androidx.lifecycle:lifecycle-runtime-ktx:2.6.2")
implementation("androidx.activity:activity-compose:1.8.2")

// Jetpack Compose
implementation("androidx.compose.ui:ui:1.5.4")
implementation("androidx.compose.material3:material3:1.1.2")

// Biometric Authentication
implementation("androidx.biometric:biometric:1.2.0-alpha05")

// CameraX
implementation("androidx.camera:camera-camera2:1.3.1")
implementation("androidx.camera:camera-lifecycle:1.3.1")
implementation("androidx.camera:camera-view:1.3.1")

// Location Services
implementation("com.google.android.gms:play-services-location:21.0.1")

// ML Kit
implementation("com.google.mlkit:text-recognition:16.0.0")

// Room Database
implementation("androidx.room:room-runtime:2.6.1")
implementation("androidx.room:room-ktx:2.6.1")

// WorkManager
implementation("androidx.work:work-runtime-ktx:2.9.0")

// Network
implementation("com.squareup.okhttp3:okhttp:4.12.0")
```

---

## 📊 Threat Detection Algorithms

### Phishing Link Scoring
- URL Shorteners: +30 points
- Suspicious Domain: +25 points
- IP Address: +40 points
- No HTTPS: +20 points
- Suspicious Characters: +15 points
- Multiple Subdomains: +10 points
- **Threshold**: 60+ = Phishing

### Image Analysis Scoring
- Suspicious Keywords: +15 per keyword
- URLs in Image: +25 points
- Phone Numbers: +10 points
- QR Code: +20 points
- **Threshold**: 30+ = Suspicious

### Financial Fraud Scoring
- Large Amount (>₹10,000): +30 points
- Unusual Time (12am-6am): +20 points
- Failed Transaction: +15 points
- Outgoing Payment: +10 points
- **Threshold**: 30+ = Suspicious

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 👥 Authors

- **Sujal** - Initial work

---

## 🙏 Acknowledgments

- Google ML Kit for text recognition
- CameraX for camera functionality
- Material Design 3 for beautiful UI
- Android Jetpack for modern Android development

---

## 📞 Support

For support, email support@axiomsecurity.com or open an issue in this repository.

---

## 🔮 Future Enhancements

- [ ] Voice passphrase authentication
- [ ] AI-powered behavioral analysis
- [ ] Real-time network traffic monitoring
- [ ] App permission analyzer
- [ ] Dark web breach monitoring
- [ ] VPN integration
- [ ] Encrypted messaging
- [ ] Remote device wipe
- [ ] Geofencing alerts
- [ ] Multi-language support

---

## ⚠️ Disclaimer

This app is designed for educational and personal security purposes. Always follow local laws and regulations regarding surveillance and monitoring. The developers are not responsible for misuse of this application.

---

**Made with ❤️ for a safer digital world**
