# DealFinder (SHP Affiliate)

SwiftUI iOS shopping deal finder and affiliate app with product price comparison, deal alerts, category browsing, favorites, shopping lists, and commission tracking integration.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Language | Swift |
| UI | SwiftUI |
| Architecture | MVVM |
| Dependencies | CocoaPods |
| Platform | iOS |

## Architecture Overview

```
DealFinder/
  App/                         # App entry point and configuration
  Models/                      # Data models (products, deals, categories)
  Networking/                  # API client and request handling
  Services/                    # Business logic services
  Storage/                     # Local data persistence
  ViewModels/                  # MVVM view models
  Views/
    Home/                      # Home feed with featured deals
    Categories/                # Product category browsing
    Search/                    # Product search interface
    ProductDetail/             # Individual product details
    Favorites/                 # Saved favorite products
    ShoppingList/              # Shopping list management
    Tracking/                  # Order and commission tracking
    Components/                # Reusable UI components
    Settings/                  # App settings
    Splash/                    # Launch screen
  Utilities/                   # Helper functions and extensions
  Resources/                   # Assets and configuration files

DealFinderWidget/              # Home screen widget
DealFinderTests/               # Unit tests
api/                           # Backend API configuration
website/                       # Promotional web page
```

## Key Features

- Product deal browsing with category filters
- Price comparison across multiple retailers
- Search functionality with autocomplete
- Favorites management for saved deals
- Shopping list creation and management
- Commission and affiliate tracking dashboard
- Product detail pages with price history
- Home screen widget for deal highlights
- Deal alert notifications for price drops

## How It Works

1. **Browse**: Users explore deals through the home feed, category filters, or search. Products are fetched from affiliate API endpoints.
2. **Compare**: Product detail pages show prices across retailers with affiliate links for purchase redirection.
3. **Save**: Users can favorite products or add them to shopping lists for future reference.
4. **Track**: The tracking section shows affiliate commission status and order history.
5. **Widget**: The DealFinderWidget displays top deals on the home screen for quick access.
