# 🏗️ LiveLine Architecture & Performance Benchmarks

## System Architecture

LiveLine is engineered with an event-driven, sub-second latency pipeline built on .NET 9 and SignalR:

```
┌───────────────────────────────────────┐
│     Mobile Admin App (React Native)   │
│  - 1-Tap Ball Scoring Matrix          │
│  - Biometric Local Authentication     │
│  - Offline Event Replay Queue         │
└──────────────────┬────────────────────┘
                   │ HTTPS POST /api/matches/{id}/score
                   ▼
┌───────────────────────────────────────┐
│       Nginx Reverse Proxy / SSL       │
│  - HTTP/2 & WebSocket Upgrade         │
│  - Static APK & Web Asset Caching     │
└──────────────────┬────────────────────┘
                   │
                   ▼
┌───────────────────────────────────────┐
│    ASP.NET Core 9 Minimal API Core    │
│  - ICricketScoringEngine (In-Memory)  │
│  - IBallSettlementGate (ACID Locks)   │
│  - SignalR MatchNotificationHub       │
└───────────┬───────────────────┬───────┘
            │                   │
            ▼                   ▼
┌──────────────────────┐ ┌───────────────┐
│  PostgreSQL 16 ACID  │ │ SignalR Push  │
│  - Audit Ledger Log  │ │ - 12ms to Web │
│  - Player Wallets    │ │ - No Polling  │
└──────────────────────┘ └───────┬───────┘
                                 │
                                 ▼
                     ┌───────────────────────┐
                     │   Angular 18 Client   │
                     │  - Sub-Second Ticker  │
                     │  - Micro-Bets Widget  │
                     └───────────────────────┘
```

## Latency Benchmark

| Platform / Feed | Average Latency | Slippage Window |
| :--- | :--- | :--- |
| **LiveLine SignalR WebSockets** | **< 100 ms (0.1s)** | **Zero (Auto-Lock)** |
| Traditional Polling APIs | 2,000 - 5,000 ms (2-5s) | High arbitrage risk |
| Cable / Satellite TV Broadcast | 4,000 - 7,000 ms (4-7s) | Extreme courtsiding exposure |
| OTT Streaming (Hotstar/JioCinema) | 12,000 - 25,000 ms (12-25s) | Total vulnerability |

## Dual Scoring Modes

1. **Autonomous Manual Mode:**
   - 100% free of external API subscriptions.
   - Operated via the mobile admin app.
   - Full control over ball delivery events, boundaries, extras, and wickets.

2. **Cricbuzz RapidAPI Integration Mode:**
   - Automatic ingest of external international fixtures, squads, and live scores.
   - Fallback to manual scoring at any time.

---
<sub>LiveLine Commercial Architecture Overview</sub>

