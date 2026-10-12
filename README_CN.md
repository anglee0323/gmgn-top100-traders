# GMGN 五链固定Top100数据

原五链sol/bsc/base/eth/robinhood各30d已实现美元利润Top100，共500链钱包角色、409地址字符串；不是500个人。
榜单时点与活动下载时点分别保留，不更新榜单替换账户。

|位置|用途|
|---|---|
|v2_batch/receipts/链/钱包/|原始分页回执及manifest、成功/失败状态和续游标|
|v2_batch/data/链/钱包.json|已整理来源活动与范围metadata|
|data/ready/cohort.json|固定500角色与原名单SHA256|
|data/ready/events.parquet|全部有效来源活动，保留完整原行|
|data/ready/trades.parquet|仅来源buy/sell|
|data/ready/coverage.csv / json|每链钱包真实页数、行数、终点/续游标/错误|
|data/ready/inputs.json|来源文件字节SHA256与行数|

API终页只证明该来源不再给续游标，不等于所有链上历史已齐。原始活动不等于独立经济交易数。
公开链索引与Solana RPC补采记录仍分来源存放；不得用当前汇率当历史价格，或把native费写成美元。
本地大表/raw不提交GitHub；元数据、范围和数据字典可同步。字段与读取见 [DATA_DICTIONARY.md](DATA_DICTIONARY.md)。
