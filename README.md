# GMGN 五链固定Top100数据

原五链sol/bsc/base/eth/robinhood各30d已实现美元利润Top100，共500链钱包角色、409地址字符串；不是500个人。
榜单时点与活动下载时点分别保留，不更新榜单替换账户。

|位置|用途|
|---|---|
|v2_batch/receipts/链/钱包/|原始分页回执及manifest、成功/失败状态和续游标|
|v2_batch/data/链/钱包.json|已整理来源活动与范围metadata|
|data/ready_provider_complete_20261012T213405Z/cohort.json|固定500角色与原名单SHA256|
|data/ready_provider_complete_20261012T213405Z/events.parquet|全部有效来源活动，保留完整原行|
|data/ready_provider_complete_20261012T213405Z/trades.parquet|仅来源buy/sell|
|data/ready_provider_complete_20261012T213405Z/coverage.csv / json|每链钱包真实页数、行数、终点/续游标/错误|
|data/ready_provider_complete_20261012T213405Z/inputs.json|来源文件字节SHA256与行数|

主活动固定表已验收：500个原链钱包来源均有实际成功终页；events.parquet共6872331条来源活动，trades.parquet共6794248条来源buy/sell。较早的data/ready目录不作为当前主表入口。

已完成的当前Sol余额组件位于`data/ready_final_original_cohorts_v1/sol_holdings/`，258627条可读余额记录，100个Sol钱包的native/SPL/Token2022各自来源与观察时间保留。它是当前余额，不是历史本金。补充历史的联合最终目录仍在组装，不能据此称全链历史已经完整。

API终页只证明该来源不再给续游标，不等于所有链上历史已齐。原始活动不等于独立经济交易数。
已验收的`data/ready_final_original_cohorts_v1/ethereum_index/`包含1346437条实际来源记录：普通交易626129、代币转账675152、内部调用45033、UserOp123。原100个ETH钱包的普通交易、代币转账及UserOp均到来源末页；内部调用69来源末页、31接口错误保留续页。`chain_inputs.json`、`chain_coverage.json`及四张Parquet表按组件相对路径回放，来源EOF不证明链上所有历史。

公开链索引与Solana RPC补采记录仍分来源存放；不得用当前汇率当历史价格，或把native费写成美元。
本地大表/raw不提交GitHub；元数据、范围和数据字典可同步。字段与读取见 [DATA_DICTIONARY.md](DATA_DICTIONARY.md)。
