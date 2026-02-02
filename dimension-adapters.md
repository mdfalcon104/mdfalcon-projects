# Dimension Adapters (DeFi Llama)

Community-driven adapter framework for the DeFi Llama analytics dashboard. Each adapter extracts on-chain metrics (volume, fees, revenue) from a specific DeFi protocol across multiple blockchains, enabling standardized comparison of protocol performance.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Runtime | Node.js, TypeScript 5 |
| Blockchain SDK | @defillama/sdk (multi-chain RPC, token pricing) |
| Data Sources | Subgraphs (The Graph), Direct RPC, Dune Analytics, Allium, Flipside |
| Database | PostgreSQL (via Sequelize, for caching) |
| HTTP | Axios, graphql-request |
| Math | bignumber.js, ethers v6 |

## Architecture Overview

```
dexs/                   # 679 DEX volume adapters
  uniswap/  aerodrome/  jupiter/  raydium/  ...
fees/                   # 544 fee/revenue adapters
  aave/  uniswap/  lido/  gmx/  ...
aggregators/            # 59 aggregator volume adapters
  1inch-agg/  cowswap/  paraswap/  ...
protocols/              # Protocol-specific metric adapters
aggregator-derivatives/ # Derivative aggregator adapters
aggregator-options/     # Options aggregator adapters
bridge-aggregators/     # Bridge aggregator adapters
options/                # Options protocol adapters
incentives/             # Protocol incentive trackers
nfts/                   # NFT marketplace volume adapters
users/                  # User count adapters
helpers/                # Shared utilities and base implementations
  getUniSubgraphVolume.ts   # Uniswap-fork subgraph volume fetcher
  getUniSubgraphFees.ts     # Uniswap-fork subgraph fee fetcher
  solidly.ts                # Solidly-fork adapter base
  chains.ts                 # Chain ID and slug mappings
  prices.ts                 # Token price resolution
  dune.ts / allium.ts       # SQL-based data source helpers
  compoundV2.ts             # Compound-fork adapter base
cli/                    # Testing and interactive tools
  testAdapter.ts            # Run and validate any adapter locally
  interactive.js            # Interactive adapter selector
```

## Key Modules

- **Adapter Types** (`helpers/adapters/types.ts`): Defines `SimpleAdapter`, `BaseAdapter`, `FetchOptions`, and `FetchResultV2` -- the contracts every adapter must implement.
- **Uniswap Subgraph Helpers** (`helpers/getUniSubgraph*/`): Reusable fetchers for any Uniswap V2/V3 fork, parameterized by subgraph URL and entity names.
- **Solidly Helper** (`helpers/solidly.ts`): Base adapter for Solidly/ve(3,3) forks (Velodrome, Aerodrome, Thena, etc.).
- **Chain Mapping** (`helpers/chains.ts`): Canonical chain slug-to-ID mapping shared by all adapters.
- **CLI Test Runner** (`cli/testAdapter.ts`): Validates adapter output format and data correctness before submission.

## Key Features

- 1,300+ adapters covering DEXs, lending, derivatives, bridges, NFTs, and more
- Multi-chain coverage: EVM, Solana, Tron, Cosmos, Aptos, Sui, Starknet, Near
- Standardized daily and total volume/fees/revenue output format
- Pluggable data sources: subgraphs, direct RPC calls, SQL analytics (Dune, Allium, Flipside)
- Community contribution model with CLI validation tooling
- Automatic timestamp-based backfill support

## How It Works

1. **Adapter Definition**: Each adapter exports a `SimpleAdapter` or `BreakdownAdapter` object specifying which chains it supports and a `fetch()` function for each chain.
2. **Fetch Cycle**: The DeFi Llama indexer invokes `fetch(timestamp, options)` for each adapter. The adapter queries its data source (subgraph, RPC, or SQL) and returns daily and cumulative metrics.
3. **Helpers Layer**: Common patterns (Uniswap forks, Solidly forks, Compound forks) are abstracted into parameterized helpers that handle subgraph pagination, timestamp math, and token price resolution.
4. **Validation**: Contributors run `ts-node cli/testAdapter.ts` to verify output structure and data sanity before submitting a pull request.
