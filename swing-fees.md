# Swing Fee Collector

Solana program built with the Anchor framework for collecting and distributing cross-chain swap fees. Manages fee configuration, partner revenue sharing, token storage accounts, and treasury withdrawals for the Swing cross-chain swap platform.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Language | Rust |
| Framework | Anchor (Solana) |
| Build | Cargo, Xargo |
| Testing | TypeScript (Anchor test framework) |
| Client | @solana/web3.js |
| Deployment | Solana Mainnet |

## Architecture Overview

```
programs/swing-fees/
  Cargo.toml                # Rust dependencies and program metadata
  Xargo.toml                # Cross-compilation config for Solana BPF
  src/
    lib.rs                  # Program entry point and instruction dispatch
    errors.rs               # Custom error definitions
    events.rs               # Program event emissions
    instructions/
      mod.rs                # Instruction module exports
      initialize.rs         # Initialize fee configuration account
      change_owner.rs       # Transfer program ownership
      create_partner_share.rs   # Create partner revenue share account
      set_partner_share.rs      # Update partner share percentage
      create_token_storage.rs   # Create token storage PDA
      do_cut_fees.rs            # Execute fee collection from swaps
      withdraw_partner_fees.rs  # Partner fee withdrawal
      withdraw_swing_fees.rs    # Swing treasury fee withdrawal
    state/
      mod.rs                # State module exports
      fee_configuration.rs  # Fee configuration account structure
      cut_fees.rs           # Fee cut calculation state
      partner_share.rs      # Partner share account structure

app/                        # Client-side interaction scripts
tests/                      # Integration tests
migrations/                 # Anchor migration scripts
```

## Instructions

| Instruction | Description |
|-------------|-------------|
| `initialize` | Create and configure the fee collection program with owner and base fee rate |
| `change_owner` | Transfer program ownership to a new authority |
| `create_partner_share` | Register a new partner with a revenue share percentage |
| `set_partner_share` | Update an existing partner's revenue share percentage |
| `create_token_storage` | Create a PDA-owned token account for fee collection |
| `do_cut_fees` | Execute fee deduction from a swap transaction amount |
| `withdraw_partner_fees` | Allow partners to withdraw their accumulated fee share |
| `withdraw_swing_fees` | Allow Swing treasury to withdraw accumulated platform fees |

## Key Features

- On-chain fee collection from cross-chain swap transactions
- Configurable fee rates with owner-only administration
- Partner revenue sharing with per-partner percentage configuration
- PDA-derived token storage accounts for secure fee accumulation
- Separate withdrawal flows for partners and Swing treasury
- Event emissions for off-chain tracking and analytics
- Optimized release profile with LTO and single codegen unit

## How It Works

1. **Initialization**: The program owner calls `initialize` to create the fee configuration account, setting the base fee rate and ownership.
2. **Partner Setup**: Partners are registered via `create_partner_share` with their wallet address and revenue share percentage. Token storage PDAs are created for each supported token.
3. **Fee Collection**: When a swap occurs through the Swing platform, `do_cut_fees` is called to deduct the configured fee percentage from the transaction, splitting it between the partner share and Swing treasury.
4. **Withdrawals**: Partners call `withdraw_partner_fees` to claim their accumulated share. The Swing team calls `withdraw_swing_fees` to withdraw platform revenue from the treasury PDA.
