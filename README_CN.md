# GMGN 历史全时期盈利 Top 100 交易员：Solana 链上情报数据集

[English](README.md) | [简体中文](README_CN.md)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Snapshot: 2026--10--10](https://img.shields.io/badge/snapshot-2026--10--10_01%3A55_UTC-blue.svg)](data/)

开源离线历史数据集，覆盖 GMGN 平台（`gmgn.ai`）**Solana 链上历史全时期（All-Time）盈利 Top 100 顶级交易员**。

本仓库为 **2026-10-10 01:55:00 UTC 抓取固化的静态离线历史数据存档**。提供纯净的原始数据与字段字典，专供量化交易分析、钱包画像、历史回测与机器学习模型训练，不包含主观结论，亦无任何外部运行时脚本依赖。

涵盖 **100% 未脱敏真实 Solana 钱包地址**、全生命周期已实现盈亏（Realized PnL）、多时间窗口胜率（24小时、7天、30天、全时期）、交易频次、盈亏倍数分布（>5倍、2至5倍、<-50%）、平均持仓时长、多链资产分布、创世入金源头钱包归因（Genesis Funding Wallet）以及**预归一化机器学习特征矩阵**。

---

## 数据集范围与快照信息

* **快照时间戳**：`2026-10-10 01:55:00 UTC`
* **数据状态**：固化离线存档
* **实体数量**：100 个全时期排行榜交易员档案
* **主要公链**：Solana（原生 base58 格式）
* **跨链资产覆盖**：Solana、Ethereum、Robinhood、Arc、Monad、Tron
* **核心指标**：全生命周期已实现盈亏、多周期胜率、倍数收益分布、创世入金钱包地址与入金交易哈希

---

## 仓库结构

```
gmgn-top100-traders/
├── README.md                      # 英文文档
├── README_CN.md                   # 中文文档（本文件）
├── pyproject.toml                 # 项目元数据
├── LICENSE                        # MIT 开源许可证
└── data/
    ├── traders_profiles.csv       # 表格档案（排名、真实钱包、已实现盈亏、胜率、入金源）
    ├── traders_profiles.json      # 全量嵌套结构化 JSON 档案
    ├── tokens_holdings.csv        # 交易代币与持仓（交易员、代币符号、合约地址、USD价值）
    ├── tokens_holdings.json       # 结构化代币列表
    ├── ml_features.csv            # 预归一化机器学习特征矩阵
    └── shared_tokens_graph.json   # 交易员间代币协同交易/持仓网络图
```

---

## 数据模式与字段字典

### 1. `data/traders_profiles.csv`

| 字段名称 | 类型 | 描述 |
| :--- | :--- | :--- |
| `rank` | 整数 | 全时期综合排名序号（1 至 100） |
| `handle` | 字符串 | 交易员主要标识（推特用户名或钱包前缀） |
| `name` | 字符串 | 显示名称或备注 |
| `x_username` | 字符串 | 绑定的推特账号（如有） |
| `wallet_address` | 字符串 | 完整未脱敏 Solana 原生钱包地址（base58） |
| `address_status` | 字符串 | 验证状态（`unmasked`） |
| `total_realized_profit_usd` | 浮点数 | 验证的全生命周期已实现盈亏（美元） |
| `primary_chain` | 字符串 | 主交易公链（`solana`） |
| `sol_balance` | 浮点数 | 追踪的 Solana 原生代币余额 |
| `eth_balance` | 浮点数 | 追踪的 Ethereum 跨链资产余额 |
| `robinhood_balance` | 浮点数 | 追踪的 Robinhood 链资产余额 |
| `arc_balance` | 浮点数 | 追踪的 Arc 链资产余额 |
| `monad_balance` | 浮点数 | 追踪的 Monad 链资产余额 |
| `trx_balance` | 浮点数 | 追踪的 Tron 链资产余额 |
| `active_chains_count` | 整数 | 余额大于 1 的活跃公链数量 |
| `tags` | 列表 | 平台标签（如 `kol`、`smart_degen`、`axiom` 等） |
| `realized_profit_all_usd` | 浮点数 | 全生命周期已实现盈亏（美元） |
| `realized_profit_30d_usd` | 浮点数 | 近 30 天已实现盈亏（美元） |
| `realized_profit_7d_usd` | 浮点数 | 近 7 天已实现盈亏（美元） |
| `realized_profit_1d_usd` | 浮点数 | 近 24 小时已实现盈亏（美元） |
| `winrate_all_pct` | 浮点数 | 全生命周期交易胜率百分比 |
| `winrate_30d_pct` | 浮点数 | 近 30 天交易胜率百分比 |
| `winrate_7d_pct` | 浮点数 | 近 7 天交易胜率百分比 |
| `winrate_1d_pct` | 浮点数 | 近 24 小时交易胜率百分比 |
| `volume_30d_usd` | 浮点数 | 近 30 天交易量（美元） |
| `volume_7d_usd` | 浮点数 | 近 7 天交易量（美元） |
| `volume_1d_usd` | 浮点数 | 近 24 小时交易量（美元） |
| `trades_count_all` | 整数 | 全生命周期交易笔数（买入 + 卖出） |
| `trades_count_30d` | 整数 | 近 30 天交易笔数 |
| `trades_count_7d` | 整数 | 近 7 天交易笔数 |
| `buy_count_all` | 整数 | 全生命周期买入操作次数 |
| `sell_count_all` | 整数 | 全生命周期卖出操作次数 |
| `tokens_traded_all` | 整数 | 交易过的不同代币合约总数 |
| `avg_holding_period_sec` | 浮点数 | 平均持仓周期时长（秒） |
| `pnl_gt_5x_count` | 整数 | 收益超过 5 倍的代币笔数 |
| `pnl_2x_5x_count` | 整数 | 收益在 2 至 5 倍之间的代币笔数 |
| `pnl_lt_minus_dot5_count` | 整数 | 亏损超过 50% 的代币笔数 |
| `fund_from_address` | 字符串 | 创世入金钱包地址（资金源头归因） |
| `fund_tx_hash` | 字符串 | 创世入金交易哈希 |
| `fund_amount_sol` | 浮点数 | 初始入金金额（SOL） |
| `followers_x` | 整数 | 推特粉丝数量 |
| `archetype` | 字符串 | 定量行为原型画像分类 |
| `hhi_concentration` | 浮点数 | 赫芬达尔-赫希曼持仓集中度指数 |
| `chain_entropy` | 浮点数 | 多链分布香农熵 |
| `network_co_holding_score` | 浮点数 | 协同交易重合度网络评分 |

---

### 2. `data/tokens_holdings.csv`

| 字段名称 | 类型 | 描述 |
| :--- | :--- | :--- |
| `trader_handle` | 字符串 | 关联交易员标识 |
| `symbol` | 字符串 | 代币代码符号 |
| `token_address` | 字符串 | 链上代币合约地址 / Mint 地址 |
| `chain` | 字符串 | 所属区块链（`solana`） |
| `usd_value` | 浮点数 | 持仓或交易金额（美元） |
| `portfolio_share_pct` | 浮点数 | 组合占比百分比 |
| `co_holders_count` | 整数 | 前 100 榜单中同样交易/持有该代币的其他交易员数量 |

---

### 3. `data/ml_features.csv`

面向机器学习管线（聚类分群、胜率预测、跟单风险评分）预计算的数值特征矩阵：

| 特征名称 | 描述 |
| :--- | :--- |
| `handle` | 交易员标识 |
| `rank` | 榜单排名序号 |
| `wallet_address` | 未脱敏 Solana 钱包地址 |
| `log_realized_profit` | $\log_{10}(\text{已实现盈亏 USD})$ |
| `winrate_all_pct` | 全生命周期胜率百分比 |
| `winrate_30d_pct` | 30 天胜率百分比 |
| `winrate_7d_pct` | 7 天胜率百分比 |
| `trades_count_all` | 累计交易操作次数 |
| `tokens_traded_all` | 交易过的代币数量 |
| `avg_holding_period_sec` | 平均持仓时长（秒） |
| `runner_5x_ratio` | 5倍以上高收益代币占比 |
| `heavy_loss_ratio` | 50%以上重度亏损代币占比 |
| `followers_x` | 推特影响力规模 |
| `hhi_concentration` | 持仓集中度指数 |
| `chain_entropy` | 多链分布熵 |
| `network_co_holding_score` | 协同持仓网络指数 |
| `archetype` | 行为原型分类标签 |

---

## 快速使用示例

### Python (Pandas)

```python
import pandas as pd

# 读取交易员档案
profiles = pd.read_csv("data/traders_profiles.csv")

# 筛选历史盈利超 100 万美元且全周期胜率 > 60% 的顶尖交易员
elite_traders = profiles[
    (profiles["realized_profit_all_usd"] > 1_000_000) &
    (profiles["winrate_all_pct"] > 60.0)
]
print(elite_traders[["rank", "name", "wallet_address", "realized_profit_all_usd", "winrate_all_pct"]])

# 聚类排查共享入金资金源的关联钱包团伙
funding_clusters = profiles.groupby("fund_from_address")["wallet_address"].count()
print(funding_clusters[funding_clusters > 1])
```

### Python (JSON)

```python
import json

with open("data/traders_profiles.json", "r", encoding="utf-8") as f:
    traders = json.load(f)

print(f"载入档案数: {len(traders)}")
print(f"Top 1 交易员: {traders[0]['name']} ({traders[0]['wallet_address']})")
```

---

## 许可证

本数据集依据 [MIT 许可证](LICENSE) 开源发布。
