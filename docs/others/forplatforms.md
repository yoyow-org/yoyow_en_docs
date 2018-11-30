# 平台接入概述
# Introduction to Platform Integration

YOYOW 是基于区块链技术的媒体公链, 其目标是建立一个利用区块链技术，使用去中心化的共识方案为内容生产领域实现贡献定价和权益回报的网络，使内容生产者、内容投资者、内容筛选者和生态建设者都能得到合理的激励与回报。
YOYOW专注于底层技术与大的利益分配设定，把更大的自主权分配给接入平台与开发者，平台与开发者无需太多的区块链专业知识，就可以依托YOYOW，建立自己的区块链内容产品。

YOYOW is a media public chain based on blockchain technology. Its goal is to build a network that utilizes blockchain technology and uses decentralized consensus method to price contributions and provide equity returns for content production areas, enabling content producers, content investors, content filterers and eco-builders to get reasonable incentives and rewards. YOYOW focuses on the underlying technology and large profit distribution settings, and assigns greater autonomy to integrated platforms and developers. Platforms and developers can build their own blockchain-based content products by relying on YOYOW without much blockchain expertise. 

媒体平台的接入可以获得以下优势： 

Integration of the media platforms can bring the following advantages:

- 实现当前业务+区块链的融合，引入合理的激励机制，有效提升用户积极性
- Realizing the integration of current business + blockchain, introducing a reasonable incentive mechanism, and effectively enhancing user enthusiasm
- 可轻松实现资产Token的发行，获得便利的Token变现渠道
- Easily realizing the issuance of token assets and getting convenient token monetization channels
- 可直接获得YOYOW激励
- Can directly get YOYOW incentives
- 可共享YOYOW当前所有用户
- Can share all current users of YOYOW

## YOYOW 生态简介
## Introduction to YOYOW Ecology

### 用户角色
### User Roles

在系统生态中，为了兼顾效率和公平，YOYOW设计了完善的用户体系，主要包括普通用户，平台，理事会，见证人。  

In the system ecology, in order to balance efficiency and fairness, YOYOW has designed a complete user system, including common users, platforms, committees, and witnesses.


#### 普通用户
#### Common Users
普通用户是YOYOW网络的主要参与者，在YOYOW网络中，普通用户在各个平台共享统一的账号体系，拥有转账、选举等权限。

Common users are the main participants of the YOYOW network. In the YOYOW network, common users share a unified account system on each platform, and have the rights of transfers, election, etc.

#### 平台
#### Platforms
平台是一类特殊的用户，普通用户抵押一定的YOYO代币可称为平台。  
普通用户通过 YOYOW 钱包对平台进行授权，授权平台使用其零钱权限，用于跨站点登陆、对内容评分、发布内容、评论等操作，普通用户也可以随时撤销授权。  
接入后，平台可获得YOYOW网络的Token激励。

The platform is a special kind of user. Common user that deposits a collateral of a certain amount of YOYO tokens can be called a platform. Common users can authorize the platform through the YOYOW wallet, and the authorized platform uses its liquid asset authority for cross-site login, content rating, content posting, comments, etc., and common users can also revoke authorization at any time. After integration, the platforms can get token incentives of the YOYOW network.

#### 理事会
#### Committees
理事会是YOYOW网络中的管理机构，负责发起议案和对议案进行投票表决。议案的内容主要为调整系统数十项可调参数，比如各种交易的费率，最高评分权重等，也包括区块生产间隔时间、区块奖励等。  
理事会的人选由YOYOW用户投票产生，用户可随时撤票，来保证理事会决议真正体现多数持币人的意愿。

The committee is the governing body of the YOYOW network, responsible for initiating and voting on the proposals. The content of the proposals is mainly to adjust dozens of adjustable parameters of the system, such as the rate of various transactions, the highest scoring weight, etc., including as well the block production interval, block rewards, etc.
The committee candidates are voted in by the users of YOYOW and the users can withdraw their votes at any time to ensure that the resolutions of the committee truly reflect the wishes of the majority of the token holders.

#### 见证人
#### Witnesses
见证人是区块的生产者，负责收集广播的各种交易并打包到区块中，因此见证人生产区块也可以获得相应的 YOYO 代币回报。

The witness is the producer of the block, responsible for collecting the various transactions of the broadcast and packing them into the blocks, so the witnesses producing blocks can also get the corresponding YOYO token as returns.

见证人包括主力见证人、后备见证人和矿工见证人。

The witnesses include Top witnesses, Standby witnesses, and Miner witnesses.

