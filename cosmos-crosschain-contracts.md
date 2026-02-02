# Cosmos Crosschain Contracts

Cross-chain smart contracts built with Rust and CosmWasm for the Cosmos ecosystem. Includes swap router contracts for Juno and Osmosis DEXes, and a fee collector package for trustless token transfers and fee collection across IBC-connected chains.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Language | Rust |
| Framework | CosmWasm |
| IBC | Inter-Blockchain Communication Protocol |
| DEX | JunoSwap, Osmosis |
| Build | Cargo workspace |
| Optimization | LTO, single codegen unit |

## Architecture Overview

```
contracts/
  juno_swap_router/
    src/                        # JunoSwap DEX routing contract
    schema/                     # JSON schema for messages
      init_msg.json             # Instantiation message schema
      handle_msg.json           # Execute message schema
      query_msg.json            # Query message schema
      config_response.json      # Config query response schema
    examples/
      schema.rs                 # Schema generation script

  osmosis_swap_router/
    src/                        # Osmosis DEX routing contract

  swing_fee_collector/
    src/                        # Fee collection contract

packages/
  fee_collector/
    src/
      lib.rs                    # Fee collector library entry
      msg.rs                    # Message type definitions
      querier.rs                # Cross-contract query helpers

  junoswap/
    src/
      lib.rs                    # JunoSwap library entry
      msg.rs                    # JunoSwap message types

artifacts/                      # Compiled WASM binaries
Cargo.toml                      # Workspace configuration
```

## Contracts

| Contract | Description |
|----------|-------------|
| `juno_swap_router` | Routes swap transactions through JunoSwap DEX pools on Juno chain |
| `osmosis_swap_router` | Routes swap transactions through Osmosis DEX pools |
| `swing_fee_collector` | Collects and distributes cross-chain swap fees |

## Packages

| Package | Description |
|---------|-------------|
| `fee_collector` | Shared fee collection logic, message types, and cross-contract query helpers |
| `junoswap` | JunoSwap-specific message types and pool interaction helpers |

## Key Features

- Multi-DEX swap routing across Juno and Osmosis chains
- Fee collection from cross-chain swap transactions
- Cross-contract queries for pool state and pricing
- JSON schema generation for frontend integration
- Workspace-based Cargo organization for shared code
- Optimized release builds with LTO and overflow checks
- IBC-compatible message handling for cross-chain operations

## How It Works

1. **Instantiation**: Swap router contracts are deployed on their respective chains (Juno, Osmosis) with configuration specifying fee rates and authorized callers.
2. **Swap Routing**: When a cross-chain swap reaches a Cosmos chain, the router contract determines the optimal DEX pool and executes the swap through JunoSwap or Osmosis pools.
3. **Fee Collection**: The fee collector contract deducts a configured percentage from swap amounts, accumulating fees in a contract-owned account.
4. **Cross-Contract Queries**: The `fee_collector` and `junoswap` packages provide typed querier helpers for contracts to query each other's state (pool balances, fee accumulation).
5. **IBC Integration**: Contracts handle IBC messages for receiving tokens from other Cosmos chains and routing them through local DEX pools.
