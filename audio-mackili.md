# audio-mackili

**Flask API proxy for Audiomack music data -- provides authenticated access to trending, top charts, search, and song playback via Playwright-based OAuth capture and Redis caching.**

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Language | Python 3 |
| Framework | Flask 3.0 |
| Auth | Flask-JWT-Extended (JWT tokens) |
| Browser Automation | Playwright (Chromium, headless) |
| Caching | Redis |
| WSGI Server | Waitress (production) |
| Scheduling | APScheduler |
| API Docs | Flask-Swagger-UI + OpenAPI 3.0 |
| Containerization | Docker + Docker Compose (with Playwright image) |
| Deployment | Railway |

## Architecture Overview

```
audio-mackili/
├── flask_api_audiomack.py       # Main Flask app -- all endpoints and OAuth capture
├── flask_api.py                 # Alternative/base API module
├── playwright_xhr_capture.py    # Standalone Playwright XHR capture utility
├── generate_jwt_token.py        # JWT token generation script
├── openapi.json                 # OpenAPI 3.0 specification
├── requirements.txt             # Python dependencies
├── Dockerfile                   # Standard Docker build
├── Dockerfile.playwright        # Playwright-enabled Docker build
├── docker-compose.yml           # Multi-service compose (app + Redis)
├── docker.sh                    # Docker helper scripts
├── cron-oauth-refresh.sh        # Cron job for periodic OAuth refresh
├── swift-client/                # iOS Swift client example
├── test_*.py                    # Test suite (API, JWT, OAuth, Flask)
└── *.md                         # Documentation (API docs, cron setup, deployment guides)
```

## Key Features

- **Audiomack Proxy API** -- four JWT-protected endpoints:
  - `GET /api/search` -- search music by query with pagination, sort, and content type filters
  - `GET /api/trending` -- trending songs with optional category filtering (afrobeats, rap, pop, etc.)
  - `GET /api/top-songs` -- weekly top chart songs with category and pagination
  - `GET /api/play` -- get song detail and stream URL by song ID
- **Playwright OAuth Capture** -- headless Chromium browser visits Audiomack pages, intercepts XHR requests to `api.audiomack.com`, and extracts OAuth query strings in real-time. Falls back to scrolling/clicking "Load More" buttons to trigger API calls.
- **Redis Caching** -- search results cached for 2 hours; trending/top songs cached for 24 hours; song details cached for 2 hours. Cache keys include all query parameters.
- **JWT Authentication** -- all data endpoints require a valid JWT Bearer token (5-minute expiry).
- **Swagger UI** -- interactive API documentation at `/api/docs` with try-it-out support.
- **Railway Deployment** -- production-ready with Railway API key verification for cron job endpoints.

## How It Works

1. A client authenticates by obtaining a JWT token (via `generate_jwt_token.py` or a token endpoint).
2. The client calls a protected endpoint (e.g., `/api/trending?category=rap&page=1`) with the JWT in the Authorization header.
3. The server checks Redis cache first. On cache hit, the cached JSON is returned immediately.
4. On cache miss, `capture_oauth_from_audiomack()` launches a headless Chromium browser via Playwright, navigates to the corresponding Audiomack web page (e.g., `audiomack.com/trending-now/songs/rap`), and intercepts outgoing XHR requests to `api.audiomack.com` to capture the OAuth query string.
5. The captured OAuth parameters are appended to the upstream Audiomack API URL, and the server makes a direct HTTP request to fetch the data.
6. The response is cached in Redis with the appropriate TTL and returned to the client.
7. In production, the app runs on Waitress WSGI server with 8 threads and 500 max connections. Docker Compose orchestrates the Flask app alongside a Redis instance.
