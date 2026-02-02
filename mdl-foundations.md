# MDL Foundations

Backend foundation platform with TypeScript server, automated testing via Vitest, dashboard interface, and AI engine integration for iOS app generation from screenshots and product specifications.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Runtime | Node.js, TypeScript |
| Build | tsx |
| Testing | Vitest |
| AI Engine | OpenAI integration |
| Dashboard | HTML/JS frontend |

## Architecture Overview

```
backend/
  src/
    server.ts               # Express/HTTP server entry point
    middleware/              # Request middleware (auth, validation)
    routes/                  # API route handlers
    services/                # Business logic services
    types/                   # TypeScript type definitions

ai-engine/
  src/
    audit/                  # Code audit and quality analysis
    code-generator/         # iOS code generation from specs
    product-spec/           # Product specification parser
    screenshot-analyzer/    # Screenshot analysis for UI extraction
    types.ts                # AI engine type definitions

dashboard/
  index.html                # Web dashboard interface

ios-generator/              # iOS project scaffolding
scripts/                    # Build and utility scripts
tests/                      # Vitest test suites
uploads/                    # Uploaded screenshots and assets
```

## Key Features

- Screenshot analysis for extracting UI components and layout
- AI-powered iOS code generation from product specifications
- Code audit engine for quality assessment
- Product specification parsing and validation
- Web dashboard for project management and monitoring
- REST API server with middleware pipeline
- Automated testing with Vitest
- File upload handling for screenshots and design assets

## How It Works

1. **Input**: Users upload app screenshots or product specifications through the dashboard or API.
2. **Analysis**: The screenshot analyzer uses AI to identify UI components, layout patterns, and navigation flows from uploaded images.
3. **Specification**: Product specs are parsed into structured data defining screens, data models, and business logic requirements.
4. **Generation**: The code generator produces SwiftUI/UIKit code based on the analyzed screenshots and parsed specifications.
5. **Audit**: Generated code is run through the audit engine for quality checks and best practice validation.
