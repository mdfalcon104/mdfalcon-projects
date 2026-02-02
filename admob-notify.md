# AdMob Notify

NestJS backend service that automatically fetches Google AdMob revenue reports and delivers formatted summaries to a Telegram channel. Handles OAuth2 token lifecycle management with Firebase Firestore persistence, scheduled cron-based reporting via Vercel, and proactive token health monitoring with alert notifications.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Runtime | Node.js 18+, TypeScript |
| Framework | NestJS 9 |
| Auth | Google OAuth2 (googleapis) |
| Storage | Firebase Admin SDK + Firestore |
| Database | PostgreSQL (Prisma ORM, legacy) |
| Notifications | Telegram Bot API (node-telegram-bot-api) |
| Scheduling | @nestjs/schedule (cron), Vercel Crons |
| Deployment | Vercel (serverless functions) |
| Formatting | ascii-table (revenue tables) |

## Architecture Overview

```
src/
  main.ts                 # NestJS bootstrap (production)
  main.dev.ts             # Development entry point
  app.module.ts           # Root module (Config, Schedule, Firebase)
  app.controller.ts       # REST endpoints (12 routes)
  app.service.ts          # Core business logic (report generation, token management)
  firebase.service.ts     # Firebase Firestore CRUD (reports, tokens)
  prisma.service.ts       # Prisma client (legacy DB access)
  prisma/
    schema.prisma         # Report + login models (PostgreSQL)

api/
  index.ts                # Vercel serverless function entry

vercel.json               # Vercel deployment config + cron schedules
terraform/                # Infrastructure as code (if needed)
public/                   # Static landing page
```

## REST API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| GET | `/` | Health check landing page |
| GET | `/authenticate` | Redirect to Google OAuth2 consent |
| GET | `/oauth2callback` | Handle OAuth2 callback, store tokens |
| GET | `/report` | Generate and send revenue report to Telegram |
| GET | `/cron` | Cron-triggered report (Vercel secret auth) |
| GET | `/refresh-token` | Force token refresh |
| GET | `/scheduled-token-maintenance` | Run token maintenance cycle |
| GET | `/token-status` | Check if token is expiring |
| GET | `/token-info` | Detailed token expiry information |
| GET | `/health` | System health with token status |
| GET | `/check-token-health` | Force health check with Telegram alerts |
| GET | `/clearDb` | Clear report data |
| GET | `/sendTelegram` | Test Telegram connectivity |

## Key Features

- Automated daily AdMob revenue reporting via Telegram
- Google OAuth2 flow with automatic token refresh
- Firebase Firestore for persistent token and report storage
- Revenue aggregation by app with formatted ASCII table output
- Vercel Cron scheduling (hourly reports, 6-hour token refresh)
- Proactive token health monitoring with Telegram alerts
- Multi-app revenue tracking across all AdMob properties
- Timezone-aware reporting (Asia/Singapore)
- Bearer token authentication for cron endpoints

## Vercel Cron Schedule

```
59 * * * *     /cron                           # Hourly revenue report
0 */6 * * *    /refresh-token                  # Token refresh every 6 hours
0 */6 * * *    /scheduled-token-maintenance    # Token health check every 6 hours
```

## How It Works

1. **Authentication**: The admin visits `/authenticate`, which redirects to Google OAuth2 consent. Upon approval, the callback saves the refresh token and access token to Firebase Firestore.
2. **Token Management**: Before each API call, `ensureValidToken()` checks if the access token is expired or expiring within 5 minutes. If so, it uses the stored refresh token to obtain a new access token. The scheduled maintenance task runs every 6 hours to proactively refresh tokens and send Telegram alerts if issues are detected.
3. **Report Generation**: The `/cron` endpoint (triggered hourly by Vercel) calls the AdMob Network Report API for the current day. It fetches per-app, per-ad-unit earnings broken down by country and ad type.
4. **Aggregation and Delivery**: Revenue data is aggregated by app name, formatted into an ASCII table showing per-app earnings and total USD, then sent to the configured Telegram channel as a formatted markdown message.
5. **Health Monitoring**: The system continuously monitors token validity and sends graduated Telegram alerts -- warnings when tokens expire within 24 hours, and urgent alerts with re-authentication links when tokens are expired.
