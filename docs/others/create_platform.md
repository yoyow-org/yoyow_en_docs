# 从0开始创建YOYOW平台账户
# Creating YOYOW Platform Account from 0
本文将以测试网环境为例，介绍创建平台所需要的操作，比如在网页钱包中注册账号，在cli中通过命令行创建平台。

This article will take the testnet environment as an example to introduce the operations required to create the platform, such as registering an account in the web wallet and creating a platform through the command line in the cli.

所需资源：  
Resources Required:

**正式网：**  
**Official Network：** 

网页钱包地址： [https://wallet.yoyow.org](https://wallet.yoyow.org).  
正式网CLI下载： [https://github.com/yoyow-org/yoyow-core/releases](https://github.com/yoyow-org/yoyow-core/releases)

Web Wallet Address： [https://wallet.yoyow.org](https://wallet.yoyow.org).  
Official Network CLI Download： [https://github.com/yoyow-org/yoyow-core/releases](https://github.com/yoyow-org/yoyow-core/releases)


**测试网：**   
**Testnet：**  

网页钱包测试地址： [http://demo.yoyow.org:8000](http://demo.yoyow.org:8000).  
测试网CLI下载： [https://github.com/yoyow-org/yoyow-core-testnet/releases/](https://github.com/yoyow-org/yoyow-core-testnet/releases/).

Web Wallet Testing Address： [http://demo.yoyow.org:8000](http://demo.yoyow.org:8000).  
Testnet CLI Download： [https://github.com/yoyow-org/yoyow-core-testnet/releases/](https://github.com/yoyow-org/yoyow-core-testnet/releases/).


## 1. 创建账号，获取密钥
## 1. Creating Accounts, Getting Private Keys

以测试网为例，访问[测试网网页钱包地址](http://demo.yoyow.org:8000 "yoyow钱包测试网") 注册或登录账号：

Take the testnet as an example, visit [testnet web wallet address](http://demo.yoyow.org:8000 "yoyow wallet testnet") registration or login account:

![creating testnet accounts](/images/sdk/step1.png)

平台所有者的各权限私钥获取方式 登录钱包 --> 左侧菜单设置 --> 账号 --> 查看权限 --> 在对应权限密钥的右侧点击显示私钥 --> 输入密码显示私钥 --> 将看到的私钥拷贝进配置中.

The steps for platform owners to get the private keys of different authority levels are as follows: log in to the wallet --> settings on the left menu --> account --> view authority --> click "display private key" on the right side of the corresponding authority key --> enter the password to display the private key --> copy the private key you see to the configuration.
    
![Get Corresponding Private Keys](/images/sdk/step3.png)

## 2. 创建平台
## 2. Creating Platforms

    创建平台商需要最少 11000 YOYO，其中10000 为最低抵押押金，1000为创建平台手续费（测试网络注册赠送12000 测试币）

    It takes at least 11,000 YOYO to create a platform, of which 10000 is the minimum collateral deposit and 1000 is the platform fee (12000 test tokens are given for testnet registration)

### 2.1 启动cli钱包
### 2.1 Starting the Client Wallet

以测试网为例，从[测试网CLI下载](https://github.com/yoyow-org/yoyow-core-testnet/releases/)下载相应环境下的yoyow-client。

Take the testnet as an example, download the yoyow-client in the corresponding environment from [Testnet CLI Download](https://github.com/yoyow-org/yoyow-core-testnet/releases/).

#### 2.1.1 带参数启动
#### 2.1.1 Starting with Parameters

以Ubuntu为例，连接测试网的node节点（ws://47.52.155.181:10011）

Take Ubuntu as an example, connect the node of the testnet (ws://47.52.155.181:10011)

    ./yoyow_client -s ws://47.52.155.181:10011 

如若提示权限不足 
If it prompts "insufficient authority"

    sudo chmod a+x * 

  

#### 2.1.2 以配置文件启动
#### 2.1.2 Starting with a Configuration File

cli 钱包 同路径下创建wallet.json 文件

Create a wallet.json file under the same path with the cli wallet 

write

    {
      "chain_id": "3505e367fe6cde243f2a1c39bd8e58557e23271dd6cbf4b29a8dc8c44c9af8fe",
      "pending_account_registrations": [],
      "pending_witness_registrations": [],
      "labeled_keys": [],
      "blind_receipts": [],
      "ws_server": "ws://47.52.155.181:10011",
      "ws_user": "",
      "ws_password": ""
    }

然后可以直接运行 
then you can directly run

    ./yoyow_client

### 2.2 设置钱包密码
### 2.2 Setting Wallet Password

    连接成功出现
    Upon successful connection, it shows

    Please use the set_password method to initialize a new wallet before continuing

    new >>>

    Execute

    new >>> set_password your password

    Return

    set_password your password
    null
    locked >>> 

    Execute

    locked >>> unlock your password

    Return

    unlock 123
    null
    unlocked >>>

    means successful unlock

### 2.3 导入资金私钥
### 2.3 Importing Active Keys

    unlocked >>> import_key yoyow account uid active keys

    For example:

    unlocked >>> import_key 120252179 5JwREzpwb62iEcD6J6WXs2fbn1aSKWQWvGLNCqAEYwS31EHD7i4

    Return

    1937037ms th_a       wallet.cpp:820                save_wallet_file     ] saving wallet to file wallet.json
    true

    If it doesn't return true, please check if your uid and private key are correct.

### 2.4 创建平台
### 2.4 Creating Platforms

    unlocked >>> create_platform yoyow账号uid "平台名称" 抵押金额 货币符号 "平台url地址" "平台拓展信息json字符串" true
    
    Unlocked >>> create_platform yoyow account uid "platform name" collateral amount currency symbol "platform url address" "platform extra data json string" true

    For example:

    unlocked >>> create_platform 235145448 "myPlatform" 10000 YOYO "www.example.com" "{}" true

    Return

    {
      "ref_block_num": 33094,
      "ref_block_prefix": 2124691028,
      "expiration": "2018-02-07T08:40:30",
      "operations": [[
          20,{
            "fee": {
              "total": {
                "amount": 1029296,
                "asset_id": 0
              },
              "options": {
                "from_csaf": {
                  "amount": 1029296,
                  "asset_id": 0
                }
              }
            },
            "account": 235145448,
            "pledge": {
              "amount": 1000000000,
              "asset_id": 0
            },
            "name": "myPlatform",
            "url": "www.example.com",
            "extra_data": "{}"
          }
        ]
      ],
      "signatures": [
        "1f08b704dd5ccf7e05e5dec45b06ad41e6382f5dd528e3f644d52ff4fb29c2040507544d5e94b84d77d70edcd68bb35b0cded0db87816ae64979ba98eeb641d5d7"
      ]
    }

### 2.5 更新平台
### 2.5 Updating Platforms
    unlocked >>> update_platform yoyow账号uid "平台名称" 抵押金额 货币符号 "平台url地址" "平台拓展信息json字符串" true
    
    Unlocked >>> update_platform yoyow account uid "platform name" collateral amount currency symbol "platform url address" "platform extra data json string" true

    For example:

    unlocked >>> update_platform 235145448 "newplatformname" 10000 YOYO null null true

    返回与创建平台一样
    the way of returning is the same with creating a platform

    平台名称、平台url地址和平台拓展信息如没有变动则填入null，如示例操作，不会改变平台url地址和拓展信息
    
    The platform name, platform url address, and platform extra data are filled in with null if there is no change. As the operation in the example, the platform url address and extra data will not be changed.

### 2.6 平台拓展信息协议
### 2.6 Platform Extra Data Protocol

    平台属性中 extra_data 的拓展信息为选填内容，YOYOW登录的相关协议中规定拓展信息为JSON对象格式字符串格式，并遵循如下格式 
    
    The extra data of extra_data in the platform attribute is optional content. The relevant information in the YOYOW login protocol specifies the JSON object format string format and follows the format below.

    {

    "login":"http://example/login" Platform scan-code login request interface

    "description":"platform description"  platform description

    "image":"http://example.image.jpg" platform image，platform image displayed in yoyow app 1.1

    "h5url":"http://exampleH5.com" Platform h5 address, used to adjust the h5 page without the app jumping

    "packagename":"com.example.app" Platform android jump

    "urlscheme":"example://"  Platform ios jump

    }

