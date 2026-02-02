# Lich Am Lich Van Su

Native iOS Vietnamese lunar calendar application built with SwiftUI and MVVM architecture. Provides daily lunar date conversion, Vietnamese zodiac information, fortune/almanac readings, holiday tracking, event management, and astrology features. Integrates OpenAI for AI-powered fortune interpretations.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Language | Swift, SwiftUI |
| Architecture | MVVM (Model-View-ViewModel) |
| Data | CoreData (via DatabaseManager), JSON almanac data |
| Notifications | UserNotifications framework |
| Monetization | StoreKit (In-App Purchases / Subscriptions) |
| Push | Firebase Cloud Messaging |
| AI | OpenAI API integration |
| Ads | Google AdMob |
| Build | Xcode, CocoaPods |

## Architecture Overview

```
LichAmLichVanSu/
  LichAmLichVanSuApp.swift    # App entry point
  aContentView.swift           # Root content view

  Models/
    CalendarDay.swift           # Single day data model
    CalendarType.swift          # Calendar type enum (solar/lunar)
    DayInfo.swift               # Detailed day information (zodiac, elements)
    DateFormat.swift            # Date display format options
    TimeFormat.swift            # Time display format options
    Event.swift                 # User event model
    Holiday.swift               # Vietnamese holiday model
    TabBar.swift                # Tab navigation model

  ViewModels/
    MonthCalendarViewModel.swift  # Calendar grid logic and date calculations
    EventViewModel.swift          # Event CRUD operations
    SettingsViewModel.swift       # User preferences management
    OpenAIViewModel.swift         # AI fortune interpretation
    Subscriptions/
      IAPManager.swift            # In-app purchase handling
      SubscriptionPlan.swift      # Subscription tier definitions
      PoohWisdomProducts.swift    # Product identifiers
      QuotesGroup.swift           # Wisdom quote categories

  Views/
    MainTabView.swift             # Tab-based navigation (Home, Calendar, Events, Settings)
    Home/
      Components/
        MainContent.swift         # Daily lunar info display
        HeaderDateLable.swift     # Lunar/solar date header
        DayNumber.swift           # Large day number display
        DayQuote.swift            # Daily wisdom quote
        InfoTabContent.swift      # Day info tab (zodiac, elements)
        FortuneTabContent.swift   # Fortune/almanac readings
        HoursTabContent.swift     # Hourly fortune guide
        WeatherTabContent.swift   # Weather-based guidance
        TravelTabContent.swift    # Travel direction advice
        DirectionRow.swift        # Compass direction component
    Calendar/                     # Month/year calendar grid views
    Events/                       # Event creation and listing
    Astrology/                    # Zodiac and astrology features
    Settings/
      SettingsView.swift          # Preferences screen
      Selection/                  # Timezone, location, theme, notification pickers
      About/                      # About, privacy policy

  Managers/
    DatabaseManager.swift         # CoreData persistence
    HolidayManager.swift          # Vietnamese holiday data
    NotificationManager.swift     # Local notification scheduling
    AlmanacDownloadManager.swift  # Almanac data fetching
    QuoteManager.swift            # Daily wisdom quotes
    PurchaseService.swift         # StoreKit purchase flow
    AdSizeManager.swift           # AdMob banner sizing

  Utils/
    LunarCalendar.swift           # Core lunar date conversion algorithm
    ColorUtils.swift              # Theme color helpers
    Fernet.swift                  # Encryption utility
    Sheets.swift                  # Sheet presentation helpers
    SwiftUI+Toast.swift           # Toast notification extension
    TabBar+Enviroment.swift       # Tab bar environment injection
```

## Key Features

- Accurate Vietnamese lunar calendar conversion (Am Lich)
- Daily fortune readings and almanac (Lich Van Su) with good/bad activity guidance
- Vietnamese zodiac animal display with artwork for all 12 animals
- Hourly fortune guide (gio hoang dao)
- Travel direction advisor based on lunar date
- Event management with local notification reminders
- Vietnamese holiday tracking (both solar and lunar holidays)
- AI-powered fortune interpretation via OpenAI
- Astrology section with zodiac compatibility
- Subscription model with multiple tiers
- Customizable themes, date/time formats, and timezone selection
- iOS Widget support (Clip target)

## How It Works

1. **Lunar Conversion**: The `LunarCalendar` utility implements the Vietnamese lunar calendar algorithm, converting between solar and lunar dates and calculating associated zodiac, elemental, and directional data.
2. **Daily View**: On launch, the home screen displays the current lunar date with zodiac animal, daily fortune, wisdom quote, and tabbed sections for detailed almanac readings (info, fortune, hours, weather, travel).
3. **Calendar Navigation**: Users browse months in a grid view. Each day cell shows both solar and lunar dates. Tapping a day reveals full lunar details and any associated events or holidays.
4. **Event System**: Users create events tied to either solar or lunar dates. The `NotificationManager` schedules local push notifications at the user-configured reminder time.
5. **Subscription**: Premium features (ad removal, full almanac access, AI interpretations) are gated behind StoreKit subscriptions managed by `IAPManager`.
