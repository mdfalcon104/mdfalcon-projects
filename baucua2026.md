# baucua2026

**Flutter cross-platform (iOS + Android) Bau Cua dice game that teaches English vocabulary through flashcards integrated into gameplay.**

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | Flutter (Dart SDK ^3.10.7) |
| State Management | flutter_riverpod |
| Audio | audioplayers, flutter_tts (text-to-speech) |
| Storage | shared_preferences, path_provider |
| Fonts | google_fonts |
| In-App Purchase | in_app_purchase |
| Game Services | games_services (Game Center / Google Play Games) |
| Networking | http |
| Encryption | pointycastle |
| Sharing | share_plus, url_launcher |
| SVG | flutter_svg |

## Architecture Overview

```
lib/
├── main.dart                    # App bootstrap, service init, ProviderScope
├── app.dart                     # BauCuaApp root widget
├── config/
│   ├── app_constants.dart       # Game constants and configuration
│   ├── app_theme.dart           # Theme definitions
│   └── app_colors.dart          # Color palette
├── models/
│   ├── game_state.dart          # Core game state model
│   ├── game_item.dart           # Bau Cua item definitions (6 animals)
│   ├── animal.dart              # Animal data model
│   ├── game_stats.dart          # Win/loss statistics
│   ├── level_rules.dart         # Leveling and difficulty rules
│   ├── topic_word.dart          # English vocabulary word model
│   └── topic_pack.dart          # Downloadable vocabulary topic pack
├── providers/
│   ├── game_provider.dart       # Core game logic provider
│   ├── bet_provider.dart        # Betting state management
│   ├── balance_provider.dart    # Coin balance tracking
│   ├── flashcard_provider.dart  # Flashcard display logic
│   ├── topic_provider.dart      # Vocabulary topic selection
│   ├── audio_provider.dart      # Background music state
│   ├── sfx_provider.dart        # Sound effects state
│   ├── settings_provider.dart   # User settings
│   ├── iap_provider.dart        # In-app purchase state
│   ├── hope_star_provider.dart  # Hope star bonus mechanic
│   ├── game_items_provider.dart # Game item configuration
│   ├── games_service_provider.dart # Game Center integration
│   └── storage_provider.dart    # Persistent storage provider
├── screens/
│   ├── game_screen.dart         # Main game screen
│   ├── splash_screen.dart       # Launch splash
│   ├── onboarding_screen.dart   # First-run onboarding
│   └── topic_download_screen.dart # Topic pack download UI
├── widgets/
│   ├── dice/                    # 3D dice rolling animation
│   ├── betting/                 # Betting grid and cells
│   ├── bowl/                    # Bowl cover/reveal animation
│   ├── flashcard/               # Flip card widget, flashcard overlay
│   ├── wheel/                   # Lucky wheel bonus game
│   ├── slot/                    # Slot machine mini-game (reel widget)
│   ├── shop/                    # Bean shop dialog
│   ├── game/                    # Play area, roll button, top bar, hope star
│   ├── settings/                # Settings dialog, rate/share, topic download
│   ├── decorations/             # Background gradient, gold border
│   └── common/                  # Game item icon, animated coin counter, casino button
├── services/
│   ├── audio_service.dart       # Background music management
│   ├── haptic_service.dart      # Haptic feedback
│   ├── games_service.dart       # Game Center / Play Games integration
│   ├── iap_service.dart         # In-app purchase processing
│   ├── topic_download_service.dart # Download vocabulary packs from server
│   ├── crypto_service.dart      # Encryption for secure data
│   └── payout_service.dart      # Payout calculation logic
├── painters/                    # Custom painters for animal illustrations
├── animations/                  # Custom animation controllers
├── data/
│   └── topic_catalog.dart       # Available vocabulary topic catalog
├── l10n/
│   └── app_strings.dart         # Localized strings
└── utils/
    └── responsive.dart          # Responsive layout utilities
```

## Key Features

- **Bau Cua Dice Game** -- traditional Vietnamese dice game with 6 animal symbols; animated 3D dice rolling under a bowl with reveal animation.
- **English Flashcards** -- after each round, an English vocabulary flashcard is shown with flip animation and text-to-speech pronunciation.
- **Downloadable Topic Packs** -- vocabulary organized by topics (e.g., airport, food, travel); packs can be downloaded from a remote server.
- **Betting System** -- place bets on animal cells with coin balance; configurable bet amounts with animated coin counters.
- **Leveling & Progression** -- level rules determine difficulty, payout multipliers, and unlock thresholds.
- **Mini-Games** -- lucky wheel and slot machine bonus games for extra coins.
- **Game Center / Play Games** -- leaderboard and achievement integration via `games_services`.
- **In-App Purchases** -- buy coin packs and premium content.
- **Sound & Haptics** -- background music, SFX, text-to-speech, and haptic feedback.

## How It Works

1. On startup, `main.dart` initializes services (StorageService, AudioService, SfxService), signs in to Game Center silently, and wraps the app in a Riverpod `ProviderScope`.
2. The `GameScreen` presents a betting grid of 6 animals. Players place coin bets on one or more animals via `BetProvider`.
3. Pressing the roll button triggers dice animation inside a bowl (`BowlWidget` + `DiceWidget`). Three dice are rolled; results are computed by `GameProvider`.
4. Payouts are calculated by `PayoutService` based on matching dice faces and level rules, then applied to `BalanceProvider`.
5. After each round, `FlashcardProvider` selects a vocabulary word from the active topic and displays it as a flip card overlay with TTS pronunciation.
6. Users can download new topic packs via `TopicDownloadService`, which fetches encrypted packs from a server and decrypts them with `CryptoService`.
7. Game statistics (wins, losses, streaks) are tracked in `GameStats` and synced with Game Center leaderboards.
