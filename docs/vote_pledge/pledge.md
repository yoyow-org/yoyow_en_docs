# Collateral

Currently, the collateral can be used for updating the platform accounts or applying for becoming miner witnesses. Only YOYO is accepted for the collateral.

## Platform Collateral

Currently, the minimum collateral value for the platform is 10000 YOYO. The qualification of the platform is evaluated according to the current collateral value, which can be modified by the committee. Please refer to the parameter `platform_min_pledge` in [parameter table](https://yoyow.bts.ai/fees/index#tab-parameters).

## Committee Collateral

The minimum collateral value for the committee is 1000 YOYO.


## Miner Witness Collateral

- The miner witness calculates the number of blocks to be produced according to the amount of valid collateral of each account. For example, if A, B and C all deposit collateral, and A has six tokens of valid collateral, B has three tokens of valid collateral, and C has one token of valid collateral, then, for the following 100 blocks, it should be arranged that A produces 60 blocks, B produces 30 blocks and C produces 10 blocks.

- The valid collateral will be the average collateral over seven days, or the current collateral (whichever is smaller). That is to say, when the collateral is increased, the valid collateral will increase gradually, and when the collateral decreases or is canceled, the valid collateral will immediately decrease.

## Collateral Refunding

The refunding of collateral deposits will be delayed for a certain period of time when the collateral amount is decreased or the collateral is cancelled. Current value: 7 days.

Notes:
1. When there are continuous decreases in collateral, the amount will be accumulated and refunded in one lump sum. The time of refund will be delayed on the basis of the time of the last decrease.

2. For collateral increases, if there is a collateral deposit to be returned to the current account, that refund will be deducted first, and then the remaining amount will be deducted from the balance of the account.
