# Grocery Store POS - Android Mobile Application

Modern Kotlin/Jetpack Compose mobile application for delivery personnel and mobile POS operations.

## Key Features

✅ Real-time order tracking  
✅ Delivery GPS tracking and routing  
✅ Offline-first architecture with Room database  
✅ Background synchronization with WorkManager  
✅ Push notifications via Firebase Cloud Messaging  
✅ Multi-language support (Arabic & English)  
✅ Biometric authentication  
✅ QR code scanning  
✅ Photo capture and upload  
✅ Real-time inventory updates  

## Technology Stack

- **Language**: Kotlin
- **UI Framework**: Jetpack Compose
- **Database**: Room Database
- **Networking**: Retrofit + OkHttp
- **Real-time**: Firebase Cloud Messaging
- **Background Jobs**: WorkManager
- **Maps**: Google Maps API
- **Authentication**: Firebase Auth + JWT
- **DI**: Hilt

## Project Structure

```
app/
├── src/main/
│   ├── java/com/grocerypos/
│   │   ├── ui/
│   │   │   ├── screens/
│   │   │   ├── components/
│   │   │   └── theme/
│   │   ├── data/
│   │   │   ├── local/
│   │   │   ├── remote/
│   │   │   └── repository/
│   │   ├── domain/
│   │   │   ├── models/
│   │   │   └── usecases/
│   │   ├── di/
│   │   └── util/
│   └── res/
├── build.gradle.kts
└── AndroidManifest.xml
```

## Setup

```bash
git checkout android-app
./gradlew clean build
./gradlew assembleDebug
```

## Key Screens

- **Login Screen** - JWT + biometric authentication
- **Dashboard** - Orders and summary
- **Delivery Map** - Real-time GPS tracking
- **Order Details** - Full order information
- **Inventory Check** - Stock verification
- **Reports** - Delivery performance
- **Settings** - Preferences and config

## API Integration

- REST API for data operations
- Firebase Cloud Messaging for push notifications
- Real-time sync with conflict resolution
- Offline queue for pending operations

## Security

- JWT token authentication
- Biometric authentication
- Encrypted local storage
- SSL/TLS communication
- Secure credential storage

## Permissions

- Location (GPS)
- Camera
- Photos
- Internet
- Bluetooth

## Build & Deployment

```bash
# Debug APK
./gradlew assembleDebug

# Release APK (signed)
./gradlew bundleRelease

# Google Play Store
./gradlew bundleRelease
```

## Documentation

See main README.md for complete project documentation.
