# Traffic Violation Lookup (Tra Cuu Vi Pham)

SwiftUI iOS app for looking up traffic violation fines, violation history, and payment status for Vietnamese motorists. Features real-time fine checking via REST API, App Intents for Siri integration, and AdMob monetization.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Language | Swift |
| UI | SwiftUI |
| Architecture | MVVM |
| Ads | Google Mobile Ads SDK (~12.12.0) |
| Shared | hmdcommonkit_2025 |
| In-App Purchase | StoreKit |
| Dependencies | CocoaPods |
| Platform | iOS 16.0+ |

## Architecture Overview

```
TraPhatNguoi/
  TraPhatNguoiApp.swift         # SwiftUI app entry point
  ContentView.swift             # Root content view
  LookupType.swift              # Violation lookup type definitions
  AdSizeManager.swift           # Ad banner size management
  AppIntents.swift              # Siri and Shortcuts integration
  Models/                       # Data models (violations, vehicles, fines)
  Views/                        # SwiftUI views
  Services/                     # API service layer
  Extensions/                   # Swift extensions
  Assets.xcassets/              # App icons and images
  GoogleService-Info.plist      # Firebase configuration
  HMDConfig.plist               # HMD common kit configuration
  IAP.storekit                  # StoreKit configuration

website/                        # Promotional web page
screenshots/                    # App Store screenshot assets
```

## Key Features

- Traffic violation lookup by license plate number
- Real-time fine status checking via government REST API
- Violation history display with fine amounts and dates
- Payment status tracking for outstanding fines
- Multiple vehicle type support (car, motorcycle)
- App Intents for Siri voice queries and Shortcuts automation
- AdMob banner and interstitial ad integration
- StoreKit in-app purchases for ad-free experience
- Clean SwiftUI interface with Vietnamese localization

## How It Works

1. **Input**: Users enter their vehicle license plate number and select the vehicle type.
2. **API Query**: The app queries the traffic violation REST API with the plate number to fetch violation records.
3. **Results Display**: Violations are displayed in a list showing violation type, date, location, fine amount, and payment status.
4. **Siri Integration**: App Intents allow users to check violations via Siri voice commands or Shortcuts automations.
