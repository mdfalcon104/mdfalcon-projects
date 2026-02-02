# HMD CommonKit

Reusable iOS framework distributed via CocoaPods providing common utilities, audio player components, ad management, in-app purchase helpers, and shared UI elements used across multiple iOS applications.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Language | Swift, Objective-C |
| Distribution | CocoaPods |
| Networking | AFNetworking (bundled) |
| Reachability | Reachability (network status) |
| UI | MBProgressHUD, FLKAutoLayout |
| Platform | iOS 13.0+ |

## Architecture Overview

```
hmdcommonkit/
  Classes/
    Common/                     # Shared utility functions
    CommonFunction/             # Common helper methods
    Helper.h                    # Objective-C helper header
    AFNetworking/               # Bundled AFNetworking library
    FLKAutoLayout/              # Auto Layout helpers
    InApp/                      # In-app purchase management
    MBProgressHUD/              # Loading indicator component
    PromotionApp/               # Cross-promotion for other apps
    RatingUnix/                 # App rating prompt logic
    Reachability/               # Network connectivity monitoring

  Assets/                       # Shared assets (icons, images)

  # Versioned releases
  0.1.1/ through 0.5.0/        # Historical version snapshots

Example/                        # Example integration project
hmdcommonkit_2025.podspec       # CocoaPods spec (2025 edition)
hmdcommonkitwa.podspec          # WhatsApp variant podspec
```

## Podspec Variants

| Spec | Description |
|------|-------------|
| `hmdcommonkit_2025` | Main framework with ad management, IAP helpers, and common utilities |
| `hmdcommonkitwa` | Lightweight variant for specific app requirements |

## Key Features

- Google Mobile Ads integration with banner and interstitial management
- In-app purchase flow helpers with receipt validation
- App rating prompt with configurable trigger conditions
- Cross-app promotion with app showcase views
- Network reachability monitoring
- MBProgressHUD loading indicators
- Auto Layout convenience methods (FLKAutoLayout)
- AFNetworking for HTTP requests
- Shared configuration via HMDConfig.plist
- Version-tagged releases for dependency pinning

## How It Works

1. **Integration**: Apps include hmdcommonkit via CocoaPods by specifying the git repo and version tag in their Podfile.
2. **Configuration**: Each app provides an HMDConfig.plist with ad unit IDs, IAP product IDs, and feature flags.
3. **Ad Management**: The framework handles ad loading, display frequency capping, and banner placement across the app.
4. **IAP**: In-app purchase helpers manage product fetching, purchase flow, and receipt validation with a simplified API.
5. **Utilities**: Common functions, network monitoring, loading indicators, and layout helpers are available as imported modules across all consuming apps.
