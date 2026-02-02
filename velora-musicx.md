# Velora MusicX

iOS music streaming app built with Swift featuring curated playlists, background audio playback with ModernAVPlayer, offline downloads, Realm-based library management, and RxSwift reactive event handling.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Language | Swift |
| UI | UIKit, SwiftUI (hybrid) |
| Audio | ModernAVPlayer2, AVFoundation |
| Reactive | RxSwift (via ModernAVPlayer2/RxSwift) |
| Events | EventSubscriber |
| Database | RealmSwift (~10.54.0) |
| Ads | Google Mobile Ads SDK (~12.12.0) |
| Shared | hmdcommonkit_2025 |
| Dependencies | CocoaPods |
| Platform | iOS 16.0+ |

## Architecture Overview

```
VeloraMusicX/
  App/                          # App entry point and configuration
  Features/
    Favorites/                  # Favorited tracks management
    Player/                     # Audio player interface
    Playlists/                  # Playlist browsing and management
    Search/                     # Music search functionality
    Settings/                   # App settings and preferences
    Splash/                     # Launch screen
    TopCharts/                  # Top charts and trending music
  Models/                       # Data models (tracks, playlists, artists)
  Dependencies/                 # Dependency injection and service locators
  Shared/                       # Shared utilities and components
  Resources/                    # Assets, fonts, localization

website/                        # Promotional web page
resources/                      # Additional resource files
```

## Key Features

- Music streaming with background audio playback
- ModernAVPlayer2 audio engine with RxSwift reactive state
- Curated playlists and top charts browsing
- Search functionality across music catalog
- Favorites management with Realm persistence
- Offline download support for local playback
- Cloud library management across devices
- AdMob monetization with ad integration
- Feature-based modular architecture

## How It Works

1. **Browse**: Users explore music through top charts, curated playlists, or search. Content is fetched from remote APIs and cached locally.
2. **Playback**: Audio streaming is handled by ModernAVPlayer2 with background mode support. RxSwift observables manage player state (playing, paused, buffering) and UI updates reactively.
3. **Library**: Users add tracks to favorites, which are persisted in Realm for offline access. Downloaded tracks are stored locally for offline playback.
4. **Events**: EventSubscriber handles cross-feature communication (e.g., player state changes updating the mini-player across all screens).
