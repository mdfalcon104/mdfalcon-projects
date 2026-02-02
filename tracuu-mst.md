# Tax Code Lookup (Tracuu MST)

Full-stack tax code lookup service with a SwiftUI iOS app, Cloudflare Workers serverless backend, and responsive web interface for querying Vietnamese business registration data by tax identification number.

## Tech Stack

| Layer | Technology |
|-------|------------|
| iOS | Swift, SwiftUI, MVVM |
| Backend | TypeScript, Cloudflare Workers |
| Database | D1 (Cloudflare SQL) |
| Web | HTML, CSS, JavaScript |
| Deployment | Wrangler CLI |
| iOS Deps | Alamofire, AlamofireObjectMapper, Google Mobile Ads |

## Architecture Overview

```
ios/
  TracuuMST/                # SwiftUI iOS application
  TracuuMST.xcodeproj/      # Xcode project
  TracuuMST.xcworkspace/    # CocoaPods workspace
  Podfile                   # iOS dependencies
  TracuuMSTTests/           # Unit tests

backend/
  src/
    index.js                # Cloudflare Worker entry point
  schema.sql                # Database schema definition
  industry_codes_seed.sql   # Industry code reference data
  scripts/                  # Database setup and migration scripts
  wrangler.toml             # Cloudflare Workers configuration

website/
  firebase.json             # Firebase hosting config
  public/                   # Static web files

docs/                       # Design and implementation documentation
```

## Key Features

- Tax code (MST) lookup with business registration details
- Company name, address, and registration status display
- Industry code classification with seeded reference data
- SwiftUI native iOS app with search interface
- Cloudflare Workers backend for low-latency API responses
- D1 SQL database for business registration data
- Responsive web interface for browser-based lookups
- AdMob integration for iOS app monetization

## How It Works

1. **User Input**: Users enter a Vietnamese tax identification number (MST) in either the iOS app or web interface.
2. **API Request**: The query is sent to the Cloudflare Workers backend, which handles request validation and rate limiting.
3. **Database Lookup**: The Worker queries the D1 database for matching business registration records, joining with industry code reference data.
4. **Response**: Business details (company name, address, registration date, industry classification, operating status) are returned and displayed in the client interface.
