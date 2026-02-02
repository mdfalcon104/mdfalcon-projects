# YourCam YourImage

A Flutter-based photo enhancement and secure vault application featuring AI-powered face restoration via GFPGAN and an AES-256 encrypted file vault with iCloud sync support.

## Tech Stack

- **Framework:** Flutter (Dart 3.10+)
- **State Management:** Provider (ChangeNotifier)
- **AI Backend:** GFPGAN via Cloudflare Worker proxy to Replicate API
- **Encryption:** AES-256-CBC with PKCS7 padding (encrypt package)
- **Key Storage:** Flutter Secure Storage + native Keychain/Keystore
- **Auth:** JWT (HS256) for API authentication
- **Media:** just_audio with background playback, video_player, image_picker
- **Monetization:** In-App Purchase, AdMob (native bridge)
- **Platforms:** iOS, Android

## Architecture Overview

```
lib/
  main.dart                          # App entry, Provider setup, disguise mode
  core/
    constants/iap_products.dart      # IAP product IDs
    providers/
      ad_manager.dart                # Ad lifecycle management
      premium_provider.dart          # Premium status state
      theme_provider.dart            # Light/dark theme state
    services/
      biometric_service.dart         # Face ID / fingerprint auth
      iap_service.dart               # StoreKit / Google Play billing
      credit_service.dart            # Enhancement credit tracking
      native_services.dart           # Platform channel bridge
    theme/app_theme.dart             # Material theme definitions
    utils/                           # Snackbar helpers, formatters, logging
    widgets/                         # Shared UI components
  features/
    enhancement/
      enhancement_page.dart          # Photo picker + before/after UI
      services/
        gfpgan_service.dart          # Cloudflare Worker API client
        enhancement_settings_service.dart  # Scale/dimension prefs
        enhancement_history_service.dart   # History persistence
      widgets/
        before_after_slider.dart     # Side-by-side comparison widget
        enhancement_history_page.dart
    vault/
      vault_page.dart                # Encrypted file browser
      models/vault_item.dart         # Vault item data model
      services/
        vault_service.dart           # Abstract CRUD interface
        vault_service_impl.dart      # Concrete implementation
        vault_cloud_service.dart     # iCloud metadata sync
        cloud_sync_service.dart      # iCloud/Google Drive file migration
      utils/
        encryption.dart              # AES-256-CBC text encryption
        file_encryption.dart         # Streaming AES-256-CBC file encryption (1MB chunks)
        native_key.dart              # Native key provider (platform channel)
      widgets/                       # Photo viewer, document reader, vault list
    security/
      security_gate.dart             # PIN/pattern lock guard
      security_page.dart             # Security settings UI
      security_settings.dart         # Security mode configuration
      widgets/pattern_lock_widget.dart
    settings/                        # Enhancement, theme, legal, ads, upgrade settings
```

## Key Features

- **AI Photo Enhancement:** Sends photos to a GFPGAN model via a Cloudflare Worker endpoint. Supports configurable upscale factor and max dimension. Images are resized in a Dart isolate to avoid UI jank.
- **AES-256 Encrypted Vault:** Files are encrypted at rest using AES-256-CBC with per-file random IVs. Large files are processed in 1MB streaming chunks with progress callbacks. Keys are stored in the native Keychain (iOS) / Keystore (Android).
- **Cloud Sync:** Vault files can optionally sync to iCloud Documents (iOS) or Google Drive backup (Android). Files remain encrypted in transit and at rest.
- **Multi-Layer Security:** PIN (4-6 digits), pattern lock, and biometric authentication (Face ID / Touch ID). First-launch PIN setup enforced.
- **Disguise Mode:** Hides the vault behind a fake enhancement-only UI. Exit button auto-hides after 5 seconds; triple-tap on the title to reveal it.
- **Premium & Ads:** In-app purchase removes ads and unlocks unlimited enhancements. AdMob banner and interstitial ads served via native bridge.

## How It Works

1. **Enhancement Flow:** User picks a photo, the app resizes it if needed (isolate-based), converts to base64, and POSTs to a Cloudflare Worker with JWT auth. The worker creates a GFPGAN prediction on Replicate, and the app polls for completion. A reward ad plays during the polling window. The enhanced image URL is returned for preview and optional save/share.
2. **Vault Flow:** Files imported into the vault are encrypted with AES-256-CBC using a device-derived key from the native Keychain. The app writes a custom header (`VAULT_ENC_V1`) followed by the IV, then length-prefixed encrypted chunks. Decryption reverses this process. Cloud sync migrates encrypted files between Documents (synced) and Application Support (local-only).
3. **Security Flow:** On first launch, the user sets a PIN. Subsequent vault access requires PIN verification. The security gate supports PIN, pattern, and biometric modes. Disguise mode persists across app restarts via Flutter Secure Storage.
