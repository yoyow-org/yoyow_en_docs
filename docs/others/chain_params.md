# YOYOW 参数表

|名称|含义|数值|备注|
|:--|:--|:--|:--|
|block_interval|出块间隔|3 s||
|maximum_transaction_size|最大交易尺寸 |65536 字节|64K|
|maximum_block_size|最大块尺寸|2000000 字节|1.907 M|
|maximum_time_until_expiration|交易最长有效期|86400 s|24小时|
|maximum_authority_membership|账号中最多授权项数量|10|即最多支持 N/10 模式的多重签名（与 BTS 相同）|
|max_authority_depth|最大账号授权级别|2|初始值 2 层。即直接签名验证时，检查 2 层账户的公钥|
|csaf_rate|币龄抵扣手续费比率|864000000|86400*10000 ，即每 10000 YOYO 每天可得 1 YOYO 用于抵扣手续费|
|max_csaf_per_account|每账户最大积累币龄手续费金额|100000000|100 YOYO|
|csaf_accumulate_window|手续费币龄最长累积时间|604800|1 周|
|min_witness_pledge|见证人最低抵押金额|1000000000|10000 YOYO|
|max_witness_pledge_seconds|见证人计算平均押金时长|604800|矿工按前 1 周的平均押金数量排序安排出块。|
|witness_avg_pledge_update_interval|见证人平均押金更新间隔|1200|每 1 小时自动更新一次平均押金；另外，如果押金数量发生变化，平均押金会实时更新。|
|witness_pledge_release_delay|见证人押金退还延迟块数|201600|即减少押金时 7 天后退回到账|
|min_governance_voting_balance|投票资格最低余额|1000000000|10000 YOYO ，账户余额超过这个值得才可投票选见证人和理事会，低于这个余额自动失去投票权|
|max_governance_voting_proxy_level|最大投票代理层数|4|4 层|
|governance_voting_expiration_blocks|投票有效期|2592000|90 天，即投票后 90 天内不刷新投票意向的话，原投票失效。|
|governance_votes_update_interval|有效票数（按币龄计算）更新间隔|28800|1 天，即每天自动更新一次平均余额（用途详见下面参数），另外，如果账户余额发生变化，平均余额会实时更新。|
|max_governance_votes_seconds|有效票（币龄）最长积累时间|5184000|60 天，即有效票数取账户前 60 天的平均余额。平均余额从第一次投票时间开始算；如果投票过期，有效票数清零，下次投票重新开始算。|
|max_witnesses_voted_per_account|每账号最多投见证人人数|101||
|max_witness_inactive_blocks|见证人丢块导致强制离线阈值（块数）|28800|相当于 24 小时，即如果一个见证人 24 小时内没有出过块，又丢一块时，见证人强制离线，不再安排出块。见证人随时可以用命令重新激活，即可再安排出块。|
|by_vote_top_witness_pay_per_block|主力见证人每块工资|30000||
|by_vote_rest_witness_pay_per_block|备选见证人每块工资|30000||
|by_pledge_witness_pay_per_block|矿工（按抵押排序安排出块的）见证人每块工资|50000||
|by_vote_top_witness_count|每轮主力见证人数量|11||
|by_vote_rest_witness_count|每轮备选见证人数量|3||
|by_pledge_witness_count|每轮矿工（按抵押安排出块的）见证人数量|7|以上 3 个参数加起来，等于每一轮总的见证人人数。|
|budget_adjust_interval|预算更新间隔（块数）|10512000|28800 * 365 ，即每年调整一次。|
|budget_adjust_target|预算目标（释放百分比）|1000|即 10% ，含义是下一个周期会释放剩余供应量的 10%|
|committee_size|理事会人数|5||
|committee_update_interval|每届理事会任期时长|864000|28800 * 30 ，即 30 天|
|min_committee_member_pledge|理事最低抵押金额。|100000000|1000 YOYO|
|committee_member_pledge_release_delay|理事押金释放延迟|28800| 1 天|
|max_committee_members_voted_per_account|每账户可投理事数量|1||
|witness_report_prosecution_period|见证人举报追诉期（块数）|28800|1 天|
|witness_report_allow_pre_last_block|见证人举报是否允许早于最新块| false|即只允许举报见证人的最新块|
|witness_report_pledge_deduction_amount|见证人举报后押金扣除金额|100000000|1000 YOYO|
|platform_min_pledge|平台最低抵押金额|1000000000|10000 YOYO|
|platform_max_pledge_seconds|平台计算平均押金时长|604800|1 周|
|platform_avg_pledge_update_interval|平台平均押金更新间隔|1200|1 小时，即每小时自动更新一次平均押金；另外，如果押金数量发生变化，平均押金会实时更新。|
|platform_pledge_release_delay|平台押金释放延迟|28800|1 天，即减少押金时 1 天后退回到账|
|platform_max_vote_per_account|每账户可投票支持的平台数量|10||
