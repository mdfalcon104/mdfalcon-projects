# Pikafish Chinese Chess

Cross-platform Chinese chess (Xiangqi) application with a Node.js real-time multiplayer server powered by the Pikafish engine, a React web client, and a native iOS app. Supports online matchmaking, AI opponents, and game room management.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Server | Node.js, TypeScript |
| Real-time | Colyseus (WebSocket rooms) |
| Chess Engine | Pikafish (NNUE-based) |
| Web Client | React, HTML/JS |
| iOS Client | Swift, Xcode |
| Build | TypeScript compiler |

## Architecture Overview

```
src/
  server/
    index.ts                # Colyseus server entry point
    ChessRoom.ts            # Game room logic (move validation, state sync)
    Matchmaker.ts           # Player matchmaking system
    OnlineTracker.ts        # Online player tracking
    PikafishEngine.ts       # Pikafish engine process wrapper
    game/                   # Game state and rules
    storage/                # Game persistence

engine/
  pikafish                  # Pikafish binary executable
  pikafish.nnue             # Neural network evaluation file

public/
  index.html                # Web client entry page
  game.js                   # Client-side game logic and board rendering
  colyseus.js               # Colyseus client library

ios/
  PikafishChess/            # Native iOS app
  PikafishChess.xcodeproj/  # Xcode project
  project.yml               # XcodeGen project configuration

data/                       # Game data and opening books
```

## Key Features

- Real-time multiplayer Chinese chess with WebSocket synchronization
- Pikafish NNUE engine for AI opponent with configurable difficulty
- Online matchmaking with ELO-based pairing
- Game room management (create, join, spectate)
- Online player tracking and presence detection
- Cross-platform play between web and iOS clients
- Move validation and game state management on the server
- Game persistence and replay support

## How It Works

1. **Server Start**: The Colyseus server initializes, loads the Pikafish engine binary, and begins accepting WebSocket connections.
2. **Matchmaking**: Players connect and the Matchmaker pairs them based on availability or ELO rating, creating a ChessRoom for each game.
3. **Game Play**: ChessRoom manages the game state, validates moves from both players, and synchronizes state to all connected clients in real-time.
4. **AI Mode**: For single-player games, PikafishEngine wraps the Pikafish process, sending board positions via UCI protocol and receiving best move calculations.
5. **Clients**: The web client renders the board in the browser using JavaScript. The iOS client provides a native SwiftUI interface with the same WebSocket connection.
