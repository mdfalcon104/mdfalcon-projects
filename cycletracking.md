# Cycle Tracking

iOS health tracking app built with Swift for menstrual cycle prediction, symptom logging, and data visualization. Features Firebase cloud sync, reminder notifications, and charting for cycle pattern analysis.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Language | Swift |
| UI | UIKit, SwiftUI (hybrid) |
| Networking | Alamofire 4.8, AlamofireObjectMapper |
| Ads | Google Mobile Ads SDK |
| Shared | hmdcommonkit (common utilities) |
| Analytics | Firebase |
| Dependencies | CocoaPods |
| Platform | iOS 15.6+ |

## Architecture Overview

```
CycleTracking/
  App/                          # App entry point and configuration
  Features/                     # Feature modules
  Extensions/                   # Swift extensions
  Assets.xcassets/              # App icons and images
  Config.h                      # Objective-C configuration header
  CC-Bridging-Header.h          # Swift-ObjC bridging header
  GoogleService-Info.plist       # Firebase configuration
  HMDConfig.plist               # HMD common kit configuration

firebase.json                   # Firebase project configuration
icon.sketch                     # App icon design file
public/                         # Firebase hosting assets
```

## Key Features

- Menstrual cycle tracking with period start/end logging
- Cycle prediction based on historical pattern analysis
- Symptom logging (mood, pain, energy, flow intensity)
- Reminder notifications for upcoming periods
- Data visualization with cycle history charts
- Firebase cloud sync for data backup
- AdMob monetization with banner and interstitial ads
- Privacy-focused local data storage

## How It Works

1. **Cycle Logging**: Users log period start and end dates. The app calculates cycle length averages from historical data.
2. **Prediction**: Based on recorded cycle patterns, the app predicts upcoming period dates, fertile windows, and ovulation estimates.
3. **Symptom Tracking**: Daily symptoms can be logged with intensity levels. The app correlates symptoms with cycle phases.
4. **Notifications**: Configurable reminders alert users before predicted period start dates.
5. **Data Sync**: Firebase handles cloud backup and cross-device synchronization of cycle data.
