# Flash Trade Challenge

A native iOS trading application for executing token swaps on the Binance Smart Chain (BSC). Features EVM wallet generation, real-time token balance tracking, KyberSwap DEX aggregator integration for optimal swap routing, and on-device transaction signing via WalletCore.

## Tech Stack

- **Platform:** iOS (Swift 5, SwiftUI)
- **Architecture:** MVVM with ObservableObject controllers and EnvironmentObject injection
- **Blockchain:** BSC Mainnet (Chain ID 56)
- **DEX Aggregator:** KyberSwap Aggregator API (route + build endpoints)
- **RPC Provider:** Alchemy BSC RPC
- **Transaction Signing:** TrustWallet WalletCore (EthereumSigning)
- **Auth:** JWT (HS256) for backend API communication
- **Smart Contracts:** Solidity contracts (project includes /solidity directory)

## Architecture Overview

```
flash-trade-challenge/
  core/
    FlashTrade/
      FlashTradeApp.swift              # @main entry, controller injection
      Models/
        Wallet.swift                   # EVM wallet model (address, private key)
        Token.swift                    # Token model (symbol, contract address, decimals)
        SwapQuote.swift                # KyberSwap quote model (amounts, route, gas)
        TradingScreenModel.swift       # UI state model for the trading screen
      Controllers/
        WalletController.swift         # Wallet creation, storage, balance refresh
        BalanceController.swift        # Real-time BNB + token balance polling
        SwapController.swift           # Swap execution, auto-sell scheduling, limit orders
        NavigationController.swift     # Screen navigation state
      Services/
        BSCRPCService.swift            # JSON-RPC client (eth_getBalance, eth_call, eth_sendRawTransaction)
        KyperSwapService.swift         # KyberSwap route quoting and transaction building
        TransactionSigner.swift        # WalletCore EthereumSigning (EIP-155 chain replay protection)
        TransferService.swift          # Token transfer execution
        TransactionAPIService.swift    # Backend transaction recording
        JWTService.swift               # JWT token generation for API auth
      Views/
        TradingScreen.swift            # Main trading UI
        Components/
          BalanceRow.swift             # Token balance display row
          BinancePaymentSheet.swift    # Fiat on-ramp via Binance Pay
          ResetWalletSheet.swift       # Wallet reset confirmation
  api/                                 # Backend API service
  solidity/                            # Smart contract source code
  resources/                           # Design assets and documentation
```

## Key Features

- **EVM Wallet Management:** Generate and store BSC-compatible wallets with private key management. Wallet data persisted securely on device.
- **Real-Time Balance Tracking:** Polls BSC RPC for BNB (native) and ERC-20 token balances (USDT, RHEA). Handles large number precision with Decimal arithmetic for accurate display.
- **KyberSwap DEX Integration:** Two-step swap process: (1) GET `/bsc/api/v1/routes` for optimal route quoting across liquidity sources, (2) POST `/bsc/api/v1/route/build` to generate signed transaction data with slippage tolerance and deadline.
- **On-Device Transaction Signing:** Uses TrustWallet's WalletCore library for EIP-155 compliant transaction signing. Supports nonce management, gas price estimation, and custom gas limits.
- **Auto-Sell & Limit Orders:** Schedule automatic token sells at a future time or set limit order prices. Timer-based execution with cancellation support.
- **Fiat On-Ramp:** Binance Pay integration for purchasing BNB directly within the app.

## How It Works

1. **Wallet Setup:** On first launch, `WalletController` generates a new EVM wallet using WalletCore's key generation. The wallet address and private key are stored securely.
2. **Balance Monitoring:** `BalanceController` periodically calls `BSCRPCService.getBNBBalance()` (eth_getBalance) and `getTokenBalance()` (eth_call to ERC-20 balanceOf). Hex responses are converted to human-readable values using Decimal arithmetic to avoid floating-point precision loss.
3. **Swap Execution:** `SwapController` orchestrates the full swap flow:
   - Queries KyberSwap for the best route (`getSwapQuote`)
   - Builds the transaction with slippage and deadline (`buildSwapTransaction`)
   - Fetches nonce and gas price from BSC RPC
   - Signs the transaction locally using `TransactionSigner` (WalletCore EthereumSigning with BSC Chain ID 56)
   - Broadcasts via `eth_sendRawTransaction`
4. **Auto-Sell:** Users can schedule a sell at a future time. A `Timer` fires at the scheduled date and executes the sell flow. Limit orders periodically check the current price and execute when the target is reached.
