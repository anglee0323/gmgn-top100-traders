# 数据字典

时间按UTC读取。原始整数与精确小数以文本保存；金额计算使用Decimal。null表示未知或来源未提供。

资产键使用链+合约/mint，账户键使用表所列组合键。相同交易中的多个事件、执行腿和资产变化分别保留。

## events.parquet

|字段|类型|含义|
|---|---|---|
|dataset|string|数据集标识。|
|account_id|string|数据集内稳定账户/链钱包角色键；FOMO的original_fomo:rank:handle为本地标识。|
|rank|int32|冻结样本中的原排名；GMGN排名在每链内部定义。|
|handle|string|来源账户用户名，可为空。|
|chain|string|链标识；GMGN使用sol/bsc/base/eth/robinhood；通用EVM来源表使用十进制chain ID文本。|
|wallet|string|被查询/关联的钱包；回执表可为空，账户成员关系见memberships_json。|
|event_id|string|来源活动事件键；同一交易可有多条活动。|
|tx_hash|string|EVM交易哈希或Solana签名；UserOp来源中为用户操作哈希，需在raw_json读取外层交易。|
|timestamp_utc|string|来源时间，ISO 8601 UTC文本；未知为null，读取时显式转换为UTC时间。|
|event_type|string|来源活动分类，例如buy/sell；它是观察分类。|
|asset_address|string|链内资产合约或mint标识；原生资产可为空。|
|asset_symbol|string|来源显示符号，不作为资产唯一键。|
|token_amount_provider|string|来源提供的资产数量文本，保留来源单位。|
|quote_asset_address|string|报价/对手资产的链内合约或mint。|
|quote_amount_provider|string|来源提供的报价资产数量文本。|
|gas_native_provider|string|来源报告的原生币网络费用文本，可缺失。|
|gas_usd_provider|string|来源报告的美元网络费用文本，可缺失。|
|cost_usd_provider|string|来源的USD数值字段，不能据此推定完整成本基础或净利润。|
|source_path|string|相对dataset根目录的压缩来源文件路径。|
|source_sha256|string|来源文件原始字节SHA256；先解压存储层再校验。|
|source_pointer|string|解码来源JSON后定位原记录的RFC 6901 JSON指针。|
|raw_json|string|完整来源记录的JSON文本。|

## ethereum_index/ethereum_internal_transactions.parquet

|字段|类型|含义|
|---|---|---|
|record_id|string|来源记录的稳定标识，不能替代交易哈希。|
|source|string|来源接口/记录类型。|
|chain|string|链标识；GMGN使用sol/bsc/base/eth/robinhood；通用EVM来源表使用十进制chain ID文本。|
|wallet|string|被查询/关联的钱包；回执表可为空，账户成员关系见memberships_json。|
|memberships_json|string|关联账户/链钱包角色列表的JSON文本；回执可同时属于多个账户。|
|tx_hash|string|EVM交易哈希或Solana签名；UserOp来源中为用户操作哈希，需在raw_json读取外层交易。|
|timestamp_utc|string|来源时间，ISO 8601 UTC文本；未知为null，读取时显式转换为UTC时间。|
|asset_address|string|链内资产合约或mint标识；原生资产可为空。|
|token_account|string|来源提供的代币账户，可缺失。|
|amount_raw|string|来源资产原始整数文本；避免浮点转换。|
|decimals_source|int32|来源资产精度，0是有效值，未知为null。|
|amount_basic|string|按来源精度换算的十进制数量文本；精度未知时为null。|
|amount_ui_source|string|来源显示数量文本，仅保留观察值。|
|fee_raw|string|来源原始费用整数文本；类型及单位由fee_type_source/fee_unit指定。|
|fee_unit|string|费用计量单位，例如wei；不是美元。|
|fee_type_source|string|费用的来源含义/计算口径。|
|fee_payer|string|执行费用付款地址，可能为bundler，不一定等于样本钱包。|
|context_slot|string|来源区块高度/上下文槽位文本，具体语义按source解释。|
|source_path|string|相对dataset根目录的压缩来源文件路径。|
|source_sha256|string|来源文件原始字节SHA256；先解压存储层再校验。|
|source_pointer|string|解码来源JSON后定位原记录的RFC 6901 JSON指针。|
|raw_json|string|完整来源记录的JSON文本。|

