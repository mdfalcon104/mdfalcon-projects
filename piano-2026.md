# Piano 2026

A realistic piano keyboard simulator built with Flutter, featuring SoundFont-based MIDI audio, multi-octave scrollable keyboard, recording/playback, customizable themes, and premium upgrade support.

## Tech Stack

- **Framework:** Flutter (Dart 3.0+)
- **State Management:** Provider (ChangeNotifier)
- **Audio Engine:** flutter_midi_pro (SoundFont .sf2 playback)
- **Audio Session:** audio_session for iOS/Android audio routing
- **Volume Control:** flutter_volume_controller
- **MIDI:** XML parsing for instrument/channel configuration
- **Monetization:** In-App Purchase (StoreKit/Google Play), AdMob (native bridge)
- **Analytics:** Firebase (native Swift/Kotlin integration)
- **Platforms:** iOS, Android

## Architecture Overview

```
lib/
  main.dart                        # App entry, MultiProvider setup, splash-based init
  config/
    ad_config.dart                 # Ad unit IDs and placement config
    debug_config.dart              # Debug logging toggle
  models/
    piano_key.dart                 # Individual key model (note, octave, type)
    piano_settings.dart            # User preferences (range, theme, instrument)
    keyboard_theme.dart            # Visual theme definitions (8+ themes)
    midi_instrument.dart           # MIDI instrument model with program number
    midi_channel.dart              # MIDI channel configuration
    recording.dart                 # Recording model (timestamp, notes, duration)
    app_enums.dart                 # Shared enumerations
  services/
    midi_audio_service.dart        # SoundFont loading, note playback, instrument switching
    audio_service.dart             # Audio session management
    piano_key_service.dart         # Key layout generation and octave management
    recording_service.dart         # Record, playback, save/load recordings
    settings_service.dart          # SharedPreferences persistence
    purchase_service.dart          # IAP initialization and purchase flow
    ad_service.dart                # Ad loading and display (depends on purchase status)
    review_service.dart            # In-app review prompt logic
    volume_service.dart            # System volume monitoring
    tracking_service.dart          # Analytics event tracking
    sound_effects_service.dart     # UI sound effects
    native_config_service.dart     # Platform channel for native config
  screens/
    piano_screen.dart              # Main piano UI with toolbar
    settings_screen.dart           # Settings (instrument, theme, range, MIDI channel)
    premium_upgrade_screen.dart    # Premium purchase flow
    splash_screen.dart             # Animated splash with parallel service init
    recordings_list_screen.dart    # Saved recordings browser
  widgets/
    piano_keyboard.dart            # Scrollable multi-octave keyboard layout
    piano_key_widget.dart          # Individual key rendering with touch handling
    themed_toast.dart              # Theme-aware toast notifications
    webview_screen.dart            # In-app web view
    splash_screen.dart             # Splash animation widget
```

## Key Features

- **Realistic SoundFont Audio:** Uses .sf2 SoundFont files loaded through flutter_midi_pro for authentic piano, organ, guitar, and 100+ MIDI instrument sounds. Supports program change and channel switching.
- **Multi-Octave Scrollable Keyboard:** Horizontally scrollable keyboard with pinch-to-zoom. White and black keys rendered with accurate proportions and touch hit-testing.
- **8+ Visual Themes:** Wood, Dark, Modern, Neon, Golden, Electric, Vintage, Tropical. Each theme defines key colors, gradients, and background textures.
- **Recording & Playback:** Record note sequences with timestamps. Playback replays notes at recorded timing. Save and manage recordings in a library.
- **MIDI Instrument Selection:** 100+ General MIDI instruments selectable via settings. Instruments organized by category (piano, organ, guitar, strings, brass, etc.).
- **Premium Features:** Ad removal, all themes unlocked, all instruments, and extended octave range via a single IAP.

## How It Works

1. **Startup:** `main.dart` creates all service instances without awaiting initialization. The splash screen triggers parallel initialization of settings, recordings, purchases, review, and volume services. Audio SoundFont loading happens in the background to avoid blocking the UI.
2. **Keyboard Rendering:** `PianoKeyService` generates a list of `PianoKey` models for the configured octave range. `PianoKeyboard` renders them in a horizontally scrollable layout. `PianoKeyWidget` handles touch-down/up events and sends note-on/note-off commands to `MidiAudioService`.
3. **Audio Playback:** `MidiAudioService` loads a .sf2 SoundFont file and plays notes by program number and MIDI channel. Instrument switching triggers a MIDI program change message.
4. **Recording:** `RecordingService` captures note events with timestamps relative to recording start. Playback uses a timer to replay events at the original timing.
