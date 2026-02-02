# Katrina International

iOS music licensing and international distribution app built with Swift and UIKit. Features authorization management, artist profiles, music download management, audio playback, and content delivery for the global music market.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Language | Swift, Objective-C |
| UI | UIKit, Storyboard |
| Audio | ModernAVPlayer (via hmdaudioplayerkit) |
| Database | RealmSwift (~10.54.2), SQLite.swift |
| Networking | Alamofire (via shared pods) |
| Navigation | ViewDeck (side menu) |
| Download | MZDownloadManager2 |
| Auth | SwiftJWT |
| Ads | Google Mobile Ads (via hmdcommonkit) |
| Dependencies | CocoaPods |
| Platform | iOS 13.0+ |

## Architecture Overview

```
katrina_international/
  AppDelegate.swift              # App lifecycle management
  AppState.swift                 # Global application state
  ViewController.swift           # Main view controller
  Root/                          # Root navigation controllers
  Controller/                    # Feature view controllers
  View/                          # Custom UIKit views
  Model/                         # Data models (tracks, artists, licenses)
  Extensions/                    # Swift extensions
  HMDextension/                  # HMD framework extensions
  KTExtensionsController/        # Katrina-specific controller extensions
  Libs/                          # Embedded libraries
  Utilities/                     # Helper classes
  SupportFiles/                  # Configuration and support files
  Assets.xcassets/               # App icons and images

html/                            # Web-based content pages
```

## Key Features

- Music licensing management with authorization controls
- Artist profile browsing and content discovery
- Audio streaming with ModernAVPlayer playback engine
- Background music download via MZDownloadManager2
- Realm database for offline music library persistence
- JWT-based authentication for secure API access
- Side menu navigation with ViewDeck
- ESTMusicIndicator for playback status visualization
- Tag-based music categorization (TagListView)
- Cross-promotion with other music apps
- Pull-to-refresh content updates

## How It Works

1. **Authentication**: Users authenticate with JWT tokens for secure access to licensed music content.
2. **Browse**: Artist profiles and music catalogs are fetched from the API and displayed with categorized browsing and search.
3. **Download**: MZDownloadManager2 handles background downloads with progress tracking. Downloaded tracks are stored locally and indexed in Realm.
4. **Playback**: ModernAVPlayer (via hmdaudioplayerkit) manages audio streaming and local playback with background mode support and lock screen controls.
5. **Licensing**: Authorization management ensures users only access music they have valid licenses for, with license status tracked per track and artist.
