# GMGN Trader Dataset

2026-10-14版，包含Solana、BSC、Base、Ethereum和Robinhood各100个链钱包角色，共500个角色、409个不同地址字符串。样本由2026-10-11约17:38 UTC的30天已实现USD利润排行确定；排名时间与交易记录覆盖时间分别保存。

[下载数据集 v2026.10.14](https://github.com/anglee0323/gmgn-top100-traders/releases/tag/v2026.10.14)

## 数据表

|Parquet表（相对dataset）|行数|记录粒度 / 关联键|
|---|---:|---|
|events.parquet|6,872,331|来源活动；`account_id, event_id`|
|ethereum_index/ethereum_transactions.parquet|626,129|ETH普通交易索引；`record_id`|
|ethereum_index/ethereum_token_transfers.parquet|675,152|ETH资产转移索引；`record_id`|
|ethereum_index/ethereum_internal_transactions.parquet|45,033|ETH内部调用索引；`record_id`|
|ethereum_index/ethereum_user_operations.parquet|123|ETH UserOp索引；`record_id`|
|account_features.parquet|500|账户×链的描述性计数与时间特征|

`events`中buy/sell观察共6,794,248条，可用`event_type IN ('buy','sell')`筛选。一个交易可以有多个事件，不应仅按交易哈希删除事件。ETH索引通过`chain, wallet`关联账户，`tx_hash`按具体来源解释；UserOp哈希与外层交易哈希不同。

## 覆盖范围

500个主地址活动来源均到达成功终页，494个非空、6个ETH来源为空。终页表明该来源可返回的分页记录结束，不保证包含链上所有交易类型。每钱包精确时间边界见`coverage.csv`。

|链|主活动中最早可见日期（UTC，整链样本极值）|
|---|---|
|Solana|2025-10-10|
|BSC|2024-11-25|
|Base|2024-05-31|
|Ethereum|2021-07-16|
|Robinhood|2026-07-03|

主活动记录的最新时间在2026-10-11。上表不是每个钱包统一的起始日期；ETH补充索引覆盖单列于`ethereum_index/coverage.json`。`limitations`包含6个主来源空结果及31个未闭合的内部调用索引范围，共37项；这些是来源范围条目，并非缺失交易数量。500个链钱包角色也不等于500名独立个人。


字段与单位见`schema.md`，逐来源时间边界见`coverage.csv`。表内路径相对于本目录，来源回放见`evidence/index.json`与schema说明。
