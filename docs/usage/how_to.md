# 用户注册
# User Registration

## 注册账户
## Registering Accounts
YOYOW平台的账号是一串数字（类似QQ号码），注册时由系统分配，未来会开放昵称功能。

The account of the YOYOW platform is a string of numbers (similar to the QQ number). It is assigned by the system when registering, and the nickname function will be released in the future.

目前只能通过[网页钱包](https://wallet.yoyow.org)或者[客户端钱包](https://yoyow.org/client/index.html)注册账号。
可以参考：[网页钱包注册方法](https://steemit.com/cn/@peterchen145/yoyow-online-wallet-sign-up-tutorial)

Currently, accounts can only be registered through [Web Wallet](https://wallet.yoyow.org) or [Client Wallet](https://yoyow.org/client/index.html).
You can refer to: [Web Wallet Registration Method](https://steemit.com/cn/@peterchen145/yoyow-online-wallet-sign-up-tutorial)

特别需要提醒的是，注册时会生成主控密钥，该私钥仅在注册时候显示一次，网页钱包或者客户端钱包并不会保存，一定要妥善保存。

Special attention is needed here. The system will generate a private Owner Key when registering. The private key only appears once upon successful registration. Please make sure to back up and save it properly since the web wallet and client wallet will not store your private Owner Key. 

## 密钥权限说明
## Private Key Authority

YOYOW的体系中，每个账户有四类密钥
主控密钥：最高权限，能修改本账号的其它权限的密钥。注册成功时显示的密钥即是主控密钥。
资金密钥：拥有动用余额资金的权限，比如控制转账操作。
次级密钥：拥有动用零钱包的权限，其中包括对平台的授权操作，也被称作零钱密钥。
备注密钥：用于查看转账等操作的备注信息。

In the YOYOW system, there are four types of private keys for each account.
- Owner Key: it has the highest authority. It is the priavate key that can modify other authorities of the account. The private key that appears upon successful registration is the Owner Key.
- Active Key: it has the authority to use the assets in the balance, such as controlling the actions of the transfer.
- Secondary Key: it has the authority to use the tipping, including the actions of authorizing the platforms. It is also called tipping Key.
- Memo Key: it is used to check the memo information of the transfer actions.

不同的密钥权限只能做权限内的事情，既不能不能向上越权也不能向下越权。资金密钥没有动用零钱的权限，次级密钥也没有动用余额的权限；主控密钥也没有动用余额和零钱的权限，但主控密钥可以重置其他权限的密钥，在网页钱包中导入账号主控密钥即可重置其他三类密钥。

Authorization at each level is for actions within that level only. No action beyond or below one's authorization is allowed. Active Key does not have the right to tranfer tipping, and Secondary Key does not have the right to transfer balances either. The private Owner Key does not have the authority to use balances and tipping either, but it can reset the private keys of other authority levels. Other three types of private keys can be reset by importing the Owner Key of the account in the web wallet.

更多密钥相关的信息可以参考[yoyow私钥教程](https://steemit.com/cn/@peterchen145/yoyow-keys-tutorial-yoyow)

For more info about private keys, you can refer to [yoyow private key tutorials](https://steemit.com/cn/@peterchen145/yoyow-keys-tutorial-yoyow)

## 密钥的保存
## Private Key Saving

主控密钥仅在注册成功时出现一次，浏览器不会保存该私钥，YOYOW也不会保存该私钥。建议将主控密钥妥善，安全的保存，需要保证不会丢失，不会被盗，在网盘、笔记中明文保存都是比较危险的。一般情况下也建议不要轻易动用主控密钥。

Private Owner Key only appears once upon successful registration. The browser will not save the private key, and neither will YOYOW. It is recommended to save your private Owner Key properly and safely and ensure that it is free from being lost and stolen. It is dangerous to store the private key in plaintext on netdisk or online notes. It is also recommended not to use private Owner Key easily.

在网页钱包中，其他低权限的私钥会使用您的钱包密码进行加密后保存在本地浏览器数据库中，客户端也是同理。某些特殊情况下（如：更换新的计算机、计算机故障或浏览器崩溃）导致您浏览器本地数据丢失，可以再通过导入主控密钥来重建账号。

In the web wallet, other private keys from lower authority levels will be encrypted using your wallet password and stored in the browser's local database. The same is true for the client wallet. Your account can be rebuilt using the private Owner Key while certain circumstances (such as switching computer, computer failure or browser crash) lead to the loss of local data on your browser.
