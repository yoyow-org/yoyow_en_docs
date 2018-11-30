# 用户发行资产
# User-Issued Assets (UIA)

YOYOW提供了发行资产的功能，普通账户可以创建和发行各种自定义的资产(UIA)。

YOYOW provides the feature of issuing assets, and common accounts can create and issue a variety of custom assets (UIA).

用户发行资产可以转账交易，因此，可被作为内容平台的奖励，替代传统的积分机制。

User-issued assets can be transferred in transactions, so they can be used as rewards for content platforms to replace the traditional point mechanism.

后期会开放YOYO与用户发行资产之间的兑换关系，更多有价值的玩法也会陆续推出。

Later, the exchange relationship between YOYO and UIA will be opened, and more valuable operations will be launched.

## 发行资产
## Issuing Assets
网页钱包中提供了简单的发行资产的操作，可以方便的指定发行Token的代码，总供应量，小数位数。如下图：
![发行资产](../images/asset/create_asset.png)

The web wallet provides a simple operation for issuing assets, and it is convenient to specify the code for issuing the token, the total supply, and the number of decimal points. As shown below:
![Issuing Assets](../images/asset/create_asset.png)

发行资产的手续费包括基础手续费和每千字节的费用，基础手续费按Token代码的长度收费，目前基础手续费如下:

The fees for issuing assets include the basic fees and the cost per kilobyte. The basic fees are charged according to the length of the Token code. The current basic fees are as follows:

| Token代码位数 | 基础手续费 | 
| :------: | ------: |
| 3位 | 30000 YOYO | 
| 4位 | 3000 YOYO | 
| 5位或更长 | 300 YOYO | 

| Token Code Digits | Basic Fees | 
| :------: | ------: |
| 3 Digits | 30000 YOYO | 
| 4 Digits | 3000 YOYO | 
| 5 Digits or more | 300 YOYO | 

## 高级选项
## Advanced Options
除了简单的Token供应量外，用户发行资产可以定义更多功能。

In addition to the simple feature of Token supply, users issuing assets can define more features.

- 白名单：资产可以选择启用白名单，如果定义了白名单，则只有白名单的账户才可以持有，使用资产。

- Whitelist: Assets can optionally be whitelisted. If a whitelist is defined, only whitelisted accounts can hold and use assets.

- 黑名单：资产默认启用黑名单，在黑名单内的账户不可以持有，使用该资产。

- Blacklist: The asset is blacklisted by default. The account in the blacklist cannot hold and use the assets.


- 强制转账：资产可以设定允许强制转账，如果启用，该资产发行人可以强制转走或者收回其他人账户里的该类资产

- Mandatory transfer: Assets can be set to allow for forced transfers. If enabled, the issuer of the asset can force transfer or take back such assets in other people's accounts.

- 限制转账：资产可以设定允许限制转账，如果启用，则转账的发起者或者接收者必须是该资产发行人

- Restricted transfers: Assets can be set to allow for restricted transfers. If enabled, the originator or recipient of the transfer must be the issuer of the assets.

- 发行资产：资产创建完成后需要发行到账户才能流通，如果启用，则该资产发行人可以增加一定数量的该类资产到某账户，发行意味着增加该资产的当前流通总量

- Issuance of assets: After the asset is created, it needs to be issued to the account for circulation. If enabled, the issuer of the asset can add a certain amount of such assets to an account. The issue means increasing the current circulation of the asset.

- 修改流通上限：如果启用，则该资产发行人可以修改该资产的流通量上限

- Modifying the circulation limit: If enabled, the asset issuer can modify the circulation limit of the assets.

更多相关信息，请参考API文档中[创建资产](../api/wallet_api.html#create-asset)中请求参数的详细描述。

For more information, please refer to the API documentation for a detailed description of the request parameters in the [Creation of Assets](../api/wallet_api.html#create-asset).
