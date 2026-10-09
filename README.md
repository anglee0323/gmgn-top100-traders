# GMGN All-Time Top 100 Traders: Solana On-Chain Intelligence Dataset

[English](README.md) | [简体中文](README_CN.md)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Snapshot: 2026--10--10](https://img.shields.io/badge/snapshot-2026--10--10_01%3A55_UTC-blue.svg)](data/)

Open-source historical dataset covering the **All-Time Top 100 traders on Solana** from the GMGN platform (`gmgn.ai`).

This repository is a **static historical snapshot captured on 2026-10-10 01:55:00 UTC**. It provides pure offline data artifacts for quantitative trading research, wallet profiling, backtesting, and machine learning without subjective commentary or external runtime dependencies.

Includes **100% unmasked Solana wallet addresses**, lifetime realized profit, win rates across multiple horizons (1d, 7d, 30d, all-time), trade frequency, profit multiplier distributions (>5x, 2x-5x, <-50%), average holding durations, multi-chain balances, genesis funding wallet provenance, and a **pre-normalized feature matrix**.

---

## Dataset Scope & Snapshot Info

* **Snapshot Timestamp**: `2026-10-10 01:55:00 UTC`
* **Data State**: Immutable offline archive
* **Tracked Entities**: 100 All-Time Leaderboard Trader Profiles
* **Primary Blockchain**: Solana (native base58 addresses)
* **Tracked Cross-Chain Balances**: Solana, Ethereum, Robinhood, Arc, Monad, Tron
* **Key Metrics**: Lifetime Realized PnL, Multi-horizon Win Rates, Multiplier Breakdown, Genesis Funder Address & Funding Transaction Hash

---

## Repository Structure

```
gmgn-top100-traders/
├── README.md                      # English documentation (this file)
├── README_CN.md                   # Chinese documentation (中文说明)
├── pyproject.toml                 # Project metadata
├── LICENSE                        # MIT License
└── data/
    ├── traders_profiles.csv       # Tabular profiles (rank, unmasked wallets, lifetime PnL, win rates, funder)
    ├── traders_profiles.json      # Structured nested JSON of all trader profiles
    ├── tokens_holdings.csv        # Traded tokens and positions (trader, symbol, contract, USD value)
    ├── tokens_holdings.json       # Structured token list
    ├── ml_features.csv            # Pre-normalized feature matrix for clustering and classification
    └── shared_tokens_graph.json   # Inter-trader co-trading network graph
```

---

## Data Schema & Field Dictionary

### 1. `data/traders_profiles.csv`

| Column | Type | Description |
| :--- | :--- | :--- |
| `rank` | Integer | Overall all-time ranking index (1 to 100) |
| `handle` | String | Primary trader identifier (Twitter username or wallet prefix) |
| `name` | String | Display name or alias |
| `x_username` | String | Bound Twitter handle (if available) |
| `wallet_address` | String | Complete unmasked Solana native wallet address (base58) |
| `address_status` | String | Verification status (`unmasked`) |
| `total_realized_profit_usd` | Float | Verified lifetime realized profit in USD |
| `primary_chain` | String | Primary trading chain (`solana`) |
| `sol_balance` | Float | Native Solana balance tracked |
| `eth_balance` | Float | Cross-chain Ethereum balance tracked |
| `robinhood_balance` | Float | Robinhood chain balance tracked |
| `arc_balance` | Float | Arc balance tracked |
| `monad_balance` | Float | Monad balance tracked |
| `trx_balance` | Float | Tron balance tracked |
| `active_chains_count` | Integer | Number of active chains with positive balance |
| `tags` | List | Platform classification tags (`kol`, `smart_degen`, `axiom`, etc.) |
| `realized_profit_all_usd` | Float | Lifetime realized profit in USD |
| `realized_profit_30d_usd` | Float | 30-day realized profit in USD |
| `realized_profit_7d_usd` | Float | 7-day realized profit in USD |
| `realized_profit_1d_usd` | Float | 24-hour realized profit in USD |
| `winrate_all_pct` | Float | Lifetime trading win rate percentage |
| `winrate_30d_pct` | Float | 30-day win rate percentage |
| `winrate_7d_pct` | Float | 7-day win rate percentage |
| `winrate_1d_pct` | Float | 24-hour win rate percentage |
| `volume_30d_usd` | Float | 30-day trading volume in USD |
| `volume_7d_usd` | Float | 7-day trading volume in USD |
| `volume_1d_usd` | Float | 24-hour trading volume in USD |
| `trades_count_all` | Integer | Total lifetime transactions count (buy + sell) |
| `trades_count_30d` | Integer | 30-day transaction count |
| `trades_count_7d` | Integer | 7-day transaction count |
| `buy_count_all` | Integer | Total lifetime buy executions |
| `sell_count_all` | Integer | Total lifetime sell executions |
| `tokens_traded_all` | Integer | Total unique token contracts traded |
| `avg_holding_period_sec` | Float | Average position holding duration in seconds |
| `pnl_gt_5x_count` | Integer | Count of positions yielding returns greater than 5x |
| `pnl_2x_5x_count` | Integer | Count of positions yielding returns between 2x and 5x |
| `pnl_lt_minus_dot5_count` | Integer | Count of positions resulting in loss greater than 50% |
| `fund_from_address` | String | Genesis funding wallet address (initial fund provenance) |
| `fund_tx_hash` | String | Genesis funding transaction hash |
| `fund_amount_sol` | Float | Initial funding deposit amount in SOL |
| `followers_x` | Integer | Twitter follower count |
| `archetype` | String | Quantitative behavioral archetype classification |
| `hhi_concentration` | Float | Herfindahl-Hirschman Index portfolio concentration |
| `chain_entropy` | Float | Multichain distribution entropy |
| `network_co_holding_score` | Float | Average network co-trading overlap score |

---

### 2. `data/tokens_holdings.csv`

| Column | Type | Description |
| :--- | :--- | :--- |
| `trader_handle` | String | Associated trader handle |
| `symbol` | String | Token ticker symbol |
| `token_address` | String | On-chain token mint / contract address |
| `chain` | String | Blockchain network (`solana`) |
| `usd_value` | Float | Position or execution value in USD |
| `portfolio_share_pct` | Float | Portfolio allocation percentage |
| `co_holders_count` | Integer | Count of other top 100 traders holding or trading this token |

---

### 3. `data/ml_features.csv`

Pre-computed numerical feature matrix for machine learning pipelines (clustering, win-rate prediction, copy-trading risk scoring):

| Feature Name | Description |
| :--- | :--- |
| `handle` | Trader handle |
| `rank` | Leaderboard rank index |
| `wallet_address` | Unmasked Solana wallet address |
| `log_realized_profit` | $\log_{10}(\text{Realized Profit USD})$ |
| `winrate_all_pct` | Lifetime win rate percentage |
| `winrate_30d_pct` | 30-day win rate percentage |
| `winrate_7d_pct` | 7-day win rate percentage |
| `trades_count_all` | Total transaction volume count |
| `tokens_traded_all` | Number of distinct tokens traded |
| `avg_holding_period_sec` | Position holding time in seconds |
| `runner_5x_ratio` | Ratio of >5x runners to total tokens traded |
| `heavy_loss_ratio` | Ratio of <-50% losses to total tokens traded |
| `followers_x` | Twitter audience reach |
| `hhi_concentration` | Position concentration index |
| `chain_entropy` | Multi-chain distribution entropy |
| `network_co_holding_score` | Syndicate co-holding index |
| `archetype` | Categorical archetype label |

---

## Quick Start & Usage Examples

### Python (Pandas)

```python
import pandas as pd

# Load trader profiles
profiles = pd.read_csv("data/traders_profiles.csv")

# Filter high win-rate traders with >$1M profit
elite_traders = profiles[
    (profiles["realized_profit_all_usd"] > 1_000_000) &
    (profiles["winrate_all_pct"] > 60.0)
]
print(elite_traders[["rank", "name", "wallet_address", "realized_profit_all_usd", "winrate_all_pct"]])

# Inspect genesis funding sources
funding_clusters = profiles.groupby("fund_from_address")["wallet_address"].count()
print(funding_clusters[funding_clusters > 1])
```

### Python (JSON)

```python
import json

with open("data/traders_profiles.json", "r") as f:
    traders = json.load(f)

print(f"Total profiles loaded: {len(traders)}")
print(f"Top 1 Trader: {traders[0]['name']} ({traders[0]['wallet_address']})")
```

---

## License

This dataset is released under the [MIT License](LICENSE).
