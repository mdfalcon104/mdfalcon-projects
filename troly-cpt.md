# Ringtone AI Studio (troly-cpt)

A multi-platform ringtone creation suite featuring an iOS app built with SwiftUI and The Composable Architecture (TCA), a Python AI backend for voice synthesis and audio analysis, and a Firebase-hosted website. Supports AI voice generation, DNA-based unique ringtone creation, smart audio hook detection, waveform editing, and GarageBand-compatible export.

## Tech Stack

- **iOS App:** Swift 5, SwiftUI, The Composable Architecture (TCA)
- **State Management:** TCA Reducers with @ObservableState, Jotai-style scoped stores
- **Audio Engine:** AVFoundation (AVAudioEngine, AVAudioFile, AVSpeechSynthesizer)
- **AI Backend:** Python API hosted on Railway (voice synthesis, DNA ringtone generation, smart hook analysis)
- **Auth:** JWT (HS256) with CryptoKit HMAC-SHA256 signing
- **Music API:** Custom API with AES-encrypted endpoints for browsing/downloading music
- **Monetization:** StoreKit 2 IAP, Google AdMob via hmdcommonkit
- **Design System:** Custom liquid glass UI components
- **Website:** Firebase Hosting (static site)
- **Platforms:** iOS 15+

## Architecture Overview

```
troly-cpt/
  RingtoneMaker/                    # iOS App (Swift Package + CocoaPods)
    RingtoneMaker/
      App/
        RingtoneMakerApp.swift      # @main entry, TCA Store initialization
        AppDelegate.swift           # Firebase, AdMob, push notifications
        PurchaseService.swift       # StoreKit 2 IAP management
        AdSizeManager.swift         # Dynamic ad banner sizing
        ReviewService.swift         # In-app review prompt logic
        LaunchScreen.swift          # Animated launch screen
      Features/
        Home/
          HomeFeature.swift         # TCA Reducer: recent projects, file import, ATT
          HomeView.swift            # Main dashboard with feature cards
        Editor/
          EditorFeature.swift       # TCA Reducer: waveform editing, trimming, effects
          EditorView.swift          # Audio editor UI
          Components/
            WaveformView.swift      # Real-time waveform visualization
            TimelineView.swift      # Scrubable timeline with markers
            PlaybackControlsView.swift
            EffectsPanelView.swift  # Audio effects (reverb, delay, pitch)
            SmartTrimView.swift     # AI-powered smart trim suggestions
        VoiceRingtone/
          VoiceRingtoneFeature.swift  # TCA Reducer: 15+ voice styles, TTS generation
          VoiceRingtoneView.swift     # Voice style picker, template library, generation
        VoicePresets/
          VoicePresetsFeature.swift   # Pre-generated voice ringtone catalog
          VoicePresetsView.swift
        DNARingtone/
          DNARingtoneFeature.swift    # TCA Reducer: name + birthday + mood = unique ringtone
          DNARingtoneView.swift
        MindfulRingtone/
          MindfulRingtoneFeature.swift  # Meditation/ambient ringtone generation
          MindfulRingtoneView.swift
        ScienceRingtone/
          ScienceRingtoneFeature.swift  # Science-themed sound generation
          ScienceRingtoneView.swift
        ContextAwareRingtone/
          ContextAwareRingtoneFeature.swift  # Rule-based ringtone selection
          ContextAwareRingtoneView.swift
        MusicBrowser/
          MusicBrowserFeature.swift   # Browse and download music for editing
          MusicBrowserView.swift
        Recording/
          RecordingFeature.swift      # Microphone recording
          RecordingView.swift
        Library/
          LibraryFeature.swift        # Saved ringtone library
          LibraryView.swift
        Export/
          ExportedRingtonesFeature.swift  # Export management
          ExportedRingtonesView.swift
        Guide/
          GuideFeature.swift          # GarageBand installation guide
          GuideView.swift
        Settings/
          SettingsFeature.swift       # App preferences
          SettingsView.swift
      Core/
        RingtoneAI/
          RingtoneAIClient.swift    # TCA Dependency: AI API client (voice, DNA, smart hook)
        AudioEngine/
          AudioEngineClient.swift   # TCA Dependency: AVAudioEngine wrapper
          AudioEngineClient+Live.swift
        AudioAnalyzer/
          AudioAnalyzerClient.swift # TCA Dependency: audio analysis
          AudioAnalyzerClient+Live.swift
        Audio/
          AudioMixer.swift          # Multi-track audio mixing
          PresetAudioPlayer.swift   # AVAudioPlayer wrapper for presets
          GarageBandService.swift   # GarageBand export integration
        MusicAPI/
          MusicAPIClient.swift      # TCA Dependency: music browsing/download API
          AudioPlayerClient.swift   # Streaming audio player
          AESUtils.swift            # AES decryption for API responses
          CoverImageCache.swift     # Image caching for album art
        MediaPicker/
          MediaPickerClient.swift   # TCA Dependency: file/video import
          MediaPickerClient+Live.swift  # Video audio extraction
      DesignSystem/
        Theme/
          RMColors.swift            # Color palette
          RMTypography.swift        # Font styles
          RMSpacing.swift           # Spacing tokens
        Components/
          LiquidGlassCard.swift     # Glass-morphism card
          LiquidGlassButton.swift   # Translucent button
          LiquidGlassToolbar.swift  # Frosted glass toolbar
          GlowingIcon.swift         # Animated glowing icons
          ShimmerEffect.swift       # Loading shimmer
          SafariView.swift          # In-app Safari
        Modifiers/
          LiquidGlassEffect.swift   # Glass-morphism modifier
          SpringAnimation.swift     # Spring animation preset
  api/
    ringtone-ai/                    # Python AI backend (Railway deployment)
  website/
    public/                         # Firebase Hosting static site
  ringtones-maker/                  # Screenshots and marketing assets
```

