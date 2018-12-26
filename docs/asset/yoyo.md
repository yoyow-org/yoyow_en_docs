# YOYO Token

YOYOW的核心资产是YOYO，YOYO可以存放在账户的余额或者零钱中。

The core asset of YOYOW is YOYO.

## 余额
## Balance
大额的YOYO建议存储在余额中。只有余额中的YOYO可以累积积分，积分可以用来抵扣手续费。

We recommend storing most of your YOYO in the balance. The YOYO in the balance can accumulate Bonus Points, which can be used to pay fees.

## 零钱
## Tipping

零钱建议存放小额的YOYO，日常的小额支付建议使用零钱而不是余额。

We recommend storing small amount of YOYO in the Tipping. For daily small payments, it is recommended to use Tipping instead of balance.

## 余额和零钱的设计
## The Design of Balance and Tipping

余额和零钱的设计主要是为了平衡易用性和安全性。动用余额和零钱是分别需要两个不同的权限，主控密钥控制余额，零钱密钥（次级密钥）控制零钱。 

Balance and Tipping are designed primarily to balance usability and security. The use of balance and tipping requires two different permissions. The active key controls the balance, and the liquid asset key (secondary key) controls the tipping.

零钱权限可以授权给平台，以方便在平台构建的生态中自动交易，不用每次都动用自己密钥，以便减少风险。

The authority of tipping can be delegated to the platform to facilitate automated trading in the ecosystem created by the platform, without having to use your own private key for each time so as to reduce risks.

因此，余额的设计是为了存放大额的资产，零钱不建议存放过多的资产。

Therefore, the balance is designed to store large amounts of assets, and it is not recommended to deposit too many assets in tipping.
