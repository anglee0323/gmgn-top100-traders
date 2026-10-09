# GMGN All-Time Top 100 Traders: Multichain Intelligence Dataset

[English](README.md) | [简体中文](README_CN.md)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Snapshot: 2026--10--10](https://img.shields.io/badge/snapshot-2026--10--10_02%3A15_UTC-blue.svg)](data/)

Open-source historical dataset covering the **All-Time Top 100 traders across 5 major blockchains** from the GMGN platform (`gmgn.ai`).

This repository is a **static historical snapshot captured on 2026-10-10 02:15:00 UTC**. It provides pure offline data artifacts for quantitative trading research, wallet profiling, backtesting, and machine learning without subjective commentary or external runtime dependencies.

Includes **500 unmasked wallet addresses (100 per chain)** across **Solana, BSC, Base, Robinhood, and Ethereum**, lifetime realized profit, win rates across multiple horizons (1d, 7d, 30d, all-time), trade frequency, profit multiplier distributions (>5x, 2x-5x, <-50%), average holding durations, multi-chain balances, genesis funding wallet provenance, and a **pre-normalized feature matrix**.

---

## Dataset Scope & Snapshot Info

* **Snapshot Timestamp**: `2026-10-10 02:15:00 UTC`
* **Data State**: Immutable offline archive
* **Tracked Entities**: 500 All-Time Leaderboard Trader Profiles (100 per chain)
* **Supported Blockchains**:
  * **Solana (`data/sol/`)**: 100 traders (native base58 addresses)
  * **BSC (`data/bsc/`)**: 100 traders (BNB Smart Chain EVM `0x...` addresses)
  * **Base (`data/base/`)**: 100 traders (Base L2 EVM `0x...` addresses)
  * **Robinhood (`data/robinhood/`)**: 100 traders (Robinhood chain EVM `0x...` addresses)
  * **Ethereum (`data/eth/`)**: 100 traders (Ethereum mainnet EVM `0x...` addresses)
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
    ├── traders_profiles.csv       # Aggregate 500 profiles across all 5 chains
    ├── traders_profiles.json      # Structured nested JSON of all 500 profiles
    ├── tokens_holdings.csv        # Aggregate tokens holdings across all 5 chains
    ├── tokens_holdings.json       # Structured token list
    ├── ml_features.csv            # Pre-normalized 500-trader feature matrix
    ├── shared_tokens_graph.json   # Multi-chain token co-trading network graph
    ├── sol/                       # Solana Top 100 dedicated folder (profiles, tokens, ML features)
    ├── bsc/                       # BSC Top 100 dedicated folder (profiles, tokens, ML features)
    ├── base/                      # Base Top 100 dedicated folder (profiles, tokens, ML features)
    ├── robinhood/                 # Robinhood Top 100 dedicated folder (profiles, tokens, ML features)
    └── eth/                       # Ethereum Top 100 dedicated folder (profiles, tokens, ML features)
```

---

## Data Schema & Field Dictionary

### 1. `data/traders_profiles.csv` (and per-chain files)

| Column | Type | Description |
| :--- | :--- | :--- |
| `rank` | Integer | Leaderboard rank within corresponding chain (1 to 100) |
| `handle` | String | Primary trader identifier (Twitter username or wallet prefix) |
| `name` | String | Display name or alias |
| `x_username` | String | Bound Twitter handle (if available) |
| `wallet_address` | String | Complete unmasked wallet address (base58 on Solana, `0x...` on EVM) |
| `address_status` | String | Verification status (`unmasked`) |
| `total_realized_profit_usd` | Float | Verified lifetime realized profit in USD |
| `primary_chain` | String | Chain origin (`solana`, `bsc`, `base`, `robinhood`, `ethereum`) |
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
| `fund_amount_sol` | Float | Initial funding deposit amount (native coin) |
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
| `chain` | String | Blockchain network (`solana`, `bsc`, `base`, `robinhood`, `ethereum`) |
| `usd_value` | Float | Position or execution value in USD |
| `portfolio_share_pct` | Float | Portfolio allocation percentage |
| `co_holders_count` | Integer | Count of other top traders holding or trading this token |

---

### 3. `data/ml_features.csv`

Pre-computed numerical feature matrix for machine learning pipelines (clustering, win-rate prediction, copy-trading risk scoring):

| Feature Name | Description |
| :--- | :--- |
| `handle` | Trader handle |
| `rank` | Chain rank index |
| `wallet_address` | Unmasked wallet address |
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

# Load aggregate 500 trader profiles across all chains
all_profiles = pd.read_csv("data/traders_profiles.csv")
print(f"Total profiles: {len(all_profiles)}")

# Load dedicated BSC dataset
bsc_profiles = pd.read_csv("data/bsc/traders_profiles.csv")
print(f"BSC Top 1: {bsc_profiles.iloc[0]['name']} - PnL: ${bsc_profiles.iloc[0]['realized_profit_all_usd']:,.2f}")

# Compare win rates across chains
chain_summary = all_profiles.groupby("primary_chain").agg({
    "realized_profit_all_usd": "mean",
    "winrate_all_pct": "mean",
    "trades_count_all": "mean"
})
print(chain_summary)
```

---

## License

This dataset is released under the [MIT License](LICENSE).
