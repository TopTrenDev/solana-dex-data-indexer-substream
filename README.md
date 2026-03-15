<p align="center">
  <img src="https://img.shields.io/badge/Solana-DEX%20Indexer-9945FF?style=for-the-badge&logo=solana&logoColor=white" alt="Solana" />
  <img src="https://img.shields.io/badge/Substreams-StreamingFast-0EA5E9?style=for-the-badge" alt="Substreams" />
</p>

# ⚡ Solana DEX Data Indexer

> High-performance Solana DEX transaction indexer built with **Substreams**. Stream and parse swap transactions across multiple DEX protocols in real time—for analytics, MEV research, trading bots, and on-chain data pipelines.

---

## 📑 Table of Contents

- [Features](#-features)
- [Supported DEX Protocols](#-supported-dex-protocols)
- [Architecture](#-architecture)
- [Quick Start](#-quick-start)
- [Authentication](#-authentication)
- [Running the Indexer](#-running-the-indexer)
- [Example Output](#-example-output)
- [Use Cases](#-use-cases)
- [Publishing](#-publishing)
- [Connect](#-connect)

---

## ✨ Features

| | |
|---|---|
| **Real-time** | Stream Solana DEX swaps as they land on-chain |
| **Substreams** | Built on [StreamingFast Substreams](https://substreams.streamingfast.io) for scalable, deterministic indexing |
| **Program filtering** | Only processes transactions that touch supported DEX Program IDs |
| **Multi-protocol** | Single pipeline for multiple DEX protocols |
| **Use-case ready** | Analytics, MEV, trading signals, and data pipelines |

---

## 🏦 Supported DEX Protocols

| Protocol | Status |
|----------|--------|
| **Raydium** | ✅ |
| **Orca** | ✅ |
| **Meteora** | ✅ |
| **Pump.fun** | ✅ |
| **PumpSwap** | ✅ |
| **Bonk.fun** | ✅ |

---

## 🏗 Architecture

```
┌─────────────────┐
│  Solana Blocks  │
└────────┬────────┘
         │
         ▼
┌─────────────────────────┐
│  Substreams Engine      │
└────────┬────────────────┘
         │
         ▼
┌─────────────────────────────────┐
│  map_filtered_transactions      │  ← Filter by Program IDs, exclude voting
└────────┬────────────────────────┘
         │
         ▼
┌─────────────────────────────────┐
│  DEX Swap Parser                 │
└────────┬────────────────────────┘
         │
         ▼
┌─────────────────────────────────┐
│  Structured Trade Events         │
└────────┬────────────────────────┘
         │
         ▼
┌─────────────────────────────────┐
│  JSON / Your Data Pipeline      │
└─────────────────────────────────┘
```

### Module: `map_filtered_transactions`

- **Input:** Solana blocks from the Substreams engine  
- **Logic:** Keeps only transactions that interact with supported DEX Program IDs; voting transactions are excluded.  
- **Output:** Filtered transactions ready for the DEX swap parser.

---

## 🚀 Quick Start

1. **Install the Substreams CLI**

   [Download & install →](https://substreams.streamingfast.io)

2. **Build the project**

   ```bash
   substreams build
   ```

3. **Authenticate** (see [Authentication](#-authentication))

4. **Run** (see [Running the Indexer](#-running-the-indexer))

---

## 🔐 Authentication

Log in to the StreamingFast endpoint before running the indexer:

```bash
substreams auth
```

---

## ▶ Running the Indexer

Stream filtered DEX transactions and write them to a file:

```bash
substreams run \
  -e mainnet.sol.streamingfast.io:443 \
  substreams.yaml \
  map_filtered_transactions \
  -s 355325435 \
  -t +1 \
  > trades.jsonl
```

- `-s` — start slot  
- `-t +1` — stream one block (adjust for continuous streaming)

---

## 📤 Example Output

Each emitted event is a structured trade record (e.g. one line per trade in `trades.jsonl`):

```json
{
  "dex": "Raydium",
  "token_in": "SOL",
  "token_out": "USDC",
  "amount_in": 1.5,
  "amount_out": 150.2,
  "wallet": "abc123",
  "slot": 355325435
}
```

Use this stream for dashboards, research, or downstream pipelines.

---

## 🎯 Use Cases

- **DEX analytics dashboards** — volume, pairs, and flow in real time  
- **MEV research** — detect and analyze sandwich and arbitrage patterns  
- **Trading bots** — use on-chain swap flow as signals  
- **On-chain data pipelines** — feed data warehouses or ML models  
- **Market monitoring** — track liquidity and large trades across DEXes  

---

## 📦 Publishing

Publish this Substream to the Substreams registry:

```bash
substreams registry login
substreams registry publish
```

---

## 🔗 Connect

[![Twitter](https://img.shields.io/badge/Twitter/X-@toptrendev-000000?style=for-the-badge&logo=x&logoColor=white)]([https://x.com/toptrendev](https://x.com/intent/follow?screen_name=toptrendev))
[![Telegram](https://img.shields.io/badge/Telegram-@TopTrenDev_66-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/TopTrenDev_66)

---

*Built with [Substreams](https://substreams.streamingfast.io) for the Solana ecosystem.*
