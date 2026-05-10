# 🚀 Build Instructions for Secure File Share

## Prerequisites

### 1. Java Development Kit (JDK)

**Required:** JDK 11 or higher

#### Check if Java is installed:
```powershell
java -version
```

#### If Java is NOT installed, choose one option:

**Option A: Install via winget (Recommended)**
```powershell
# Install JDK 17 (LTS)
winget install EclipseAdoptium.Temurin.17.JDK

# Or JDK 11 (LTS)
winget install EclipseAdoptium.Temurin.11.JDK
```

**Option B: Manual Download**
1. Download from: https://adoptium.net/
2. Choose "Latest LTS" (Java 17 or 21)
3. Install with default options
4. Restart your terminal

#### Set JAVA_HOME (if not automatically set):
```powershell
# Check current JAVA_HOME
$env:JAVA_HOME

# If empty, set it (replace path with your actual Java path)
[System.Environment]::SetEnvironmentVariable('JAVA_HOME', 'C:\Program Files\Eclipse Adoptium\jdk-17.0.x-hotspot', 'Machine')

# Restart terminal after setting
```

### 2. Android SDK (Optional for emulator)
You can build the APK without Android Studio, but you'll need the SDK to run in an emulator.

---

## 📦 Building the APK

### Clean Build
```powershell
cd "d:\Projects 2025\Final Year projects\FileShare"
.\gradlew clean
```

### Build Debug APK
```powershell
.\gradlew assembleDebug
```

**Output location:**
`app/build/outputs/apk/debug/app-debug.apk`

### Build Release APK (for distribution)
```powershell
.\gradlew assembleRelease
```

**Output location:**
`app/build/outputs/apk/release/app-release-unsigned.apk`

---

## 🔧 Common Build Commands

### Check Gradle Tasks
```powershell
.\gradlew tasks
```

### Install to Connected Device
```powershell
.\gradlew installDebug
```

### Build and Install
```powershell
.\gradlew assembleDebug installDebug
```

### Clean and Rebuild
```powershell
.\gradlew clean assembleDebug
```

---

## 📱 Installing on Android Device

### Method 1: Direct Install (ADB)
```powershell
# Enable USB debugging on your Android device first!
adb install app/build/outputs/apk/debug/app-debug.apk
```

### Method 2: Copy APK
1. Build the APK using `.\gradlew assembleDebug`
2. Copy `app-debug.apk` to your phone
3. Open it and install (enable "Install from unknown sources")

### Method 3: Android Studio
1. Open project in Android Studio
2. Click Run ▶️ button
3. Select connected device or emulator

---

## 🐛 Troubleshooting

### "gradlew is not recognized"
Make sure you're in the project root directory:
```powershell
cd "d:\Projects 2025\Final Year projects\FileShare"
```

### "JAVA_HOME is not set"
```powershell
# Find Java installation
where java

# Set JAVA_HOME (use your actual path)
$env:JAVA_HOME = "C:\Program Files\Eclipse Adoptium\jdk-17.0.x-hotspot"
```

### Build fails with dependency errors
```powershell
# Clear Gradle cache
.\gradlew clean --refresh-dependencies
```

### Permission Denied
```powershell
# Make gradlew executable (if needed)
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

---

## 📊 Build Output

### Successful Build
```
BUILD SUCCESSFUL in 45s
42 actionable tasks: 42 executed
```

Your APK will be at:
- Debug: `app\build\outputs\apk\debug\app-debug.apk`
- Release: `app\build\outputs\apk\release\app-release-unsigned.apk`

### APK Size
Expected size: ~3-5 MB (debug build)

---

## 🔐 Signing Release APK (Optional)

For Play Store or production deployment:

### 1. Generate Keystore
```powershell
keytool -genkeypair -v -keystore my-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias my-key-alias
```

### 2. Sign APK
```powershell
jarsigner -verbose -sigalg SHA256withRSA -digestalg SHA-256 -keystore my-release-key.jks app-release-unsigned.apk my-key-alias
```

### 3. Zipalign
```powershell
zipalign -v 4 app-release-unsigned.apk app-release-signed.apk
```

---

## ✅ Verification

### Check APK Contents
```powershell
.\gradlew assembleDebug --info | Select-String "APK"
```

### Verify Signature
```powershell
jarsigner -verify -verbose -certs app-debug.apk
```

### APK Analyzer (Android Studio)
1. Build → Analyze APK
2. Select your APK file
3. View size breakdown and contents

---

## 🚀 Quick Start (TL;DR)

```powershell
# 1. Install Java (if needed)
winget install EclipseAdoptium.Temurin.17.JDK

# 2. Navigate to project
cd "d:\Projects 2025\Final Year projects\FileShare"

# 3. Build APK
.\gradlew assembleDebug

# 4. Find your APK
# Output: app\build\outputs\apk\debug\app-debug.apk
```

---

## 📖 Next Steps After Building

1. **Test on Device**: Install APK on 2+ Android devices
2. **Test Features**: 
   - WiFi Direct connection
   - File transfer (single & batch)
   - QR code pairing
   - Transfer history
   - Settings changes
3. **Verify Security**: Check encryption is working
4. **Performance Test**: Try large file transfers
5. **UI/UX Review**: Test all screens and flows

---

## 💡 Development Tips

### Watch for Changes (Auto-rebuild)
```powershell
.\gradlew build --continuous
```

### Check Dependencies
```powershell
.\gradlew dependencies
```

### Lint Check
```powershell
.\gradlew lint
```

### Generate Docs
```powershell
.\gradlew javadoc
```

---

**Need help?** Open an issue or check the Gradle logs for detailed error messages!
