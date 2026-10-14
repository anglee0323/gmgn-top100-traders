# 字段与单位

- dataset/account_id/rank/chain/wallet保留固定链钱包角色。Sol地址与合约大小写敏感，EVM按十六进制身份比较。角色不等于独立个人。
- event_id由来源活动身份生成，保留同交易不同资产、池或执行观察；tx_hash不能直接去重所有事件。
- event_type是来源buy/sell等原分类，不代表已验证净收益或所有权完整账本。
- asset_address/quote_asset_address按链和实际CA识别资产，不用symbol套价。
- token_amount_provider/quote_amount_provider/gas_native_provider/gas_usd_provider/cost_usd_provider保留来源数值文本；不把native费充USD，不把来源正费用当完整钱包成本。
- raw_json是完整来源活动原行。交易首条可见记录、周转额、当前财富不当作历史本金。
- source_path相对于本目录；先解压其gzip存储层得到原始文件字节并核对source_sha256。如果原文件本身是gzip，再解压，按source_pointer读取JSON。evidence/index.json同时记录存储层与原字节SHA。
- coverage中pages/continuation/source_eof/earliest_utc/latest_utc为本来源范围；成功终页原始回执在terminal_receipt_path。缺失不作0，空来源不证明链上无交易。
- ETH独立索引保留record_id/source/memberships_json/log或调用字段、raw数量/decimals/fee_currency等；token Transfer不是独立买卖，internal调用value不自动当已结算转账，UserOp不等于外层交易。
- buy/sell作为events筛选视图使用，不重复存同一批行；不同来源表不能相加计算独立交易笔数。