在当前机制下，YOYOW用户可充分行使自己的权利。理事会与见证人均为投票产生，一切以票数说话。用户可通过投票选出愿意参与社区建设、愿意为社区做贡献的账号。同时，每次投票有效期90天，保证了理事会、见证人团队的新鲜活力。

Under the current mechanism, YOYOW users can fully exercise their rights. Both the committee and the witnesses are elected by votes, and all is decided according to the number of votes. Users can vote for an account that is willing to participate in community building and is willing to contribute to the community. At the same time, each vote is valid for 90 days, which guarantees the vitality of the committee and the witness team.

### 密钥和资产
### Private Key and Assets

#### 密钥体系
#### Private Key System
在区块链项目中，私钥是账号的重中之重。然而，当一个用户做任何操作都使用账号私钥签名的话，无疑极大的增加了账号的风险。对于YOYOW而言，考虑到不同权限不同的安全级别，设定了三级密钥体系：**主控密钥**，**资金密钥**，**零钱密钥**。主控密钥为最高权限密钥，不到不得已的时候不应该使用主控密钥；资金密钥可以操作余额；零钱密钥可以操作零钱，也可以用来发帖，点赞，登陆鉴权等。

In the blockchain project, the private key is the top priority of the account. However, when a user does any operation and uses the account private key to sign, it will undoubtedly greatly increase the risk of the account. Considering the different security levels of different authorities, the three-level key system is set in the YOYOW system: **the Owner Key**, **the Active Key**, and **the Secondary Key**. The Owner Key is the key of highest authority, and the Owner Key should not be used unless it is absolutely necessary. The Active Key can operate the balance. The Secondary Key can operate the liquid assets, and can also be used for posting, giving likes, login authentication, etc.

通俗来讲：  

 1. 主控密钥是核心，若其他低级别密钥丢失，可动用主控密钥重置更新。所以，务必妥善保存，丢失无法找回。    
 2. 转账等资金操作，可以动用资金密钥。   
 3. 用户授权给平台，只需要授予次级密钥（零钱权限），平台通过这个权限可以代理发帖、点赞及小额转账。  

 Simply speaking:
1. The Owner key is the core. If other low-level keys are lost, you can use the Owner key to reset and update. Therefore, it must be kept properly and once lost, it cannot be retrieved.
2. For fund operations such as transfer, you can use the Active key.
3. The users who want to authorize the platforms only need to grant the Secondary key (liquid asset authority), through which the platforms can act as proxy for the users to post, give likes and make small-amount transfers.

除了跟资产相关的三级密钥，还有一个**备注密钥**用来加密和查看交易的备注信息。

In addition to the asset-related 3-level keys, there is also a **Memo Key** used to encrypt and view the note information for the transactions.

