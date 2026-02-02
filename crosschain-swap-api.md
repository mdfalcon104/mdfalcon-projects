# Crosschain Swap API

Production-grade cross-chain token transfer and swap API powering the Swing platform. Orchestrates routing across 20+ bridge protocols and 15+ DEX aggregators to find optimal paths for cross-chain asset transfers across EVM, Solana, Bitcoin, TON, Sui, and MultiversX networks.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Runtime | Node.js 20, TypeScript 5.3 |
| Framework | NestJS 9 |
| Database | PostgreSQL (Prisma ORM, dual-schema) |
| Cache | Redis (with in-memory fallback) |
| Queue | Graphile Worker (PostgreSQL-backed job queue) |
| Monitoring | Sentry, New Relic |
| Infra | Docker, Docker Compose |
| API Spec | OpenAPI / Swagger |

## Architecture Overview

```
src/
  bridges/              # 20 bridge protocol integrations
    across/  cbridge/  hop/  stargate/  wormhole/
    axelar/  debridge/  mayan/  synapse/  symbiosis/
    squid/  relay/  skip/  orbiter/  thorswap/
    bob-gateway/  chainlink-ccip/  swapkit/
  aggregators/          # 15 DEX aggregator integrations
    jupiter/  oneinch/  paraswap/  odos/  openocean/
    dodo/  bebop/  stonfi/  swing/  swingzap/  delta/
    platform/  bebop-gasless/
  agents/               # Smart routing agent layer (delta)
  tokens/               # Token registry, CoinGecko pricing, bridge token lists
  tasks/                # Scheduled cron jobs and fee distribution pipeline
    fees-distribution/  # Multi-step fee collection, swap, bridge, distribution
  queues/               # Async job queue with status tracking
  prisma/               # Main schema (transactions, projects)
  guards/               # Rate limiting, authentication
  interceptors/         # Wallet screening, rate-limit metrics
  sentry/               # Error tracking integration
  config/               # Environment and database configuration
prisma/                 # Transaction & project schema + migrations
prisma-platform/        # Platform-level schema (secondary database)
scripts/                # Operational cron scripts (EOA gas, token refresh)
```

## Key Modules

- **Bridge Interface** (`bridge.interface.ts`): Common abstraction that all 20 bridge integrations implement -- quote, build transaction, track status.
- **Aggregator Interface** (`aggregator.interface.ts`): Unified interface for DEX aggregators to provide swap quotes and executable calldata.
- **Fee Distribution Pipeline** (`tasks/fees-distribution/`): Multi-step orchestration for collecting protocol fees across chains, swapping to target tokens, bridging to settlement chain, and distributing to stakeholders.
- **Queue System** (`queues/`): Graphile Worker-based async processing for long-running bridge transactions with status polling.
- **Prisma Dual Schema**: Separate schemas for transaction data (`prisma/`) and platform configuration (`prisma-platform/`).

## Key Features

- Unified API across 20+ bridge protocols (Across, Celer, Hop, Stargate, Wormhole, Axelar, deBridge, Mayan, Synapse, etc.)
- 15+ DEX aggregator integrations (Jupiter, 1inch, Paraswap, Odos, OpenOcean, etc.)
- Multi-chain support: EVM chains, Solana, Bitcoin, TON, Sui, MultiversX, Cosmos (via Skip)
- Rate limiting with custom guards and metrics tracking
- Wallet screening for compliance
- Automated fee distribution across chains
- Cron-based token list refresh, EOA gas approval, and Jupiter referral account setup
- OpenAPI spec generation for client SDK codegen

## How It Works

1. **Quote Phase**: Client requests a cross-chain transfer quote. The API queries relevant bridge and aggregator modules in parallel, comparing routes by price, speed, and gas cost.
2. **Build Phase**: Once a route is selected, the API constructs the on-chain transaction(s) the user needs to sign -- this may involve approve + swap + bridge steps.
3. **Execute & Track**: After the user signs and broadcasts, the API enqueues a tracking job. The queue system polls bridge APIs for completion status and updates the PostgreSQL transaction record.
4. **Fee Settlement**: A scheduled fee distribution pipeline collects accumulated protocol fees, performs necessary swaps and bridges, and distributes funds to project wallets.
