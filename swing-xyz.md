# Swing.xyz

A monorepo web3 platform for cross-chain token swaps, staking, and portfolio management. Provides a production Next.js application, a publishable SDK, a React UI widget library, and multi-chain wallet connectors supporting EVM, Solana, IBC, TON, Tron, Bitcoin, and Sui.

## Tech Stack

- **Monorepo Tooling:** Yarn 4 Workspaces + Turborepo
- **Frontend:** Next.js 15 (App Router, Turbopack), React 19, TailwindCSS 3
- **State Management:** Jotai (atomic), TanStack React Query
- **Blockchain:** viem 2.x, wagmi 2.x, ethers BigNumber
- **Wallet Support:** WalletConnect, Phantom, Backpack, Ctrl, OKX, Keplr, TronLink, TonConnect
- **UI Components:** Radix UI, shadcn/ui, FontAwesome, Recharts
- **Build:** tsup (SDK/UI/wallets), Next.js (app)
- **Testing:** Vitest, Testing Library
- **Linting:** Biome
- **Monitoring:** Sentry, Mixpanel, Vercel Analytics
- **CI/CD:** Changesets for versioning, Vercel for deployment

## Architecture Overview

```
swing.xyz/
  package.json          # Root workspace config (Yarn 4)
  turbo.json            # Turborepo task pipeline
  biome.json            # Linting/formatting rules
  apps/
    app/                # Production Next.js 15 application
      src/
        app/            # App Router pages
          (header-layout)/
            page.tsx            # Home page
            swap/page.tsx       # Cross-chain swap UI
            stake/page.tsx      # Staking interface
            tokens/[chainSlug]/[tokenSlug]/  # Token detail pages
        components/
          Provider.tsx          # Root provider (React Query, wagmi, wallets)
          ConnectWallet.tsx     # Wallet connection button
          RewardsDialog.tsx     # Rewards/points system
          header.tsx            # Navigation header
          search.tsx            # Token/chain search (cmdk)
          widget/index.tsx      # Swap widget integration
          tokens/               # Token info, audit, compare, trending
          ui/                   # shadcn components (button, card, dialog, etc.)
        hooks/                  # useQueryParams, useLocalStorage, useMediaQuery
        lib/
          coingecko/            # CoinGecko API client (OpenAPI typed)
          goplus/               # GoPlus security audit client
          analytics.ts          # Mixpanel tracking
    demo/               # Demo/sandbox application
    galaxy/             # Galaxy campaign integration
    stake/              # Standalone staking application
  packages/
    sdk/                # @swing.xyz/sdk - Core cross-chain swap SDK
      src/
        lib/
          sdk/index.ts          # SwingSDK class (quote, send, claim, status)
          types.ts              # Chain, token, route, transaction types
          utils.ts              # Helper utilities
          errors.ts             # Typed SDK errors
          wallet/adapter.ts     # Wallet adapter interface
          helpers/              # Chain helpers, logger, reporter, waitForTxStatus
    ui/                 # @swing.xyz/ui - React widget library
      src/
        widgets/
          Swap/                 # Swap widget with route selection
          Stake/                # Staking widget
          Gas/                  # Gas estimation widget
          Withdraw/             # Withdrawal widget
          Rewards/              # Points/rewards system
        components/
          ChainTokenSelect/     # Chain + token picker
          RouteSelect/          # Route comparison (fees, time, details)
          TransactionDetails/   # Transfer steps, claim button
          Transactions/         # Transaction history list
          TransferStatus/       # Live transfer status tracking
          WalletConnect/        # Wallet connection UI
          WalletSelect/         # Multi-wallet selector
          Provider/             # SDK + wallet provider wrapper
        hooks/                  # useBalance, useRoute, useQuoteState, useSwingSdk, etc.
        store/                  # Jotai atoms (transfer, balance, wallet, transactions)
    wallets/            # @swing.xyz/wallets - Multi-chain wallet connectors
      src/
        adapters/               # Chain-specific adapters
          evm/                  # EVM adapter (viem/wagmi)
          solana/               # Solana adapter
          ibc/                  # Cosmos IBC adapter
          ton/                  # TON adapter
          tron/                 # Tron adapter
          sui/                  # Sui adapter
          bitcoin/              # Bitcoin adapter
        connectors/             # Wallet-specific connectors
          walletconnect/        # WalletConnect (EVM, Solana, IBC, Bitcoin, Tron)
          phantom/              # Phantom (EVM, Solana, Bitcoin, Sui)
          backpack/             # Backpack (EVM, Solana)
          ctrl/                 # Ctrl (EVM, Solana, Bitcoin)
          okx/                  # OKX (EVM, Solana, Tron, Bitcoin)
          keplr/                # Keplr (IBC/Cosmos)
          tron-link/            # TronLink (Tron)
          ton-connect/          # TonConnect (TON)
        connections/            # Connection management, reconnect logic
        hooks/                  # useConnect, useConnection, useConnectors
        store.ts                # Wallet state management
    design-system/      # Shared design tokens and styles
    tailwind/           # Shared TailwindCSS configuration
    tsconfig/           # Shared TypeScript configuration
```

## Key Features

- **Cross-Chain Swaps:** Quote and execute token swaps across 30+ chains (EVM, Solana, Cosmos, TON, Tron, Bitcoin, Sui) via the Swing platform API. Route selection with fee comparison.
- **Multi-Wallet Support:** Connect 8+ wallet types across 7 blockchain ecosystems. Automatic chain detection and switching.
- **Staking:** Stake tokens with real-time APY display and claim functionality.
- **Token Intelligence:** CoinGecko price data, GoPlus security audits, trending tokens, and cross-chain token comparison.
- **Rewards System:** Points-based rewards with check-ins, referrals, leaderboard, and transaction-based earning.
- **Publishable SDK:** `@swing.xyz/sdk` provides a standalone TypeScript SDK for integrating cross-chain swaps into any application.
- **Embeddable Widgets:** `@swing.xyz/ui` exports React components (Swap, Stake, Gas, Rewards) that can be embedded in third-party apps.

## How It Works

1. **Swap Flow:** The user connects a wallet via `@swing.xyz/wallets`, selects source/destination chains and tokens, and enters an amount. The `@swing.xyz/sdk` fetches a quote from the Swing platform API, which returns available routes with fees and estimated times. The user selects a route, the SDK builds the transaction, the wallet signs it, and the SDK monitors cross-chain transfer status via polling.
2. **Wallet Connection:** The `wallets` package provides chain-specific adapters (EVM, Solana, IBC, etc.) and wallet-specific connectors (WalletConnect, Phantom, etc.). Each connector handles authentication, signing, and chain switching for its ecosystem. The connection state is managed via Jotai atoms.
3. **Build Pipeline:** Turborepo orchestrates builds with dependency ordering. Internal packages use tsup for bundling. The Next.js app uses Turbopack for development. Changesets manage semantic versioning across all publishable packages.
