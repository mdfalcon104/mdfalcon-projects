# English Grammar 2025

SwiftUI-based English grammar learning app for Vietnamese learners with structured lessons, interactive exercises, progress tracking, and offline content stored in a Realm database.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Language | Swift |
| UI | SwiftUI |
| Architecture | MVVM |
| Database | RealmSwift (~20.0.3) |
| Backend | Firebase (Core, Firestore, Auth, Crashlytics) |
| Networking | Alamofire 4.8, AlamofireObjectMapper |
| Ads | Google Mobile Ads SDK (~11.13.0) |
| Shared | hmdcommonkit_2025 |
| In-App Purchase | StoreKit |
| Dependencies | CocoaPods |
| Platform | iOS 15.6+ |

## Architecture Overview

```
nguphaptienganh2025/
  nguphaptienganh2025App.swift   # SwiftUI app entry point
  ContentView.swift              # Root content view
  MainTabView.swift              # Tab-based main navigation
  Models/                        # Data models (lessons, exercises, progress)
  Views/                         # SwiftUI views organized by feature
  Services/                      # Business logic and data services
  Extensions/                    # Swift extensions
  Resources/                     # Bundled content and assets
  dict_hh.db                     # Offline dictionary database
  Assets.xcassets/               # App icons and images
  GoogleService-Info.plist       # Firebase configuration
  HMDConfig.plist                # HMD common kit configuration
  NguPhapTiengAnhStore.storekit  # StoreKit configuration

AI/                              # AI-assisted content generation
html/                            # Web-based lesson content
```

## Key Features

- Structured English grammar lessons organized by difficulty level
- Interactive exercises with immediate feedback
- Progress tracking per lesson and overall completion
- Offline dictionary database for vocabulary lookup
- Firebase Firestore for user data persistence and cloud sync
- Firebase Auth for user account management
- Realm database for offline lesson content storage
- StoreKit in-app purchases for premium lessons
- AdMob integration for free-tier monetization
- Vietnamese language interface for native learners

## How It Works

1. **Content Loading**: Grammar lessons are stored in the bundled Realm database for offline access. The app syncs with Firebase Firestore for progress data.
2. **Lesson Flow**: Users navigate through grammar topics organized by categories (tenses, articles, prepositions). Each lesson includes explanations with Vietnamese translations and example sentences.
3. **Exercises**: Interactive exercises test comprehension with multiple choice, fill-in-the-blank, and sentence construction tasks. Answers are validated with detailed feedback.
4. **Progress Tracking**: Completion status, scores, and weak areas are tracked per lesson and synced to Firebase for cross-device access.
5. **Dictionary**: The embedded dict_hh.db provides instant offline lookup for vocabulary encountered in lessons.