## ethereum_index/ethereum_token_transfers.parquet

|字段|类型|含义|
|---|---|---|
|record_id|string|来源记录的稳定标识，不能替代交易哈希。|
|source|string|来源接口/记录类型。|
|chain|string|链标识；GMGN使用sol/bsc/base/eth/robinhood；通用EVM来源表使用十进制chain ID文本。|
|wallet|string|被查询/关联的钱包；回执表可为空，账户成员关系见memberships_json。|
|memberships_json|string|关联账户/链钱包角色列表的JSON文本；回执可同时属于多个账户。|
|tx_hash|string|EVM交易哈希或Solana签名；UserOp来源中为用户操作哈希，需在raw_json读取外层交易。|
|timestamp_utc|string|来源时间，ISO 8601 UTC文本；未知为null，读取时显式转换为UTC时间。|
|asset_address|string|链内资产合约或mint标识；原生资产可为空。|
|token_account|string|来源提供的代币账户，可缺失。|
|amount_raw|string|来源资产原始整数文本；避免浮点转换。|
|decimals_source|int32|来源资产精度，0是有效值，未知为null。|
|amount_basic|string|按来源精度换算的十进制数量文本；精度未知时为null。|
|amount_ui_source|string|来源显示数量文本，仅保留观察值。|
|fee_raw|string|来源原始费用整数文本；类型及单位由fee_type_source/fee_unit指定。|
|fee_unit|string|费用计量单位，例如wei；不是美元。|
|fee_type_source|string|费用的来源含义/计算口径。|
|fee_payer|string|执行费用付款地址，可能为bundler，不一定等于样本钱包。|
|context_slot|string|来源区块高度/上下文槽位文本，具体语义按source解释。|
|source_path|string|相对dataset根目录的压缩来源文件路径。|
|source_sha256|string|来源文件原始字节SHA256；先解压存储层再校验。|
|source_pointer|string|解码来源JSON后定位原记录的RFC 6901 JSON指针。|
|raw_json|string|完整来源记录的JSON文本。|

## ethereum_index/ethereum_transactions.parquet

|字段|类型|含义|
|---|---|---|
|record_id|string|来源记录的稳定标识，不能替代交易哈希。|
|source|string|来源接口/记录类型。|
|chain|string|链标识；GMGN使用sol/bsc/base/eth/robinhood；通用EVM来源表使用十进制chain ID文本。|
|wallet|string|被查询/关联的钱包；回执表可为空，账户成员关系见memberships_json。|
|memberships_json|string|关联账户/链钱包角色列表的JSON文本；回执可同时属于多个账户。|
|tx_hash|string|EVM交易哈希或Solana签名；UserOp来源中为用户操作哈希，需在raw_json读取外层交易。|
|timestamp_utc|string|来源时间，ISO 8601 UTC文本；未知为null，读取时显式转换为UTC时间。|
|asset_address|string|链内资产合约或mint标识；原生资产可为空。|
|token_account|string|来源提供的代币账户，可缺失。|
|amount_raw|string|来源资产原始整数文本；避免浮点转换。|
|decimals_source|int32|来源资产精度，0是有效值，未知为null。|
|amount_basic|string|按来源精度换算的十进制数量文本；精度未知时为null。|
|amount_ui_source|string|来源显示数量文本，仅保留观察值。|
|fee_raw|string|来源原始费用整数文本；类型及单位由fee_type_source/fee_unit指定。|
|fee_unit|string|费用计量单位，例如wei；不是美元。|
|fee_type_source|string|费用的来源含义/计算口径。|
|fee_payer|string|执行费用付款地址，可能为bundler，不一定等于样本钱包。|
|context_slot|string|来源区块高度/上下文槽位文本，具体语义按source解释。|
|source_path|string|相对dataset根目录的压缩来源文件路径。|
|source_sha256|string|来源文件原始字节SHA256；先解压存储层再校验。|
|source_pointer|string|解码来源JSON后定位原记录的RFC 6901 JSON指针。|
|raw_json|string|完整来源记录的JSON文本。|

