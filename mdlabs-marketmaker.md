# MDLabs MarketMaker

NestJS-based market making bot with automated bid-ask spread management, order book monitoring, position rebalancing, and multi-exchange support for cryptocurrency trading pairs on Solana.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Runtime | Node.js 20+, TypeScript |
| Framework | NestJS 11 |
| Database | PostgreSQL (Prisma ORM) |
| Blockchain | @solana/web3.js |
| HTTP | Axios |
| Scheduling | @nestjs/schedule |
| Validation | class-validator, class-transformer |
| Testing | Jest |
| Build | SWC compiler |
| Deployment | Yarn |

## Architecture Overview

```
src/
  main.ts                   # NestJS bootstrap entry point
  app.module.ts             # Root module registration
  app.controller.ts         # Health and status endpoints
  app.service.ts            # Core application service
  config/                   # Configuration management
  common/                   # Shared utilities and constants
  database/                 # Prisma database service
  modules/
    pool/                   # Liquidity pool management
    rpc/                    # RPC connection handling
    telegram/               # Telegram notification module
    trading/                # Core trading logic
  pool/                     # Pool state tracking
  solana/                   # Solana blockchain interactions
  statistics/               # Trading statistics and reporting
  tasks/                    # Scheduled task runners
  telegram/                 # Telegram bot notifications
  tokenprice/               # Token price fetching
  trading/                  # Trade execution engine
  training-data/            # Historical data for strategy optimization
  types/                    # TypeScript type definitions

prisma/
  schema.prisma             # Database models

config.json                 # Trading pair configuration
monitor-bot.js              # External monitoring script
```

## Key Features

- Automated bid-ask spread management with configurable spread percentages
- Order book monitoring and depth analysis
- Position rebalancing based on inventory thresholds
- Solana DEX integration for on-chain market making
- Real-time token price tracking from multiple sources
- Trading statistics collection and performance reporting
- Telegram notifications for trade executions and alerts
- Scheduled tasks for periodic market analysis and rebalancing
- Database persistence for trade history and position tracking
- Training data collection for strategy optimization

## How It Works

1. **Configuration**: Trading pairs, spread targets, and position limits are defined in config.json. The NestJS app bootstraps with all modules registered.
2. **Price Discovery**: The token price module continuously fetches prices from multiple RPC endpoints and aggregator APIs to maintain accurate mid-market prices.
3. **Spread Management**: The trading module calculates optimal bid and ask prices based on the configured spread percentage, current inventory, and market conditions.
4. **Order Execution**: Orders are submitted to Solana DEX pools through the pool module, using RPC connections managed by the rpc module.
5. **Rebalancing**: Scheduled tasks monitor position inventory and trigger rebalancing trades when positions drift beyond configured thresholds.
6. **Monitoring**: All trading activity is logged to the database, statistics are computed periodically, and alerts are sent via Telegram for significant events.
