# 360 Music Pocket

iOS music streaming and download application with multi-source content support. Combines YouTube audio extraction, cloud storage music libraries (Google Drive, Dropbox), local file management, playlist organization, and a full-featured audio player with background playback and popup mini-player.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Language | Swift (UIKit + some SwiftUI) |
| Architecture | MVC with service layer |
| Audio | AVFoundation, custom player engine |
| YouTube | XCDYouTubeKit (video/audio extraction) |
| Cloud Storage | Google Drive API, Dropbox SDK |
| Networking | Alamofire, AlamofireObjectMapper |
| Data | Realm (local persistence) |
| UI Components | LNPopupController (mini player bar) |
| Ads | Google AdMob |
| Auth | Google Sign-In |
| Transcription | WhisperKit (audio transcription) |

## Architecture Overview

```
SwiftyMusicPocket/
  Root/
    SMPRoot/                    # App delegate, tab bar controller setup

  Controllers/
    Browse/                     # Content discovery and browsing
    Search/                     # Audio search (YouTube + local)
    Player/                     # Full-screen player controller
    Playlist/                   # Playlist management
    ServiceCloud/               # Cloud storage browser (Drive, Dropbox)
    Settings/                   # App preferences
    YouTubeKit/                 # YouTube integration controllers
    AudioAnalysis/              # Audio analysis features
    Transcribe/                 # Audio-to-text transcription

  Model/
    Local/                      # Realm models for local tracks, playlists
    Remote/                     # API response models
    Search2/                    # Search result models

  View/
    Cell/                       # Table/collection view cells
    Popup/                      # Popup views and dialogs
    Settings/                   # Settings UI components
    Storyboard/                 # Interface Builder files
    SubView/                    # Reusable sub-components

  Network/
    RootAPI.swift               # Base API client
    RootAPI+Audio+Search.swift  # Audio search API extensions

  Services/                     # Audio recording, WhisperKit, etc.
  Utils/                        # TextToSpeech, language utilities
  Extensions/                   # UIKit + Foundation extensions
  Libs/
    HamadoLib/                  # Shared utility library

  LNPopupController/            # Mini player popup bar framework
  XCDYouTubeKit/                # YouTube video extraction library
```

## Key Features

- YouTube audio streaming and download with background playback
- Google Drive and Dropbox music library integration
- Full-featured audio player with equalizer support
- Mini player bar (popup controller) with swipe-to-expand
- Playlist creation and management with Realm persistence
- Audio search across multiple sources
- Offline playback for downloaded tracks
- Audio transcription via WhisperKit
- Text-to-speech functionality
- Background audio playback with lock screen controls
- Google Sign-In for cloud storage authentication

## How It Works

1. **Content Discovery**: Users browse or search for audio content. Search queries are routed to YouTube via XCDYouTubeKit and to local storage simultaneously. Results are displayed in a unified list.
2. **Cloud Import**: Users authenticate with Google Drive or Dropbox via OAuth. The ServiceCloud controller browses their cloud folders and allows streaming or downloading audio files to local storage.
3. **Playback Engine**: Selected tracks are loaded into the AVFoundation-based player. The `LNPopupController` provides a persistent mini-player bar at the bottom of the tab bar that expands into a full-screen player with playback controls, seek bar, and track info.
4. **Playlist Management**: Tracks are organized into playlists stored in Realm. Users can create, edit, reorder, and delete playlists. Playing a playlist queues all its tracks sequentially.
5. **Offline Mode**: Downloaded tracks are stored locally and accessible without an internet connection. The app manages local file storage and metadata in Realm.
6. **Background Audio**: The app registers for background audio capabilities, allowing continuous playback when the app is minimized, with lock screen and control center integration.
