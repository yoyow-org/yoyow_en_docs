# 抵押
# Collateral

目前，抵押可以用于升级平台账号，或申请矿工见证人。抵押只能抵押YOYO。

Currently, the collateral can be used for updating the platform accounts or applying for becoming miner witnesses. Only YOYO is accepted for the collateral.

## 平台抵押
## Platform Collateral

当前平台的最低抵押金额为10000 YOYO，根据当前抵押值判断平台的资格。该值可以由理事会修改，可以参考[参数表说明](https://yoyow.bts.ai/fees/index#tab-parameters)中参数`platform_min_pledge`

Currently, the minimum collateral value for the platform is 10000 YOYO. The qualification of the platform is evaluated according to the current collateral value, which can be modified by the committee. Please refer to the parameter `platform_min_pledge` in [parameter table](https://yoyow.bts.ai/fees/index#tab-parameters).

## 理事会抵押
## Committee Collateral

理事会的最低抵押金额为1000 YOYO

The minimum collateral value for the committee is 1000 YOYO.


## 矿工见证人抵押
## Miner Witness Collateral

- 矿工见证人根据各账号有效抵押数量计算应出块数量。如：只有ABC三人抵押，其中A有效抵押6个币，B有效抵押3个币，C有效抵押1个币，那么在后续100个块中，应该安排A出块60个，B出块30个，C出块10个。
- The miner witness calculates the number of blocks to be produced according to the amount of valid collateral of each account. For example, if A, B and C all deposit collateral, and A has six tokens of valid collateral, B has three tokens of valid collateral, and C has one token of valid collateral, then, for the following 100 blocks, it should be arranged that A produces 60 blocks, B produces 30 blocks and C produces 10 blocks.

- 有效抵押数为'7天内平均抵押'与'当前抵押'两者较小值。即：增加抵押时，有效抵押数缓慢增加；降低或解除抵押时，有效抵押立即减少。
- The valid collateral will be the average collateral over seven days, or the current collateral (whichever is smaller). That is to say, when the collateral is increased, the valid collateral will increase gradually, and when the collateral decreases or is canceled, the valid collateral will immediately decrease.

## 抵押金额的返还
## Collateral Refunding

降低抵押金额或者解除抵押时，押金延迟一段时间后退还，当前设置为7天。

The refunding of collateral deposits will be delayed for a certain period of time when the collateral amount is decreased or the collateral is cancelled. Current value: 7 day.

注：
1. 连续多次降低抵押时，金额累加一起退还，计划退还时间以最后一次降低时间为准进行延迟
2. 增加抵押时，如果当前账号有延迟退还的押金，优先从里面扣，不足部分从余额扣取

Notes:
1. When there are continuous decreases in collateral, the amount will be accumulated and refunded in one lump sum. The time of refund will be delayed on the basis of the time of the last decrease.

2. For collateral increases, if there is a collateral deposit to be returned to the current account, that refund will be deducted first, and then the remaining amount will be deducted from the balance of the account.
