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

## 下载与使用

Git仓库包含说明、账户名单、覆盖清单和示例；Parquet表及压缩来源文件从Release下载。安装uv并完成GitHub CLI登录（`gh auth login`）后，在仓库根目录执行：

```sh
uv run --no-project python download_data.py
uv sync --locked
uv run --no-sync python examples/analyze.py --output analysis-output
```

第三条命令生成`account_features.parquet/.csv`、`daily_activity.csv`、`activity_by_chain.png`和经标准化的`features.npy`。这几份输出由数据表直接计算。已有同名输出目录时脚本拒绝覆盖。

```python
from pathlib import Path
import pandas as pd

root = Path("dataset")
features = pd.read_parquet(root / "account_features.parquet")
```

`account_features.parquet`是随版本提供的账户×链描述性特征表。`examples/analyze.py`提供其可复现计算过程，使用投影读取和SQL聚合，无需一次加载所有来源JSON。生成的数值矩阵可输入scikit-learn等框架；预测目标、时间截断及训练划分由具体研究问题定义。


按列读取活动并筛选买卖观察：

```python
events = pd.read_parquet(root / "events.parquet",
    columns=["account_id", "chain", "wallet", "tx_hash", "timestamp_utc", "event_type"],
    filters=[("event_type", "in", ["buy", "sell"])])
```

## 文件导航

|路径|内容|
|---|---|
|dataset/accounts.json|账户身份、链钱包与原排名|
|dataset/metadata.json|版本、表行数、文件大小与SHA256|
|dataset/schema.md / schema.json|每张表的字段、类型及单位|
|dataset/coverage.csv / coverage.json|逐账户、钱包、来源的时间范围及分页状态|
|dataset/limitations.csv / limitations.json|具体地址、链、来源与范围限制|
|dataset/validation.json|文件完整性、引用和回放校验状态|
|dataset/evidence/|按内容摘要存储的压缩来源文件，供追溯与核查|
|examples/analyze.py|汇总、图表和数值特征矩阵示例|

所有`source_path`都相对于dataset目录。通常分析只读取Parquet；需要核查原始字段时，按schema中的来源回放说明定位。原始整数用文本保存，避免大整数经过浮点数后失真；精度未知时保持null。

## 分析口径

交易、事件、资产变化、订单和回执分别具有不同粒度，不能直接相加为交易总数。费用保留原生单位和付款方；网络gas不能替代全部账户成本。数据不附带完整净利润或策略收益标签。

样本按排行榜筛选，代表该样本的可观察活动；不能直接外推全部交易者。全期特征适合描述性分析，预测建模应按当时可知信息重算特征，按账户/时间分组验证，并保留缺失和覆盖标记。

采集方法与复现命令见[Smart Money Dataset Builder](https://github.com/anglee0323/smart-money-dataset-builder)。该仓库作为只读方法存档保留。
