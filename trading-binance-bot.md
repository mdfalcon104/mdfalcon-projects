# Trading Binance Bot

Automated trading bot for BSC and Solana that monitors Binance meme token rankings, detects price drops, and executes trades on PancakeSwap and Jupiter DEX. Features multi-timeframe technical analysis, dollar-cost averaging strategies, portfolio tracking, and real-time Telegram notifications.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Runtime | Node.js 20+, TypeScript |
| BSC Trading | Ethers.js v6 (PancakeSwap) |
| Solana Trading | @solana/web3.js (Jupiter) |
| Database | PostgreSQL (Prisma ORM) |
| Notifications | Telegram Bot API |
| Deployment | Railway (Docker) |
| Testing | tsx test runner |

## Architecture Overview

```
src/
  index.ts                  # BSC bot entry point
  index-solana.ts           # Solana bot entry point
  index-all.ts              # Combined multi-chain entry point
  config.ts                 # BSC configuration (thresholds, pairs, timing)
  config-solana.ts          # Solana configuration
  services/                 # Core trading services
  solana/                   # Solana-specific trading logic
  types/                    # TypeScript type definitions
  utils/                    # Shared utilities
  tests/                    # Simulation and optimization tests
    tradingSimulation.test.ts
    strategyOptimizer.test.ts
    reentrySimulation.test.ts
    volatilityOptimizer.test.ts
    htfSimulation.test.ts
    htfOptimizer.test.ts
    solanaSimulation.test.ts

prisma/
  schema.prisma             # Database schema

scripts/
  prepare.sh                # Build preparation script
```

## Key Features

- Multi-chain trading across BSC (PancakeSwap) and Solana (Jupiter DEX)
- Binance ranking monitoring for meme token price drop detection
- Multi-timeframe technical analysis with HTF (higher time frame) support
- Dollar-cost averaging (DCA) strategies for both BSC and Solana
- Portfolio tracking with position management and P&L calculation
- Liquidity filtering to avoid low-liquidity tokens
- Price verification with multiple data sources
- Token buyability checks before trade execution
- Real-time Telegram notifications for trades, alerts, and portfolio updates
- Strategy optimization through backtesting simulations
- Re-entry detection for previously traded tokens

## How It Works

1. **Token Discovery**: The bot monitors Binance meme token rankings and detects significant price drops that indicate potential buying opportunities.
2. **Analysis**: Before executing trades, the bot runs multi-timeframe technical analysis, verifies prices across multiple sources, checks token liquidity, and validates token buyability.
3. **Trade Execution**: On BSC, trades are routed through PancakeSwap using Ethers.js. On Solana, trades go through Jupiter DEX aggregator using @solana/web3.js.
4. **Position Management**: The bot tracks all open positions with entry prices, implements DCA strategies for averaging down, and monitors for exit signals.
5. **Notifications**: All trading activity, portfolio updates, and alerts are delivered to Telegram in real-time.
