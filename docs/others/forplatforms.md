# Introduction to Platform Integration

YOYOW is a media public chain based on blockchain technology. Its goal is to build a network that utilizes blockchain technology and uses decentralized consensus method to price contributions and provide equity returns for content production areas, enabling content producers, content investors, content filterers and eco-builders to get reasonable incentives and rewards. YOYOW focuses on the underlying technology and large profit distribution settings, and assigns greater autonomy to integrated platforms and developers. Platforms and developers can build their own blockchain-based content products by relying on YOYOW without much blockchain expertise. 

Integration of the media platforms can bring the following advantages:

- Realizing the integration of current business + blockchain, introducing a reasonable incentive mechanism, and effectively enhancing user enthusiasm
- Easily realizing the issuance of token assets and getting convenient token monetization channels
- Can directly get YOYOW incentives
- Can share all current users of YOYOW

## Introduction to YOYOW Ecology

### User Roles 

In the system ecology, in order to balance efficiency and fairness, YOYOW has designed a complete user system, including common users, platforms, committees, and witnesses.

#### Common Users
Common users are the main participants of the YOYOW network. In the YOYOW network, common users share a unified account system on each platform, and have the rights of transfers, election, etc.

#### Platforms
The platform is a special kind of user. Common user that deposits a collateral of a certain amount of YOYO tokens can be called a platform. 
Common users can authorize the platform through the YOYOW wallet, and the authorized platform uses its Tipping authority for cross-site login, content rating, content posting, comments, etc., and common users can also revoke authorization at any time. After integration, the platforms can get token incentives of the YOYOW network.

#### Committees
The committee is the governing body of the YOYOW network, responsible for initiating and voting on the proposals. The content of the proposals is mainly to adjust dozens of adjustable parameters of the system, such as the rate of various transactions, the highest scoring weight, etc., including as well the block production interval, block rewards, etc.
The committee candidates are voted in by the users of YOYOW and the users can withdraw their votes at any time to ensure that the resolutions of the committee truly reflect the wishes of the majority of the token holders.

#### Witnesses
The witness is the producer of the block, responsible for collecting the various transactions of the broadcast and packing them into the blocks, so the witnesses producing blocks can also get the corresponding YOYO token as returns.

The witnesses include Top witnesses, Standby witnesses, and Miner witnesses.

Under the current mechanism, YOYOW users can fully exercise their rights. Both the committee and the witnesses are elected by votes, and all is decided according to the number of votes. Users can vote for an account that is willing to participate in community building and is willing to contribute to the community. At the same time, each vote is valid for 90 days, which guarantees the vitality of the committee and the witness team.

### Private Key and Assets

#### Private Key System

In the blockchain project, the private key is the top priority of the account. However, when a user does any operation and uses the account private key to sign, it will undoubtedly greatly increase the risk of the account. Considering the different security levels of different authorities, the three-level key system is set in the YOYOW system: **the Owner Key**, **the Active Key**, and **the Secondary Key**. The Owner Key is the key of highest authority, and the Owner Key should not be used unless it is absolutely necessary. The Active Key can operate the balance. The Secondary Key can operate the Tipping, and can also be used for posting, giving likes, login authentication, etc.

 Simply speaking:
1. The Owner key is the core. If other low-level keys are lost, you can use the Owner key to reset and update. Therefore, it must be kept properly and once lost, it cannot be retrieved.
2. For fund operations such as transfer, you can use the Active key.
3. The users who want to authorize the platforms only need to grant the Secondary key (Tipping authority), through which the platforms can act as proxy for the users to post, give likes and make small-amount transfers.

In addition to the asset-related 3-level keys, there is also a **Memo Key** used to encrypt and view the note information for the transactions.