## Key Features

- **AI Voice Ringtone Generation:** 15+ voice styles (Professional, Robot, Alien, Chipmunk, etc.) with server-side AI synthesis or local AVSpeechSynthesizer fallback. Voice templates for common use cases (incoming call, alarm, notification).
- **DNA Ringtone:** Generate a unique ringtone from the user's name, birthday, and mood selection. Produces a deterministic seed-based audio signature.
- **Smart Hook Detection:** Upload any audio file and the AI analyzes it to find the best hook segments (highest energy, best melodic sections) for ringtone use.
- **Waveform Editor:** Visual waveform display with scrubable timeline, trim handles, and audio effects (reverb, delay, pitch shift).
- **Music Browser:** Browse and download music from the integrated API (AES-encrypted endpoints) for use as ringtone source material.
- **Multiple AI Ringtone Modes:** Mindful (meditation/ambient), Science (science-themed), Context-Aware (rule-based dynamic selection).
- **GarageBand Export:** Guide users through exporting ringtones to GarageBand for iOS ringtone assignment.
- **Liquid Glass Design System:** Custom frosted-glass UI components with spring animations and mesh gradient backgrounds.

## How It Works

1. **TCA Architecture:** Each feature is a self-contained TCA `Reducer` with `State`, `Action`, and `Effect`. The root `AppFeature` composes all feature reducers via `Scope` and manages navigation between tabs, sheets, and overlays. Dependencies (AI API, audio engine, media picker) are injected through TCA's `@Dependency` system.
2. **Voice Generation:** The app checks server health first. If the AI server is available, it sends a POST request to `/api/v1/voice-ringtone` with JWT auth, text, voice style, and language. The server returns a download URL. If unavailable, it falls back to local `AVSpeechSynthesizer` with style-specific voice configurations (rate, pitch, volume, voice identifier).
3. **Audio Editing:** Imported files (audio, video with extraction, or recordings) are loaded into the editor. The `AudioEngineClient` wraps AVAudioEngine for real-time playback, effects processing, and rendering. The `AudioAnalyzerClient` provides waveform data for visualization. Smart trim sends audio to the AI server for hook analysis.
4. **Export:** Edited ringtones are saved as M4A files to the Documents directory. The app guides users through the GarageBand workflow to set the file as their iOS ringtone.
