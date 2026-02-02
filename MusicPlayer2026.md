# MusicPlayer2026

**Modern iOS music streaming app with Audiomack integration, YouTube audio extraction, and offline playback.**

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Language | Swift 5 (SwiftUI + UIKit hybrid) |
| Architecture | MVVM with UIKit TabBarController hosting SwiftUI views |
| Audio Playback | ModernAVPlayer2 (with RxSwift reactive bindings) |
| Networking | Alamofire + AlamofireObjectMapper |
| Local Storage | RealmSwift (songs, playlists, search history) |
| Image Loading | SDWebImage + SDWebImageSwiftUI |
| Download | MZDownloadManager2 |
| Ads | Google Mobile Ads SDK |
| Analytics | Firebase Analytics |
| Shared Kit | hmdcommonkit_2025 (review prompts, ad management) |

## Architecture Overview

```
MusicPlayer2028/
├── main.swift                   # App entry point
├── MusicPlayer2028App.swift     # SwiftUI App lifecycle
├── ContentView.swift            # Custom UITabBarController with 4 tabs
├── Core/
│   ├── Extensions/              # Swift extensions
│   └── Utilities/
│       ├── AudiomackAPIClient.swift    # Audiomack REST API client (JWT-authenticated)
│       ├── AudioMackylyPlayerConfiguration.swift  # Player config and setup
│       └── SongMenuModifier.swift      # Context menu actions for songs
├── Managers/
│   ├── AudioPlayerManager.swift        # Core audio engine (ModernAVPlayer2)
│   ├── AudioPlayerManager+Ex.swift     # Player extensions (queue, shuffle, repeat)
│   ├── DataManager.swift               # Realm database operations
│   ├── DownloadManager.swift           # Background download management
│   ├── ConfigManager.swift             # Remote configuration
│   ├── AdSizeManager.swift             # Ad sizing and display logic
│   └── TrackingPopup.swift             # ATT tracking prompt
├── Models/
│   ├── Song.swift                      # Core song model (id, title, artist, URLs)
│   ├── Playlist.swift                  # Playlist model
│   ├── StoredSong.swift                # Realm-persisted song
│   ├── StoredPlaylist.swift            # Realm-persisted playlist
│   └── SearchHistory.swift             # Recent search terms
├── Views/
│   ├── HomeScreen/
│   │   ├── ScreenDLHomeScreen.swift    # Home with hero section, popular songs, genres
│   │   ├── HeroSectionView.swift       # Featured content carousel
│   │   ├── PopularSongCard.swift       # Song card component
│   │   └── GenreTabButton.swift        # Genre filter tabs
│   ├── SearchScreen/                   # Music search interface
│   ├── PlayerScreen/
│   │   ├── ScreenDLPlayerScreen.swift  # Full-screen now-playing
│   │   ├── PlayerVideoArea.swift       # Album art / video display
│   │   ├── PlayerSongInfoView.swift    # Song title and artist
│   │   ├── PlayerProgressBar.swift     # Seek bar
│   │   ├── PlayerPlaybackControls.swift# Play/pause, skip, shuffle
│   │   └── PlayerBottomActions.swift   # Like, add to playlist, share
│   ├── LibraryScreen/                  # Offline library and playlists
│   ├── PlaylistDetailScreen/           # Playlist song list
│   ├── QueuePlayerScreen/              # Current playback queue
│   ├── AddToPlaylistScreen/            # Add song to playlist
│   ├── UpgradeScreen/                  # Premium subscription
│   ├── SettingScreen/                  # App settings
│   ├── SelectLanguageScreen/           # Language selection
│   └── Components/                     # Shared UI components
└── Utils/
    └── YouTubeKit/                     # YouTube audio stream extraction
        ├── YouTube.swift               # Main extraction API
        ├── InnerTube.swift             # YouTube InnerTube client
        ├── Cipher.swift                # Stream URL decryption
        ├── Parser.swift                # Response parsing
        ├── SignatureSolver.swift        # Signature challenge solver
        ├── Extraction.swift            # Stream extraction logic
        ├── Smart/                      # Remote-configured extraction
        │   ├── RemoteSmartClient.swift # Remote config client
        │   └── Models/                 # SmartConfig, TokenClaims, JobModels
        ├── Models/                     # Stream, ITag, Codecs, FileExtension
        ├── Extensions/                 # Concurrency, regex, WebSocket, retry
        └── Remote/Models/              # Remote stream models
```

## Key Features

- **Audiomack Streaming** -- `AudiomackAPIClient` communicates with a JWT-authenticated backend API to fetch trending songs, top charts, search results, and song stream URLs.
- **YouTube Audio Extraction** -- full `YouTubeKit` implementation with InnerTube API, cipher decryption, and signature solving for extracting audio streams from YouTube videos.
- **Full-Featured Player** -- `AudioPlayerManager` wraps ModernAVPlayer2 with queue management, shuffle, repeat (one/all), seek, and background playback. The player UI includes progress bar, controls, and album art.
- **Offline Library** -- download songs for offline playback via `DownloadManager`; persisted to Realm (StoredSong, StoredPlaylist).
- **Home Discovery** -- hero carousel, popular song cards, and genre-based browsing.
- **Search** -- search with history tracking (Realm-backed `SearchHistory`).
- **Playlist Management** -- create, edit, and manage playlists; add songs from any screen.
- **Ad Monetization** -- Google Mobile Ads with configurable ad sizing via `AdSizeManager`.
- **Custom Tab Bar** -- `CustomTabBarController` with selected-state highlighting (rounded background, tint colors).

## How It Works

1. The app launches with a custom `UITabBarController` hosting 4 tabs: Home, Search, Library, and Upgrade -- each tab wraps a SwiftUI view via `UIHostingController`.
2. **Home**: `ScreenDLHomeScreen` loads trending and popular songs from the Audiomack backend API. Songs are displayed in a hero carousel and categorized card grids.
3. **Search**: Users search for music; results come from the backend API. Recent keywords are saved to Realm.
4. **Playback**: Tapping a song initializes `AudioPlayerManager`, which resolves the stream URL (from Audiomack API or YouTube extraction) and begins playback via ModernAVPlayer2. The full-screen player shows album art, progress, and controls.
5. **Downloads**: Users can download songs for offline listening. `DownloadManager` handles background downloads; completed tracks are stored as `StoredSong` in Realm.
6. **YouTube**: When a song has a `videoId`, `YouTubeKit` extracts the audio stream URL using InnerTube API + cipher decryption, enabling playback without a YouTube player.
7. Ads are shown between interactions, managed by `hmdcommonkit_2025` with configurable frequency and the `AdSizeManager`.
