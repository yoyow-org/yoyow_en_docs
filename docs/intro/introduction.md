# YOYOW 简介
# Introduction to YOYOW

YOYOW 的名称来自英文 You Own Your Own Words，其目标是建立一个利用区块链技术，使用去中心化的共识方式为内容生产领域进行贡献定价和权益回报的网络，构建一套合理的内容收益分配机制使内容生产者、内容投资者、内容筛选者和生态建设者都能得到合理的激励与回报，同时构建一个基于用户内容评价的价值网络。 

YOYOW is named from "You Own Your Own Words". Its goal is to build a network that applies the blockchain technology and uses decentralized consensus method to price contribution and provide equity returns for the content area, and to build a reasonable content value distribution mechanism, which enables content producers, content investors, content filterers and eco-builders to get reasonable incentives and rewards while building a value network based on users' content ratings.

与其他区块链系统不同，借助于DPoS的共识机制为了适应内容生产领域的生态，YOYOW设计了更多的用户角色，独特的密钥体系和丰富的资产类型。

Unlike other blockchain systems, YOYOW has designed more user roles, unique key systems and rich asset types by using DPoS consensus mechanism in order to adapt to the ecology of content production.

## YOYOW 角色设计
## YOYOW Role Conception
在系统生态中，为了兼顾效率和公平，YOYOW设计了完善的用户体系，主要包括普通用户，平台，理事会，见证人。

In the system ecology, in order to balance efficiency and fairness, YOYOW has designed a complete user system, including common users, platforms, committees, and witnesses.



### 普通用户
### Common Users
普通用户是YOYOW网络的主要参与者，在YOYOW网络中，普通用户在各个平台共享统一的账号体系，拥有转账、选举等权限。

Common users are the main participants of the YOYOW network. In the YOYOW network, common users share a unified account system on each platform, and have the rights of transfer, election, etc.


### 平台
### Platforms
普通用户抵押一定的YOYO代币可称为平台。普通用户通过 YOYOW 钱包对平台进行授权，授权平台使用其零钱权限，用于跨站点登陆、对内容评分、发布内容、评论等操作，普通用户也可以随时撤销授权。

Common user that deposits a collateral of a certain amount of YOYO tokens can be called a platform. Common users can authorize the platform through the YOYOW wallet, and the authorized platform uses its liquid asset authority for cross-site login, content rating, content posting, comments, etc., and common users can also revoke authorization at any time.


### 理事会
### Committee
理事会是YOYOW网络中的管理机构，由YOYOW用户投票产生，负责发起议案和对议案进行投票表决。议案的内容主要为调整系统的可调参数，比如各种交易的费率，最高评分权重等，也包括区块生产间隔时间、区块奖励等。 

The committee is the governing body of the YOYOW network，voted in by the YOYOW users and is responsible for initiating and voting on the proposals. The content of the proposals is mainly to adjust the adjustable parameters of the system, such as the rate of various transactions, the highest scoring weight, etc., including as well the block production interval, block rewards, etc.

### 见证人
### Witnesses
见证人是区块的生产者，负责收集广播的各种交易并打包到区块中，因此见证人生产区块也可以获得相应的 YOYO 代币回报。

The witness is the producer of the block, responsible for collecting the various transactions of the broadcast and packing them into the blocks, so the witnesses producing blocks can also get the corresponding YOYO token as returns.

## 密钥体系
## Private Key System
在区块链项目中，私钥是账号的重中之重。然而，当一个用户做任何操作都使用账号私钥签名的话，无疑极大的增加了账号的风险。考虑到不同权限不同的安全级别，YOYOW体系中分别设定了三级密钥体系：**主控密钥**，**资金密钥**，**零钱密钥**。
- 主控密钥为最高权限密钥，不到万不得已的时候不应该使用主控密钥；
- 资金密钥可以操作余额；
- 零钱密钥可以操作零钱，也可以用来发帖，点赞，登陆鉴权等。  
- 除了跟资产相关的三级密钥，还有一个**备注密钥**用来加密和查看交易的备注信息。    

In the blockchain project, the private key is the top priority of the account. However, when a user does any operation and uses the account private key to sign, it will undoubtedly greatly increase the risk of the account. Considering the different security levels of different authorities, the three-level key system is set in the YOYOW system: **the Owner Key**, **the Active Key**, and **the Secondary Key**.

- The Owner Key is the key of highest authority, and the Owner Key should not be used unless it is absolutely necessary;
- The Active Key can operate the balance;
- The Secondary Key can operate the liquid assets, and can also be used for posting, giving likes, login authentication, etc.
- In addition to the asset-related 3-level keys, there is also a **Memo Key** used to encrypt and view the note information for the transactions.

## 资产类型
## Types of Assets
YOYOW网络内有三类资产：**YOYO代币**，**用户发行资产**（UIA），**积分**。

There are three types of assets in the YOYOW network: **YOYO token**, **user-issued assets** (UIA), and **points**.

### YOYO代币
### YOYO Token
在YOYOW的账户里，基础资产为YOYO 代币，有两处可进行存放：『**余额**』和『**零钱**』。

In YOYO's account, the base asset is YOYO token, and there are two places to store: 『**balance**』and『**liquid assets**』

- 『**余额**』拥有较高的安全性，建议大额的YOYO在余额中存储。
- 『**零钱**』进行小额的存储与转账，当用户授予零钱权限给平台时，平台可以拥有操作”零钱” 的权限，可理解为免密支付。

- 『**Balance**』 has a high level of security, and it is recommended that a large amount of YOYO be stored in the balance.
- 『**Liquid assets**』is for small amount of savings and transfer. When the user grants the authority of liquid assets to the platform, the platform can have the right to operate "liquid assets", which can be understood as password-free payment.

### 用户发行资产（UIA）
### User-Issued Assets（UIA）
用户可以发行自定义的资产：根据自身业务，创建在自己平台内流通的Token。后续会提供用户发行资产（UIA）与YOYO之间的便捷兑换方式，共享便捷的变现渠道。  

Users can issue custom assets: users can create Tokens that circulate within their own platforms based on their business. A convenient exchange method between the user-issued assets (UIA) and YOYO will be provided subsequently, sharing convenient monetization channels.

### 积分
### Bonus Points
积分的作用是抵扣手续费。当账户"余额"拥有一定数量的YOYO时，系统会自动积累积分，在支付手续费时，您可以选择通过积分进行抵扣。
更多信息可以参考：[积分](../others/csaf.html)

The function of the bonus points is to pay fees. When the account "balance" has a certain amount of YOYO, the system will automatically accumulate points. When paying fees, you can choose to deduct the points. For more information, please refer to: [Bonus Points](../others/csaf.html)


## 白皮书
## White Paper

白皮书内容请移步[白皮书下载](https://yoyow.org/files/white-paper3.pdf)

For the content of white paper, please refer to [White Paper Download](https://yoyow.org/files/white-paper3.pdf)
