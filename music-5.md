# Music 5

Modern iOS music player application built with SwiftUI and MVVM architecture. Supports multi-source music import (Google Drive, Dropbox, Apple Music, WiFi transfer, peer-to-peer), offline playback with a feature-rich audio engine, playlist management, equalizer, text-to-speech, and cloud storage integration.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Language | Swift, SwiftUI |
| Architecture | MVVM + Coordinator pattern |
| Audio Engine | ModernAVPlayer, AVFoundation |
| Reactive | RxSwift, RxCocoa |
| Cloud | GoogleAPIClientForREST (Drive), SwiftyDropbox |
| Networking | Alamofire, AlamofireObjectMapper |
| WiFi Transfer | GCDWebServer (local HTTP server) |
| Image Loading | SDWebImage |
| Auth | GoogleSignIn 6 |
| Monetization | StoreKit (In-App Purchases) |
| Ads | Google Mobile Ads SDK |
| TTS | ElevenLabs API |
| Analytics | Custom TelemetryService |
| Build | Xcode, CocoaPods |

## Architecture Overview

```
MusicApp/
  MusicAppApp.swift               # SwiftUI app entry point

  Core/
    Models/
      MediaItem.swift             # Universal track model
      MusicTrack.swift            # Extended track metadata
      Playlist.swift              # Playlist data model
      ImportJob.swift             # Import task model
      ImportSourceAccount.swift   # Cloud account model
      ProviderAccount.swift       # Storage provider model
      EqualizerPreset.swift       # EQ preset model
      UserSettings.swift          # User preferences model
      TTSAudio.swift              # Text-to-speech audio model
    Audio/
      PlaybackService.swift       # Core playback orchestration
      AudioSessionManager.swift   # AVAudioSession configuration
      BackgroundPlaybackManager.swift  # Background audio handling
      EqualizerManager.swift      # Audio equalizer control
      PlaybackStateStore.swift    # Reactive playback state
    Services/
      LibraryManager.swift        # Central music library CRUD
      PlaylistService.swift       # Playlist management
      ColorThemeManager.swift     # Dynamic UI theming
      StoreKitService.swift       # IAP subscription handling
      SleepManager.swift          # Sleep timer functionality
      DeletedTracksCleanupService.swift  # Storage cleanup
    Navigation/                   # App navigation coordination
    Utilities/
      SourceImageResolver.swift   # Provider icon mapping

  Features/
    Library/
      LibraryViewModel.swift      # Library data and filtering
      LibrarySongsView.swift      # Song list UI
      LibraryCoordinator.swift    # Library navigation
    Player/                       # Full-screen player UI
    Playlists/
      PlaylistManager.swift       # Playlist CRUD operations
      PlaylistViewModel.swift     # Playlist UI state
      Views/                      # Playlist detail, create/edit sheets
    Import/
      Cloud/                      # Cloud storage import
      Dropbox/                    # Dropbox-specific import
      AppleMusic/                 # Apple Music library import
      WiFi/                       # GCDWebServer-based WiFi transfer
      P2P/                        # Peer-to-peer transfer
      Adapters/                   # Import source adapters
      Services/                   # Import orchestration
      Common/                     # Shared import utilities
      UI/                         # Import UI screens
    CloudImport/                  # Cloud import coordination
    Settings/
      SettingsView.swift          # Preferences screen
      AdvancedSettingsView.swift  # Power-user settings
      SleepTimerSettingsView.swift
      ThemeSettingsView.swift
    Subscriptions/
      IAPManager.swift            # In-app purchase handling
    TextToSpeech/                 # ElevenLabs TTS feature
    Testing/                      # Debug and performance test views

  Services/
    ElevenLabsService.swift       # ElevenLabs TTS API client
    FirebaseService.swift         # Firebase integration
    Media/                        # Media file handling
    Notifications/                # Push notification handling
    TelemetryService.swift        # Analytics tracking
    TrackingManager.swift         # User activity tracking

  Player/                         # Legacy UIKit player components
    PlayerViewController.swift    # Full player screen
    NowPlayingController.swift    # Now playing info center
    BottomBar.swift               # Mini player bar
    PlayerRxCommand.swift         # Reactive player commands

  Views/                          # Shared SwiftUI components
  Utilities/                      # General utility helpers
  Resources/                      # Bundled assets and data files
```

## Key Features

- Multi-source music import: Google Drive, Dropbox, Apple Music, WiFi transfer, peer-to-peer
- Offline-first playback with local file storage
- Background audio with lock screen controls and Now Playing integration
- Audio equalizer with customizable presets
- Playlist management (create, edit, reorder, delete)
- Sleep timer with configurable duration
- Text-to-speech via ElevenLabs API
- WiFi file transfer via built-in GCDWebServer HTTP server
- Dynamic color themes
- Subscription model with StoreKit
- Reactive architecture with RxSwift for player state management
- 143+ Swift source files with comprehensive feature coverage

## How It Works

1. **Import**: Users connect cloud storage accounts (Google Drive, Dropbox) via OAuth or import from Apple Music library. WiFi transfer spins up a local GCDWebServer allowing drag-and-drop uploads from any browser on the same network. P2P transfer uses Multipeer Connectivity for device-to-device sharing.
2. **Library Management**: The `LibraryManager` indexes all imported tracks into a unified library. `MediaItem` and `MusicTrack` models normalize metadata regardless of source. The library supports search, sort, and filter operations.
3. **Playback**: `PlaybackService` orchestrates audio through ModernAVPlayer. `AudioSessionManager` handles AVAudioSession category configuration for background playback. `PlaybackStateStore` provides reactive state updates via RxSwift to keep all UI components synchronized.
4. **Player UI**: A hybrid UIKit/SwiftUI player. The `BottomBar` mini-player persists across tabs. Tapping expands to the full `PlayerViewController` with album art, seek bar, equalizer access, and queue management.
5. **Playlists**: `PlaylistService` manages playlist CRUD with local persistence. Users can add songs from the library, reorder tracks, and play entire playlists with shuffle/repeat modes.
6. **Subscriptions**: Premium features (ad removal, unlimited imports, EQ presets) are gated behind StoreKit subscriptions managed by `IAPManager` and `StoreKitService`.
