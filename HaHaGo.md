# HaHaGo

**iOS entertainment app with gamification -- a TikTok-style vertical video feed for fun, short-form content.**

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Language | Swift 5, SwiftUI |
| Architecture | MVVM with Services layer |
| Backend | Firebase (Firestore, Analytics, Remote Config, App Check, Cloud Messaging) |
| Ads | Google Mobile Ads SDK (AdMob banners, interstitials, app-open ads) |
| Auth | Firebase Auth with email verification |
| Monetization | StoreKit 2 (In-App Purchases, subscriptions) |
| Networking | URLSession with configurable URLCache (50 MB memory / 500 MB disk) |

## Architecture Overview

```
HaHaGo/
├── HaHaGoApp.swift              # App entry point, Firebase init, ad lifecycle
├── ContentView.swift            # Root navigation
├── Features/
│   ├── Feed/                    # Vertical video feed (TikTok-style)
│   │   ├── ViewModels/          # FeedViewModel, PlaybackStateViewModel, ReactionHandler
│   │   └── Views/               # StoryPageView, PlaybackControlsView, AdViews
│   ├── Auth/                    # Sign-in, profile editing, verification
│   │   ├── ViewModels/          # AuthViewModel, ProfileEditViewModel, RateAppHandler
│   │   └── Views/               # AuthView, SidebarView, AuthGate
│   ├── Comments/                # Comment panel and sharing
│   ├── Settings/                # Category filters, playback, theme settings
│   └── IAP/                     # In-app purchase products and UI
├── Models/
│   ├── Story.swift              # Core data model (video, thumbnail, publisher, metadata)
│   ├── UserProfile.swift        # User profile model
│   └── Settings.swift           # App settings model
├── Services/
│   ├── FeedService.swift        # Paginated story feed from remote API
│   ├── AuthService.swift        # Firebase Auth integration
│   ├── FirestoreService.swift   # Firestore read/write
│   ├── PlaybackService.swift    # Video playback management
│   ├── AdService.swift          # Ad loading and display
│   ├── AppOpenAdManager.swift   # App-open ad lifecycle
│   ├── RemoteConfigService.swift# Dynamic config from Firebase Remote Config
│   ├── NetworkMonitor.swift     # Connectivity monitoring
│   └── VerificationManager.swift# Email verification flow
├── Theme/                       # AppTheme, SemanticColors, AnimationTokens, ThemeManager
├── Components/                  # SplashScreenView, SafariWebView, Notifications
└── Utils/                       # Logging (OSLog), PerformanceMetrics, ReportedStoriesCache
```

## Key Features

- **Vertical Video Feed** -- infinite-scroll, cursor-based pagination fetching stories from a remote API; supports thumbnail previews, playback overlays, and publisher headers.
- **Reaction System** -- users react to stories (funny/sad); reactions are persisted via Firestore.
- **Comments Panel** -- threaded comments with share functionality.
- **Authentication** -- Firebase Auth with email verification, profile editing, and account deletion.
- **In-App Purchases** -- StoreKit 2 subscriptions to remove ads and unlock premium content.
- **Ad Monetization** -- AdMob banners, interstitials on tab switches, and app-open ads on launch.
- **Remote Config** -- base URL, cookie, and feature flags fetched dynamically from Firebase Remote Config.
- **Theming** -- light/dark mode with semantic color tokens and animation tokens.
- **Content Moderation** -- report stories, cache reported IDs locally, refresh from server on app activation.

## How It Works

1. On launch, Firebase is configured, analytics enabled, and an app-open ad is preloaded.
2. `FeedService` fetches a paginated list of `Story` objects (video URL, thumbnail, publisher info) from a remote content API, using cursor-based pagination.
3. Stories are displayed in a full-screen vertical pager (`StoryPageView`); playback is managed by `PlaybackStateViewModel`.
4. Users can react, comment, share, and report content. Auth-gated actions require sign-in via `AuthGate`.
5. `RemoteConfigService` dynamically updates the feed endpoint URL and feature flags without app updates.
6. Ad placements (banners in feed, interstitials on tab switches, app-open on launch) are orchestrated by `AdService` and `AppOpenAdManager`, with launch-count gating.
7. Premium users can remove ads via StoreKit 2 subscriptions managed by `IAPManager`.