以上各组密钥，由一对公私钥对组成，公钥为以YYW开头的53个字符，私钥为51个字符。可以通过[网页钱包](https://wallet.yoyow.org/)查看各个密钥（详见：[yoyow keys tutorial](https://steemit.com/cn/@peterchen145/yoyow-keys-tutorial-yoyow) ）。比如：

Each of the above groups of keys consists of a pair of public and private key pairs. The public key is 53 characters starting with YYW and the private key is 51 characters. Each key can be viewed through [web wallet](https://wallet.yoyow.org/) (see: [yoyow keys tutorial](https://steemit.com/cn/@peterchen145/yoyow-keys-tutorial-yoyow) ). For example:

![网页钱包密钥截图](/images/others/account_keys.png)

![web wallet private key screenshot](/images/others/account_keys.png)

#### 资产类型
#### Types of Assets
YOYOW网络内有三类资产：**YOYO代币**，**用户发行资产**（UIA），**积分**。

There are three types of assets in the YOYOW network: **YOYO token**, **user-issued assets** (UIA), and **points**.

##### YOYO代币
##### YOYO Token
在YOYOW的账户里，基础资产为YOYO 代币，有两处可进行存放：『**余额**』和『**零钱**』。

In YOYO's account, the base asset is YOYO token, and there are two places to store: 『**balance**』and『**liquid assets**』.

- 『**余额**』拥有较高的安全性，建议大额的YOYO在余额中存储。
- 『**Balance**』 has a high level of security, and it is recommended that a large amount of YOYO be stored in the balance.

- 『**零钱**』可理解为免密支付，进行小额的存储与转账，当用户授予零钱权限给平台时，平台甚至可拥有操作”零钱” 的权限。
- 『**Liquid assets**』, which can be understood as password-free payment, is for small amount of savings and transfer. When the user grants the authority of liquid assets to the platform, the platform can even have the right to operate "liquid assets".

##### 用户发行资产（UIA）
##### User-Issued Assets（UIA）

用户可以发行自定义的资产：根据自身业务，创建在自己平台内流通的Token。后续会提供用户发行资产（UIA）与YOYO之间的便捷兑换方式，共享便捷的变现渠道。  

Users can issue custom assets: users can create Tokens that circulate within their own platforms based on their business. A convenient exchange method between the user-issued assets (UIA) and YOYO will be provided subsequently, sharing convenient monetization channels.

##### 积分
##### Bonus Points
积分是对YOYO持有者的奖励，当您的账号持有YOYO时，积分可随时间累积。积分积累有上限，您需要手动领取积分。

Bonus Points are rewards for YOYO holders. When your account holds YOYO, Bonus Points will be accumulated over time. There is an upper limit for Bonus Points accumulated, and you have to withdraw Bonus Points manually.

###### 积分用途
###### Using Bonus Points

积分目前唯一用途是抵扣手续费。在支付手续费时，您可以选择通过积分进行抵扣。100积分等价于1 YOYO。

At present, the only use of Bonus Points is for paying fees. You can use Bonus Points to offset fees. 100 Bonus Points can be used as 1 YOYO.

当您账户"余额"部分拥有一定数量的YOYO时，系统会自动为您积累积分。有最高积累上限。

When the “balance” of your account reaches a certain amount of YOYO, the system will automatically accumulate Bonus Points for you. There is a limit to the highest amount of accumulation.

###### 积分使用方法
###### Using Method for Bonus Point 
积分需要手动领取，可手动将待领取积分领取至您的账户，也可以领取至其他联系人账户，作为奖励赠予。积分也可以租借给其他人使用。

Bonus Points are withdrawn manually. You can manually withdraw available Bonus Points to your account or other accounts of your contact as incentives. Bonus Points can be rented to others for use.

YOYOW引入积分，可以减少普通用户小额手续费（内容发布，点赞等）的支出，增加用户使用的积极性。在其他区块链项目中，对于链上操作，都需要支付不同程度的手续费。 对于YOYOW来讲，此积分体系使用户免费操作成为了可能。 

YOYOW introduces Bonus Points, which can reduce the expenditure of common users' small amount of fees (content release, likes, etc.) and increase the enthusiasm of users. In other blockchain projects, different levels of fees are required for chain operations. For YOYOW, this bonus point system makes it possible for users to operate for free.

## 接入流程
## Integration Process

![接入流程图](/images/others/join_up_process.png)

![Integration Process Diagram](/images/others/join_up_process.png)

<center>接入流程</center>
<center>Integration Process</center>

### 注册账号
### Account Registration

申请YOYOW账号需要网页提交注册。网页访问<https://wallet.yoyow.org/#/create-account>，设置密码，YOYOW会自动返回账户名和账号私钥，账号名为数字UID（yoyo号），私钥为主控密钥。该私钥系统不会保存，且仅在注册成功时出现一次，拥有管理账户的最高权限，请妥善保存，不建议保存到电脑，网盘或云笔记里，可以写到多张纸条上，保存到几个较为稳妥的位置，保证不会丢失，不会泄露。  

To apply for a YOYOW account, you need to submit a registration page. Access the webpage <https://wallet.yoyow.org/#/create-account>, set the password, and YOYOW will automatically return the account name and account private key. The account name is the number UID (yoyo number), the private key is the Owner key. The private key will not be saved by the system, and will only appear once upon successful registration. It has the highest authority to manage the account. Please save it properly. It is not recommended to save it to a computer, net-disk or cloud note. You can write it on multiple notes and save it to a few more secure locations, ensuring that they will not be lost or leaked.

![注册](/images/others/signup.png)

![Registration](/images/others/signup.png)

![注册成功](/images/others/signup_success.png)

![Successful Registration](/images/others/signup_success.png)

完成注册后，可以在『设置』 -- 『账号』里查看到自己的主控密钥，资金密钥，零钱密钥以及备注密钥。  

After completing the registration, you can view your Owner key, Active key, Secondary key and Memo key in 『Settings』-- 『Accounts』.

### 升级为平台账号
### Updating to Platform Accounts

普通账号需要升级成平台账号才能获得用户的授权，升级平台商目前需要抵押10000 YOYO的押金和1000 YOYO的手续费。该操作暂时通过yoyow的client端，通过接口执行create_platform操作，同时设定平台的名称，Token的符号以及相关的url地址和其他一些扩展信息，比如平台提供的api访问接口等。   

A common account needs to be upgraded to a platform account to obtain the user's authorization. The upgraded platform currently needs to deposit a collateral of 10,000 YOYO and a fee of 1000 YOYO. This operation is temporarily performed through the YOYOW client. You need to perform the create_platform operation through the interface, and set the name of the platform, the symbol of the Token and the related url address and other extended information, such as the API access interface provided by the platform.

具体抵押升级平台账号的操作，详见YOYOW中间件[yoyow-middleware](https://github.com/yoyow-org/yoyow-node-sdk/tree/master/middleware#2-%E5%88%9B%E5%BB%BA%E5%B9%B3%E5%8F%B0) 的使用教程。

For details on how to deposit the upgraded platform account, see the tutorials on using YOYOW middleware [yoyow-middleware](https://github.com/yoyow-org/yoyow-node-sdk/tree/master/middleware#2-%E5%88%9B%E5%BB%BA%E5%B9%B3%E5%8F%B0).

### 授权认证
### Authorization Certification

YOYOW提供类似于OAuth的授权认证。YOYOW提供了中间件来方便平台进行接入，中间件中提供包括签名平台 （sign），签名验证（ verify），以及签名平台返回二维码（signQR）等接口（详见：[《yoyow-middleware》#Auth 相关](https://github.com/yoyow-org/yoyow-node-sdk/tree/master/middleware#2-auth-%E7%9B%B8%E5%85%B3)）。平台可以通过SDK中sign或signQR接口生成绑定链接，verify接口验证用户授权。通过授权，平台可以获得用户的零钱管理权限以及登陆授权。  

YOYOW provides an authorization certification similar to OAuth. YOYOW provides middleware to facilitate platform integration. The middleware provides interfaces such as signature platform (sign), signaturea authentication (verify), and return QR code on signature platform (SignQR) (see: [《yoyow-middleware》#Auth related](https://github.com/yoyow-org/yoyow-node-sdk/tree/master/middleware#2-auth-%E7%9B%B8%E5%85%B3)). The platform can generate a binding link through the sign or signQR interface in the SDK, and can verify user authorization through the verify interface. Through authorization, the platform can obtain the user's liquid asset management authority and login authorization.

授权流程：

Authorization Process：


 ![OAuth 流程](/images/others/oauth_process.png)
 
 ![OAuth Process](/images/others/oauth_process.png)

注：

Note:  

1. 考虑到安全问题，YOYOW中间件 SDK 强烈建议单独部署于内网机器中，不对外开放IP与端口。

Considering security issues, the YOYOW middleware SDK is strongly recommended to be deployed separately on intranet machines without opening IP and ports.

2. YOYOW中间件 SDK 需要零钱权限，使用时，记得修改配置文件中默认的零钱密钥和备注密钥。 

YOYOW middleware SDK requires liquid asset authority. When using, remember to modify the default Secondary key and Memo key in the configuration file.


### 资产发行
### Issuing Assets

媒体可以根据需要发行自己的资产，资产的数量和汇率等可以由自己的需求设定。yoyow平台为设定的资产提供流通和交易的便利。 

The media can issue its own assets as needed, and the number of assets and exchange rates can be set by their own needs. The yoyow platform provides the convenience of circulation and trading for set assets.

发行的资产暂时通过钱包客户端的create_asset函数操作，需要指定资产的总量，交易的手续费汇率，以及白名单和黑名单等设置。

The issued assets are temporarily operated by the create_asset function of the wallet client, and the total amount of assets, the transaction rate of the transaction, and the whitelist and blacklist settings need to be specified.

详见：[《YOYOW钱包API》#create_assets](../Wallet%20API#2417-create_asset)。

See：[《YOYOW Wallet API》#create_assets](../Wallet%20API#2417-create_asset)。

### 用户激励
### User Incentives

平台可根据自身业务需要，指定针对的激励计划。激励的标的，可以为YOYO 或 自己发行的token。

The platform can specify the incentive plan for its own business needs. The target of the incentive can be YOYO or the toekns issued by itself.

以币问为例，币问已接入YOYOW的网络中，在币问的平台中，系统奖励周期为一周，定期统计一周内用户发表的内容所获得的点赞、点赞能量和评论数等，计算出一个奖励值，根据用户的奖励值的比例，发放YOYO代币。根据用户的活跃度和内容贡献值，用户会有威望的属性，被威望较高的用户点赞，可获得更高的点赞能量，被点赞用户也会获得更高的奖励。 用户可以在网页钱包中将YOYO提现到其他地方进行交易。

Taking Biask as an example, Biask has been integrated to the YOYOW network. In the platform of Biask, the system reward period is one week, and the likes, the like energy and the number of comments obtained by the user's posted content in a week are regularly counted and a bonus value is calculated, and YOYO token will be issued according to the proportion of the user's bonus value. According to the user's activity and content contribution value, the user will have prestige attributes. If praised by users with higher prestige, the users get higher like energy, and the liked users will also get higher rewards. Users can withdraw YOYO from the web wallet to other places for trading.

在YOYOW的SDK中提供了transfer操作，可以转出自己的资产（YOYO或者自己创建的资产）分发给平台用户。详见：[《yoyow-middleware》#转账到指定用户 transfer ](https://github.com/yoyow-org/yoyow-node-sdk/tree/master/middleware#13-%E8%BD%AC%E8%B4%A6%E5%88%B0%E6%8C%87%E5%AE%9A%E7%94%A8%E6%88%B7-transfer-%E9%9C%80%E8%A6%81%E5%AE%89%E5%85%A8%E9%AA%8C%E8%AF%81%E7%9A%84%E8%AF%B7%E6%B1%82)。    

The transfer operation is provided in the YOYOW SDK, which can be distributed to the platform users by transferring their own assets (YOYO or assets created by themselves). See: [《yoyow-middleware》#transfer to specified user](https://github.com/yoyow-org/yoyow-node-sdk/tree/master/middleware#13-%E8%BD%AC%E8%B4%A6%E5%88%B0%E6%8C%87%E5%AE%9A%E7%94%A8%E6%88%B7-transfer-%E9%9C%80%E8%A6%81%E5%AE%89%E5%85%A8%E9%AA%8C%E8%AF%81%E7%9A%84%E8%AF%B7%E6%B1%82).  

### 付费阅读/打赏
### Paid Reading/Rewards

通过YOYOW网络，平台可方便的接入加密货币的打赏/付费阅读体系。 

Through the YOYOW network, the platform can easily integrate to the system of cryptocurrency rewards/paid reading.

平台可以基于YOYO资产或自己发行的资产，鼓励用户之间转发交易，构建内容付费阅读和打赏的平台。YoYow提供的区块链服务可以保证用户之间的转发交易安全、不可篡改、公开透明，以此激励用户提供更高质量的内容。 

The platform can encourage users to forward transactions between them, and build a platform for paid reading and rewarding content based on YOYO assets or assets issued by itself. YoYow's blockchain service ensures that forwarding transactions between users is secure, non-tamperable, and transparent, thereby motivating users to deliver higher quality content.

在YOYOW的SDK中提供了transferFromUser，可以支持用户对用户通过平台转账（只可以转出零钱内存放的YOYO代理），同时将转账数据写入YOYOW链上。详见：[《yoyow-middleware》#用户对用户通过平台转账](https://github.com/yoyow-org/yoyow-node-sdk/tree/master/middleware#19-%E7%94%A8%E6%88%B7%E5%AF%B9%E7%94%A8%E6%88%B7%E9%80%9A%E8%BF%87%E5%B9%B3%E5%8F%B0%E8%BD%AC%E8%B4%A6-transferfromuser%E9%9C%80%E8%A6%81%E5%AE%89%E5%85%A8%E9%AA%8C%E8%AF%81%E7%9A%84%E8%AF%B7%E6%B1%82)。

The transferFromUser is provided in the YOYOW SDK, which can support the transfer between users through the platform (only the YOYO agent stored in the liquid assets), and write the transfer data to the YOYOW chain. See: [《yoyow-middleware》#transfers between users through the platform](https://github.com/yoyow-org/yoyow-node-sdk/tree/master/middleware#19-%E7%94%A8%E6%88%B7%E5%AF%B9%E7%94%A8%E6%88%B7%E9%80%9A%E8%BF%87%E5%B9%B3%E5%8F%B0%E8%BD%AC%E8%B4%A6-transferfromuser%E9%9C%80%E8%A6%81%E5%AE%89%E5%85%A8%E9%AA%8C%E8%AF%81%E7%9A%84%E8%AF%B7%E6%B1%82).

更多：

More:

1. YOYOW Node SDK: <https://github.com/yoyow-org/yoyow-node-sdk>.
2. YOYOW Node SDK Middleware: <https://github.com/yoyow-org/yoyow-node-sdk/tree/master/middleware>.

