# TTSMaker

A native iOS text-to-speech application built with SwiftUI and MVVM architecture. Supports 100+ voices across multiple languages, document import (PDF, DOCX, web pages), OCR scanning, and background audio playback with a full-featured task player.

## Tech Stack

- **Platform:** iOS 15+ (SwiftUI)
- **Architecture:** MVVM with Combine reactive bindings
- **Persistence:** Realm (schema-versioned migrations)
- **Networking:** URLSession with configurable URLCache (50MB memory / 500MB disk)
- **AI Integration:** OpenAI API for document analysis and voice recommendations
- **Auth:** Custom AuthManager with API key management
- **Monetization:** StoreKit (IAP), Google AdMob via hmdcommonkit
- **Analytics:** Firebase Analytics
- **Document Parsing:** VisionKit (OCR), custom DOCX/webpage parsers

## Architecture Overview

```
TTSMaker/
  TTSMakerApp.swift              # @main entry, AppDelegate, Firebase + AdMob init
  Config/
    Config.swift                 # API endpoints, feature flags
  Models/
    Voice.swift                  # Voice model (name, gender, locale, codec, usage stats)
    TTSTask.swift                # Realm Object - TTS job with status tracking
    TextFile.swift               # Imported document model
    FavoriteVoice.swift          # Realm Object - favorited voice names
    RealmVoice.swift             # Cached voice data in Realm
    VoicesResponse.swift         # API response model
    VoiceExample.swift           # Voice preview audio model
  ViewModels/
    HomeViewModel.swift          # Voice browsing, search, filtering (gender/locale/status)
    StudioViewModel.swift        # File management and creation workflows
    TypeOrPasteTextViewModel.swift  # Text editor with TTS playback controls
    TaskPlayerViewModel.swift    # Audio player with progress, speed, pitch controls
    ImportedDocumentViewModel.swift # PDF/DOCX import and text extraction
    WebpagesViewModel.swift      # Web page content extraction
    ScannedDocumentViewModel.swift  # VisionKit OCR integration
    ListOfPhrasesViewModel.swift # Phrase list management
    SettingsViewModel.swift      # App preferences and API key config
  Services/
    VoiceService.swift           # Singleton: voice fetching, caching, TTS API calls
    TaskDatabaseService.swift    # Realm CRUD for TTS tasks with cleanup
    OpenAIService.swift          # GPT integration for document analysis
    WebPageParser.swift          # HTML-to-text extraction
    DocumentParser.swift         # PDF/DOCX text extraction
    AuthManager.swift            # API key storage and validation
    PurchaseService.swift        # StoreKit IAP management
    UsageQuotaService.swift      # Free tier usage tracking
    AdSizeManager.swift          # Dynamic ad banner sizing
    SettingsManager.swift        # UserDefaults wrapper
    TrackingAT.swift             # App Tracking Transparency
    DiscardConfirmationManager.swift  # Unsaved changes guard
  Views/
    HomeView.swift               # Voice catalog with search and filters
    StudioView.swift             # Main workspace: type-and-play + file cards
    TypeOrPasteTextView.swift    # Text editor with inline TTS controls
    TaskDetailPlayerView.swift   # Full-screen audio player
    TaskHistoryView.swift        # Completed task list
    ImportedDocumentView.swift   # Document import flow
    WebpagesView.swift           # Web page URL input and parsing
    ScannedDocumentView.swift    # Camera-based OCR
    SettingsView.swift           # Preferences, API key, appearance
    ProUpgradeView.swift         # Premium subscription UI
  Components/
    Home/                        # SearchBar, VoiceList, HomeHeader, LoadingView
    Studio/                      # TypeAndPlayCard, NewFileCard, FilesSection
    TaskPlayer/                  # Header, ProgressBar, Content, Controls
    TypeOrPasteText/             # TextEditorArea, PlaybackProgressBar, Speed/Pitch/Rate overlays
    Settings/                    # SettingsHeader, ProUpgradeCard, APIKeyInputView
    ImportedDocument/            # DocumentPicker, AIAnalysisCard, DocumentWebView
    Webpages/                    # WebpageURLInput
```

## Key Features

- **100+ TTS Voices:** Browse, search, and filter voices by gender, locale, and status. Favorites persisted in Realm. Voice preview with streaming audio.
- **Multi-Source Input:** Type or paste text, import PDF/DOCX documents, scan physical documents via VisionKit OCR, or extract text from web pages.
- **AI Document Analysis:** OpenAI-powered analysis recommends optimal voices and reading settings for imported documents.
- **Task Queue:** Background TTS generation with status tracking (not started, in progress, completed, failed). Auto-resume of in-progress tasks on app launch.
- **Full-Featured Player:** Speed, pitch, and rate controls with a seekable progress bar. SRT subtitle file support.
- **Usage Quota:** Free tier with usage tracking. Pro upgrade removes ads and unlocks unlimited generation.

## How It Works

1. The user selects a voice from the catalog (filterable by gender, locale, favorites) and enters text through any input method (typing, document import, OCR scan, or web page extraction).
2. The app creates a `TTSTask` (Realm Object) and sends the text to the TTS API via `VoiceService`. The task status updates reactively through Combine publishers.
3. On completion, the generated audio file URL is stored in the task. The `TaskPlayerViewModel` handles playback with AVPlayer, supporting speed/pitch/rate adjustments and a progress bar.
4. Completed tasks are persisted in Realm and accessible from the task history view. The `TaskDatabaseService` handles cleanup of soft-deleted tasks on app launch.
