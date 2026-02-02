# Mackyplayer

Cross-platform audio player built with Flutter featuring Riverpod state management, just_audio playback engine, Hive local storage, Firebase analytics and crashlytics, Google Mobile Ads monetization, and in-app purchases.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Framework | Flutter (Dart SDK ^3.9.2) |
| State Management | flutter_riverpod, hooks_riverpod |
| Audio | just_audio, audio_service |
| Storage | Hive, hive_flutter |
| Navigation | go_router |
| Networking | http, Dio |
| In-App Purchase | in_app_purchase |
| Ads | google_mobile_ads |
| Firebase | firebase_core, firebase_analytics, firebase_crashlytics |
| Security | flutter_secure_storage, encrypt, crypto |
| Images | cached_network_image |
| UI | flutter_hooks, audio_video_progress_bar |
| Localization | flutter_localizations, intl |
| Platform | iOS, Android |

## Architecture Overview

```
lib/
  main.dart                     # App bootstrap and ProviderScope
  models/                       # Data models (tracks, playlists, albums)
  providers/                    # Riverpod providers
  screens/                      # App screens
  services/                     # Business logic services
  utils/                        # Helper utilities
  widgets/                      # Reusable UI widgets
  l10n/                         # Localization files

android/                        # Android platform code
ios/                            # iOS platform code
```

## Key Features

- Audio streaming with just_audio engine and background playback via audio_service
- Riverpod state management with hooks for reactive UI updates
- Hive local database for offline playlist and track persistence
- Audio progress bar with seek and duration display
- Cached network images for album artwork
- go_router navigation with deep linking support
- Firebase Analytics for usage tracking
- Firebase Crashlytics for crash reporting
- Google Mobile Ads (banner and interstitial)
- In-app purchases for premium features
- Encrypted secure storage for sensitive data
- Multi-language support via flutter_localizations
- App review prompt integration

## How It Works

1. **Startup**: The app initializes Firebase services, Hive storage, and wraps the widget tree in a Riverpod ProviderScope for global state management.
2. **Browse**: Users explore music through screens displaying playlists, albums, and tracks. Data is fetched via HTTP/Dio and cached locally in Hive.
3. **Playback**: just_audio handles audio streaming with background mode support through audio_service. The progress bar widget displays real-time position with seek controls.
4. **State**: Riverpod providers manage player state, playlist state, and user preferences. Flutter hooks enable reactive UI updates without boilerplate.
5. **Monetization**: Google Mobile Ads displays banner and interstitial ads. In-app purchases allow upgrading to an ad-free experience with additional features.
