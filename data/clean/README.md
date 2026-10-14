# GMGN 五链Top100清洗数据

本目录是唯一数据入口。原Sol/BSC/Base/ETH/Robinhood各100，共500个链钱包角色、409地址字符串；原排名与名单保持，不是500个人。

## 下载完整可用包

[清洗数据 Release](https://github.com/anglee0323/gmgn-top100-traders/releases/tag/clean-20261014)包含主表、字典、覆盖、缺口和唯一一份去重压缩来源证据。安装GitHub CLI并正常登录，克隆仓库后执行：

```sh
uv run --no-project python download_data.py
```

脚本下载分卷并核对SHA256后解包；已有`data/clean`时拒绝覆盖。Git主分支保留说明和小型元数据，实际大表从Release下载。

## 主表与范围

|文件|记录数|含义|
|---|---:|---|
|events.parquet|6872331|有效来源活动，保留原行|
|trades.parquet|6794248|上述活动中的来源buy/sell观察，不能与events相加|
|ethereum_index/ethereum_transactions.parquet|626129|独立ETH普通交易索引|
|ethereum_index/ethereum_token_transfers.parquet|675152|ETH资产转移索引|
|ethereum_index/ethereum_internal_transactions.parquet|45033|ETH内部调用索引|
|ethereum_index/ethereum_user_operations.parquet|123|UserOp索引，哈希不冒充外层交易哈希|

500个主来源均有成功终页；494个非空、6个ETH来源空。全部500份成功终页原始回执随唯一证据包保留。来源终点不是链创世以来所有交易类型完整。

各链主活动全体记录的最早可见日期：Sol2025-10-10、BSC2024-11-25、Base2024-05-31、ETH2021-07-16、Robinhood2026-07-03。这些是每链极值，不是每钱包都覆盖该日期；每钱包精确起止见`coverage.csv`。
主活动最新在2026-10-11；排行约2026-10-11 17:38UTC，整理日期不能当作交易更新时间。ETH独立补充索引范围另见`ethereum_index/coverage.json`。

6个主来源空结果与31个未闭合内部调用索引逐地址列在`gaps.csv`（37项）；不是37笔丢失交易。来源未覆盖类型不填0，不推算完整成本、历史本金或净利润。

所有来源路径相对`data/clean`，不依赖旧报告目录；`evidence/`按内容SHA只存一个压缩副本。旧重复导出、画像/模型/中间数据和缓存已从当前目录移除。
字段见`DATA_DICTIONARY.md`，文件摘要与实际校验见`MANIFEST.json`、`ACCEPTANCE.json`。

下次采集用[采集仓库](https://github.com/anglee0323/smart-money-dataset-builder)的同一分页流程，EVM和Sol均按原链钱包翻页；不会自动替换名单。