Each of the above groups of keys consists of a pair of public and private key pairs. The public key is 53 characters starting with YYW and the private key is 51 characters. Each key can be viewed through [web wallet](https://wallet.yoyow.org/) (see: [yoyow keys tutorial](https://steemit.com/cn/@peterchen145/yoyow-keys-tutorial-yoyow) ). For example:

![web wallet private key screenshot](/images/others/account_keys.png)

#### Types of Assets
There are three types of assets in the YOYOW network: **YOYO token**, **user-issued assets** (UIA), and **bonus points**.

##### YOYO Token
In YOYO's account, the base asset is YOYO token, and there are two places to store: 『**balance**』and『**tipping**』.

- 『**Balance**』 has a high level of security, and it is recommended that a large amount of YOYO be stored in the balance.

- 『**Tipping**』, which can be understood as password-free payment, is for small amount of savings and transfer. When the user grants the authority of tipping to the platform, the platform can even have the right to operate "Tipping".

##### User-Issued Assets（UIA）
Users can issue custom assets: users can create Tokens that circulate within their own platforms based on their business. A convenient exchange method between the user-issued assets (UIA) and YOYO will be provided subsequently, sharing convenient monetization channels.

##### Bonus Points
Bonus Points are rewards for YOYO holders. When your account holds YOYO, Bonus Points will be accumulated over time. There is an upper limit for Bonus Points accumulated, and you have to withdraw Bonus Points manually.

###### Using Bonus Points

At present, the only use of Bonus Points is for paying fees. You can use Bonus Points to offset fees. 100 Bonus Points can be used as 1 YOYO.

When the “balance” of your account reaches a certain amount of YOYO, the system will automatically accumulate Bonus Points for you. There is a limit to the highest amount of accumulation.

###### Using Method for Bonus Points 
Bonus Points are withdrawn manually. You can manually withdraw available Bonus Points to your account or other accounts of your contact as incentives. Bonus Points can be rented to others for use.

YOYOW introduces Bonus Points, which can reduce the expenditure of common users' small amount of fees (content release, likes, etc.) and increase the enthusiasm of users. In other blockchain projects, different levels of fees are required for chain operations. For YOYOW, this bonus point system makes it possible for users to operate for free.

## Integration Process

![Integration Process Diagram](/images/others/joinup_process.png)

<center>Integration Process</center>

### Account Registration

To apply for a YOYOW account, you need to submit a registration page. Access the webpage <https://wallet.yoyow.org/#/create-account>, set the password, and YOYOW will automatically return the account name and account private key. The account name is the number UID (yoyo number), the private key is the Owner key. The private key will not be saved by the system, and will only appear once upon successful registration. It has the highest authority to manage the account. Please save it properly. It is not recommended to save it to a computer, net-disk or cloud note. You can write it on multiple notes and save it to a few more secure locations, ensuring that they will not be lost or leaked.

![Registration](/images/others/signup.png)

![Successful Registration](/images/others/signup_success.png)

After completing the registration, you can view your Owner key, Active key, Secondary key and Memo key in 『Settings』-- 『Accounts』.

### Updating to Platform Accounts

A common account needs to be upgraded to a platform account to obtain the user's authorization. The upgraded platform currently needs to deposit a collateral of 10,000 YOYO and a fee of 1000 YOYO. This operation is temporarily performed through the YOYOW client. You need to perform the create_platform operation through the interface, and set the name of the platform, the symbol of the Token and the related url address and other extra data, such as the API access interface provided by the platform.

For details on how to deposit the upgraded platform account, see [Creating YOYOW Platform Account from scratch](../others/create_platform.html).

### Authorization Certification 

YOYOW provides an authorization certification similar to OAuth. YOYOW provides middleware to facilitate platform integration. The middleware provides interfaces such as signature platform (sign), signaturea authentication (verify), and return QR code on signature platform (SignQR) (see: [《yoyow-middleware》#About Auth](../sdk/intro.html#about-auth)). The platform can generate a binding link through the sign or signQR interface in the SDK, and can verify user authorization through the verify interface. Through authorization, the platform can obtain the user's Tipping management authority and login authorization.

Authorization Process：
 
 ![OAuth Process](/images/others/oauth_process.png)

Note:  

1. Considering security issues, the YOYOW middleware SDK is strongly recommended to be deployed separately on intranet machines without opening IP and ports.

2. YOYOW middleware SDK requires Tipping authority. When using, remember to modify the default Secondary key and Memo key in the configuration file.

### Issuing Assets

The media can issue its own assets as needed, and the number of assets and exchange rates can be set by their own needs. The yoyow platform provides the convenience of circulation and trading for set assets.

The issued assets are temporarily operated by the create_asset function of the wallet client, and the total amount of assets, the transaction rate of the transaction, and the whitelist and blacklist settings need to be specified.

See：[《YOYOW Wallet API》#create_assets](../api/wallet_api.html#create-asset).

### User Incentives

The platform can specify the incentive plan for its own business needs. The target of the incentive can be YOYO or the toekns issued by itself.

Taking Biask as an example, Biask has been integrated to the YOYOW network. In the platform of Biask, the system reward period is one week, and the likes, the like energy and the number of comments obtained by the user's posted content in a week are regularly counted and a bonus value is calculated, and YOYO token will be issued according to the proportion of the user's bonus value. According to the user's activity and content contribution value, the user will have prestige attributes. If praised by users with higher prestige, the users get higher like energy, and the liked users will also get higher rewards. Users can withdraw YOYO from the web wallet to other places for trading.

### Paid Reading/Rewards

Through the YOYOW network, the platform can easily integrate to the system of cryptocurrency rewards/paid reading.

The platform can encourage users to forward transactions between them, and build a platform for paid reading and rewarding content based on YOYO assets or assets issued by itself. YoYow's blockchain service ensures that forwarding transactions between users is secure, non-tamperable, and transparent, thereby motivating users to deliver higher quality content.


More:
1. YOYOW Middleware <https://github.com/yoyow-org/yoyow-middleware>.
