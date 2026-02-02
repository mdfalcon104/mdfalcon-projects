# AngleMusic

**iOS music app combining music search/download with AI-powered music generation.**

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Language | Swift 5 (UIKit + SwiftUI hybrid) |
| Architecture | MVC + MVVM (SwiftUI views with ObservableObject ViewModels) |
| Audio Playback | ModernAVPlayer (with RxSwift reactive bindings) |
| Networking | Alamofire + AlamofireObjectMapper |
| Local Storage | Realm (songs, playlists, recent keywords) |
| Download | MZDownloadManager2 |
| UI | Storyboards + SwiftUI, IQKeyboardManager, NVActivityIndicatorView |
| Ads | Google Mobile Ads SDK |
| Auth / Security | SwiftJWT, SwiftKeychainWrapper, AES encryption |
| Monetization | StoreKit (IAP subscriptions) |

## Architecture Overview

```
MusicDownloadGenerationAI/
├── Root/
│   ├── MDGARoot/                # App delegate, root navigation
│   └── SwiftUIs/                # SwiftUI entry points bridged into UIKit
├── Controllers/
│   ├── AIMusic/                 # AI music generation UI
│   │   ├── AIMusicView.swift    # Main generation screen (lyrics, style, genre, mood)
│   │   ├── ProcessingStatusView # Real-time generation progress
│   │   └── TopMediaService.swift# API client for AI music generation backend
│   ├── Search/                  # Music search (ZingMp3, web sources)
│   ├── Player/                  # Full-screen audio player
│   ├── MyAudio/                 # Downloaded music library
│   ├── Browser/                 # In-app web browser for sources
│   ├── Setting/                 # App settings
│   └── YouTubeKit/              # YouTube audio extraction toolkit
│       ├── InnerTube.swift      # YouTube InnerTube API client
│       ├── Cipher.swift         # Stream URL decryption
│       └── Smart/               # Remote configuration for extraction
├── Models/
│   ├── MusicAIViewModel.swift   # AI generation state (lyrics, style, instruments, moods)
│   ├── SearchViewModel.swift    # Search state and results
│   ├── AudioViewModel.swift     # Audio playback state
│   ├── PopularViewModel.swift   # Popular/trending music
│   ├── Subscriptions/           # IAP manager, product definitions
│   └── StyleModel.swift         # AI music style configuration
├── NetworkModels/
│   ├── Local/                   # Realm models (SongItem, PlaylistItem, RecentKeyword)
│   └── Remote/                  # API response models (Search, CategoryMetadata)
├── Managers/
│   └── SwiftUIIDownloadManager.swift  # Download queue management
├── Services/                    # Network and business logic services
├── Views/
│   ├── Sections/                # Reusable section views
│   ├── Subscription/            # Premium subscription UI
│   └── Components/              # Shared UI components
├── Components/
│   ├── Cells/                   # Table/collection view cells
│   └── Subviews/                # Reusable subviews
├── Extensions/                  # 15+ extensions (Color, UIView, String, Date, etc.)
├── Utilities/                   # RootColor, AESUtils, RootCloudServiceManager
└── SupportFiles/                # Plist, entitlements, launch assets
```

## Key Features

- **AI Music Generation** -- users compose lyrics, select style/genre/mood/instruments, and generate original songs via a backend AI service; real-time progress tracking during generation.
- **Music Search & Download** -- search across multiple sources; download tracks for offline listening with queue management.
- **Audio Player** -- full-featured player with shuffle, repeat modes, playlist support, and a mini bar player.
- **YouTube Audio Extraction** -- built-in YouTubeKit for extracting audio streams from YouTube videos.
- **Local Library** -- Realm-backed offline library with playlists, favorites, and recent search history.
- **Subscription Model** -- StoreKit IAP for premium features (more AI generations, ad removal).

## How It Works

1. The app launches into a tab-based interface (Home, Search, My Audio, Settings) built with UIKit hosting SwiftUI views.
2. **AI Music Flow**: Users enter lyrics (or use AI-generated lyrics via prompt), configure style tags (genre, mood, instruments, gender), then submit to the backend. `MusicAIViewModel` manages state; `TopMediaService` handles API calls. A `ProcessingStatusView` shows real-time generation progress.
3. **Search Flow**: `SearchViewModel` queries music APIs; results are displayed in scrollable lists with download buttons. `MZDownloadManager2` handles background downloads, persisting tracks to Realm via `DatabaseManager`.
4. **Playback**: `ModernAVPlayer` with RxSwift bindings drives the audio player. Supports streaming and local playback, with shuffle/repeat and queue management.
5. **YouTube Extraction**: `YouTubeKit` uses InnerTube API + cipher decryption to extract audio-only streams from YouTube URLs for playback.
