# YouTubeKit Server

Serverless extraction API running on Cloudflare Workers that provides remote audio stream resolution, cipher decryption, and metadata parsing for iOS clients. Uses youtubei.js library with QuickJS WASM runtime for JavaScript cipher execution in the Workers environment.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Runtime | Cloudflare Workers |
| Language | TypeScript |
| YouTube Client | youtubei.js (custom Android SDK-less fork) |
| JS Runtime | QuickJS (WASM) for cipher decryption |
| Deployment | Wrangler CLI |
| Testing | Vitest with @cloudflare/vitest-pool-workers |

## Architecture Overview

```
src/
  index.ts                  # Worker entry point and request routing
  youtube/                  # YouTube extraction logic
  durable-objects/          # Cloudflare Durable Objects for state
  quickjs.wasm              # QuickJS WASM binary for cipher execution
  quickjs.d.ts              # QuickJS type definitions

test/                       # Vitest integration tests
wrangler.toml               # Cloudflare Workers configuration
vitest.config.mts           # Test configuration
worker-configuration.d.ts   # Worker environment type definitions
```

## Key Features

- Audio stream URL resolution from YouTube video IDs
- Cipher decryption using QuickJS WASM runtime (bypasses Workers JS limitations)
- Video metadata parsing (title, duration, thumbnails, channel info)
- Durable Objects for caching and rate limiting
- Custom youtubei.js fork optimized for serverless environment
- Edge deployment across Cloudflare's global network
- Type-safe worker configuration with generated types

## How It Works

1. **Request Handling**: The Worker receives requests from iOS clients (YouTubeKit) with video IDs or search queries.
2. **YouTube Resolution**: The youtubei.js library (Android SDK-less client fork) communicates with YouTube's InnerTube API to fetch video metadata and stream manifests.
3. **Cipher Decryption**: YouTube's signature cipher is executed in a QuickJS WASM sandbox, which provides a full JavaScript runtime within the Cloudflare Workers environment where eval() is restricted.
4. **Response**: Resolved audio stream URLs, metadata, and format information are returned to the iOS client for playback with ModernAVPlayer.
