# 360 Music Pocket

An original iOS music streaming and offline download app built with Swift and UIKit. Features search-based music discovery, background audio playback with lock screen controls, playlist management, cloud storage integration, and a full offline download manager.

## Tech Stack

- **Platform:** iOS (Swift 5, UIKit, Storyboards)
- **Architecture:** MVC with singleton services
- **Networking:** Alamofire + AlamofireObjectMapper for typed API responses
- **Persistence:** Realm (playlists, recent keywords, song items)
- **Audio Player:** ModernAVPlayer with MPRemoteCommandCenter integration
- **Download Manager:** MZDownloadManager (background URLSession downloads)
- **Cloud Storage:** Dropbox, OneDrive, Yandex.Disk, Box integration
- **UI Libraries:** Custom FadingLayout, CardView, ViewDeck (side menu)
- **Media Sources:** Custom API + ZingMp3 search integration

## Architecture Overview

```
SwiftyMusicPocket/
  ViewController.swift                    # Root view controller
  Network/
    RootAPI.swift                         # Singleton API client with generic response handling
    RootAPI+Audio+Search.swift            # Search and audio stream endpoints
  Model/
    Local/
      SongItem.swift                      # Realm Object - downloaded/cached song
      PlaylistItem.swift                  # Realm Object - user playlist
      RecentKeyword.swift                 # Realm Object - search history
    Remote/
      GenericResponse.swift               # Generic API response wrapper
      Search/
        Docs.swift, Response.swift        # Search result models
        ZingMp3Response.swift             # ZingMp3 API response model
        SearchMetadata.swift, Params.swift
      CategoryMetadata/
        Metadata.swift, SongEnvator.swift # Category/browse models
        FilesDoc.swift                    # Cloud file document model
  Controllers/
    Player/
      PlayerViewController+Core.swift    # AVPlayer integration and UI
      PlayerViewController+Rx.swift      # Reactive playback bindings
      PlayerCommand.swift                # MPRemoteCommandCenter (play/pause/next/prev/seek)
      SwiftyPlayerConfiguration.swift    # Player configuration
    Browse/
      DownloadManager.swift              # Background download queue with progress tracking
    Playlist/
      ChooseSongsViewController.swift    # Song selection for playlists
    ServiceCloud/
      CloudServiceController.swift       # Base cloud service controller
      ServiceCloudBoxController.swift    # Box integration
      ServiceCloudYandexDiskController.swift  # Yandex.Disk integration
      LibraryImportController.swift      # iOS Music Library import
    Settings/
      FeedBackViewController.swift       # User feedback
      MoreAppSettingViewController.swift  # Settings and related apps
  Root/
    RootNavigationController.swift       # Custom navigation controller
    RootNavigationHome.swift             # Home navigation setup
    RootViewDeckController.swift         # Side menu (ViewDeck)
    SMPRoot/                             # Tab-based root controllers
      RootAllTracksNavigationController.swift
      RootBrowseNavigationController.swift
      RootPlaylistRootController.swift
      RootSearchNavigationController.swift
      RootSettingNavigationController.swift
  Utilities/
    RootAuthManager.swift                # Authentication management
    RootShareManager.swift               # Share sheet integration
    RootColor.swift, RootString.swift    # App constants
  View/
    Cell/                                # Table/collection view cells
    SubView/                             # Gradient views, edit bottom bar
  Extensions/                            # 20+ extensions (UIView, String, Date, Realm, etc.)
  Libs/HamadoLib/                        # Custom layout library (FadingLayout, CardView)
```

## Key Features

- **Music Search:** Multi-source search with recent keyword history stored in Realm. Results include song metadata, cover art, and streaming URLs.
- **Background Audio Playback:** Full lock screen / Control Center integration via ModernAVPlayer and MPRemoteCommandCenter. Supports play, pause, next, previous, and seek commands.
- **Offline Downloads:** Background download queue powered by MZDownloadManager. Progress tracking per song with delegate callbacks. Files stored in the Documents/Music directory.
- **Playlist Management:** Create, edit, and reorder playlists. Songs stored as Realm objects with metadata.
- **Cloud Storage Integration:** Import music files from Dropbox, OneDrive, Yandex.Disk, and Box.
- **iOS Music Library Import:** Access and import songs from the user's local music library.
- **Tab-Based Navigation:** Five-tab layout: Browse, Search, All Tracks, Playlists, Settings. Side menu via ViewDeck.

## How It Works

1. **Search and Browse:** The user searches for music via `RootAPI`, which queries a remote server and returns typed results through AlamofireObjectMapper. Results are displayed in table views with cover art loaded asynchronously.
2. **Streaming:** Selecting a track initializes ModernAVPlayer with the stream URL. The `PlayerCommand` class registers remote commands (play, pause, skip, seek) with `MPRemoteCommandCenter` for lock screen and AirPlay control.
3. **Download:** The `DownloadManager` singleton manages a background URLSession download queue. Each download tracks progress via `MZDownloadModel` and persists the completed file as a `SongItem` in Realm.
4. **Cloud Import:** Cloud service controllers authenticate with third-party APIs (Dropbox, OneDrive, etc.), browse remote file structures, and download selected files to local storage for offline playback.
