# GMGN 固定五链Top100数据

最终主表入口：data/ready_provider_complete_20261012T213405Z/。
原sol/bsc/base/eth/robinhood各100，共500链钱包角色、409地址字符串；名单未变，不能解释为500个人。

|文件|内容|
|---|---|
|events.parquet|6872331条有效来源活动，保留原行|
|trades.parquet|6794248条来源buy/sell记录|
|coverage.csv/json|每钱包真实页数、最早/最新时间、空返回与来源终点|
|cohort.json|原500角色与榜单时点|
|inputs.json、inputs/|固定来源与SHA256回放|

500个原钱包来源均有成功终页，其中6个ETH返回空。每钱包的范围以coverage为准；按各链整体最早可见记录：Sol2025-10-10、BSC2024-11-25、Base2024-05-31、ETH2021-07-16、Robinhood2026-07-03。这些是各链极值，不是每个钱包都覆盖该日期。
活动总体最新记录在2026-10-11；原排行约2026-10-11 17:38UTC，下载/整理时间分别记录，未把整理日期冒充最新交易时间。

[data/ready_final_original_cohorts_v1/ethereum_index/](data/ready_final_original_cohorts_v1/ethereum_index/)是已存的独立ETH普通/转账/internal/UserOp补充源，不能与活动表直接相加成交易数。31个内部调用索引仍保留真实错误及续页，6个主来源空返回和这31项详见[DATA_GAPS.csv](DATA_GAPS.csv)。

接口终页仅证明该来源结束，未证明链创世以来所有类型的历史或全部费用。资金周转、现有余额、第一条可见交易均不是历史本金。
旧原始分页回执在v2_batch/receipts/，按SHA回放保留；较早data/ready不是当前入口。大表/raw只存本机，Git同步范围和说明。