## ethereum_index/ethereum_user_operations.parquet

|字段|类型|含义|
|---|---|---|
|record_id|string|来源记录的稳定标识，不能替代交易哈希。|
|source|string|来源接口/记录类型。|
|chain|string|链标识；GMGN使用sol/bsc/base/eth/robinhood；通用EVM来源表使用十进制chain ID文本。|
|wallet|string|被查询/关联的钱包；回执表可为空，账户成员关系见memberships_json。|
|memberships_json|string|关联账户/链钱包角色列表的JSON文本；回执可同时属于多个账户。|
|tx_hash|string|EVM交易哈希或Solana签名；UserOp来源中为用户操作哈希，需在raw_json读取外层交易。|
|timestamp_utc|string|来源时间，ISO 8601 UTC文本；未知为null，读取时显式转换为UTC时间。|
|asset_address|string|链内资产合约或mint标识；原生资产可为空。|
|token_account|string|来源提供的代币账户，可缺失。|
|amount_raw|string|来源资产原始整数文本；避免浮点转换。|
|decimals_source|int32|来源资产精度，0是有效值，未知为null。|
|amount_basic|string|按来源精度换算的十进制数量文本；精度未知时为null。|
|amount_ui_source|string|来源显示数量文本，仅保留观察值。|
|fee_raw|string|来源原始费用整数文本；类型及单位由fee_type_source/fee_unit指定。|
|fee_unit|string|费用计量单位，例如wei；不是美元。|
|fee_type_source|string|费用的来源含义/计算口径。|
|fee_payer|string|执行费用付款地址，可能为bundler，不一定等于样本钱包。|
|context_slot|string|来源区块高度/上下文槽位文本，具体语义按source解释。|
|source_path|string|相对dataset根目录的压缩来源文件路径。|
|source_sha256|string|来源文件原始字节SHA256；先解压存储层再校验。|
|source_pointer|string|解码来源JSON后定位原记录的RFC 6901 JSON指针。|
|raw_json|string|完整来源记录的JSON文本。|

## account_features.parquet

|字段|类型|含义|
|---|---|---|
|account_id|large_string|数据集内稳定账户/链钱包角色键；FOMO的original_fomo:rank:handle为本地标识。|
|rank|int64|冻结样本中的原排名；GMGN排名在每链内部定义。|
|wallet|large_string|被查询/关联的钱包；回执表可为空，账户成员关系见memberships_json。|
|chain|large_string|链标识；GMGN使用sol/bsc/base/eth/robinhood；通用EVM来源表使用十进制chain ID文本。|
|event_count|int64|本账户×链的来源观察数；GMGN为活动行，FOMO为账户交易头行。|
|transaction_count|int64|本账户×链的不同交易哈希/签名数量。|
|buy_observation_count|int64|来源分类为buy的观察数量。|
|sell_observation_count|int64|来源分类为sell的观察数量。|
|active_days|int64|有可解析UTC时间的观察日期数。|
|first_seen_utc|timestamp[us, tz=Asia/Shanghai]|本来源观察的最早UTC时间，不代表账户创建时间。|
|last_seen_utc|timestamp[us, tz=Asia/Shanghai]|本来源观察的最晚UTC时间。|
|has_observed_activity|bool|本数据表是否包含该账户×链的观察；false不证明链上无活动。|

## 来源回放

`source_path`相对于dataset根目录。对该文件gzip解压一次得到来源原始字节，校验`source_sha256`。如果原始字节仍为gzip，再解压后解析JSON，使用`source_pointer`定位原记录。`evidence/index.json`列出存储层及来源层的字节数和SHA256。

## 汇总特征

`account_features.parquet`每账户×链一行，基于各主表的全部已观察范围计算。计数为0仅表示表内无观察；时间边界/覆盖限制仍以coverage为准。该表适合描述性分析和特征探索。预测任务应按决策截止时间重新截断、建立目标并划分训练/验证集，不能直接把全期汇总作为历史决策时特征。
