# 中间件使用说明（Nodejs）
# Instruction for Middleware（Nodejs）

[yoyow-node-sdk](https://github.com/yoyow-org/yoyow-node-sdk/tree/master/middleware)为一些应用场景提供了更简洁的HTTP接口。yoyow-node-sdk本质上是对yoyow_client接口的封装，直接提供了包括发布文章，发帖回帖，交易验证，授权登录等接口。

[yoyow-node-sdk](https://github.com/yoyow-org/yoyow-node-sdk/tree/master/middleware) provides a more compact HTTP interface for some application scenarios. Yoyow-node-sdk is essentially a wrapper around the yoyow_client interface, which provides interfaces such as publishing articles, posting and replying, transaction verification, and authorization login.

### 开始
### Start

#### 1. 创建测试网账号
#### 1. Creating Testnet Accounts

测试网地址 [http://demo.yoyow.org:8000](http://demo.yoyow.org:8000 "yoyow钱包测试网").

Testnet Website [http://demo.yoyow.org:8000](http://demo.yoyow.org:8000 "yoyow钱包测试网").

测试网CLI下载 [https://github.com/yoyow-org/yoyow-core-testnet/releases/](https://github.com/yoyow-org/yoyow-core-testnet/releases/).

Testnet CLI Download [https://github.com/yoyow-org/yoyow-core-testnet/releases/](https://github.com/yoyow-org/yoyow-core-testnet/releases/).

![创建测试网账号](/images/sdk/step1.png)

![Creating testnet accounts](/images/sdk/step1.png)

平台所有者的各权限私钥获取方式 登录钱包 》 左侧菜单设置 》 账号 》 查看权限 》 在对应权限密钥的右侧点击显示私钥 》 输入密码显示私钥 》 将看到的私钥拷贝进配置中。

The steps for platform owners to get the private keys of different authority levels are as follows: log in to the wallet 》 settings on the left menu 》 account 》 view authority 》 click "display private key" on the right side of the corresponding authority key 》 enter the password to display the private key 》 copy the private key you see to the configuration.
    
![获取对应私钥](/images/sdk/step3.png)

![Get Corresponding Private Keys](/images/sdk/step3.png)

#### 2. 创建平台
#### 2. Creating Platforms

    创建平台商需要最少 11000 YOYO，其中10000 为最低抵押押金，1000为创建平台手续费（测试网络注册赠送12000 测试币）
    
    It takes at least 11,000 YOYO to create a platform, of which 10000 is the minimum collateral deposit and 1000 is the platform fee (12000 test tokens are given for testnet registration )

##### 2.1 启动client钱包
##### 2.1 Starting the Client Wallet
###### 2.1.1 带参数启动
###### 2.1.1 Starting with Parameters

    Ubuntu

    ./yoyow_client -s ws://47.52.155.181:10011 --chain-id 3505e367fe6cde243f2a1c39bd8e58557e23271dd6cbf4b29a8dc8c44c9af8fe

    If it prompts "insufficient authority"
    
    sudo chmod a+x * 

  

###### 2.1.2 以配置文件启动
###### 2.1.2 Starting with a Configuration File

    client钱包同路径下创建wallet.json 文件
    
    Create a wallet.json file under the same path with the Client wallet 

    Write

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

    Ubuntu

    ./yoyow_client

##### 2.2 设置钱包密码
##### 2.2 Setting Wallet Password

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

##### 2.3 导入资金私钥
##### 2.3 Importing Asset Private Keys

    unlocked >>> import_key yoyow account uid Active Key
    
    For example:

    unlocked >>> import_key 120252179 5JwREzpwb62iEcD6J6WXs2fbn1aSKWQWvGLNCqAEYwS31EHD7i4

    return

    1937037ms th_a       wallet.cpp:820                save_wallet_file     ] saving wallet to file wallet.json
    true
    
    If it doesn't return true, please check if your uid and private key are correct.

##### 2.4 创建平台
##### 2.4 Creating Platforms

    unlocked >>> create_platform yoyow账号uid "平台名称" 抵押金额 货币符号 "平台url地址" "平台拓展信息json字符串" true

    Unlocked >>> create_platform yoyow account uid "platform name" collateral amount currency symbol "platform url address" "platform extra data json string" true
    
    For example:

    unlocked >>> create_platform 235145448 "myPlatform" 10000 YOYO "www.example.com" "{}" true

    return

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

##### 2.5 更新平台
##### 2.5 Updating Platforms

    unlocked >>> update_platform yoyow账号uid "平台名称" 抵押金额 货币符号 "平台url地址" "平台拓展信息json字符串" true

    Unlocked >>> update_platform yoyow account uid "platform name" collateral amount currency symbol "platform url address" "platform extra data json string" true
    
    For example:

    unlocked >>> update_platform 235145448 "newplatformname" 10000 YOYO null null true

    返回与创建平台一样
    
    the way of returning is the same with creating a platform

    平台名称、平台url地址和平台拓展信息如没有变动则填入null，如示例操作，不会改变平台url地址和拓展信息

    The platform name, platform url address, and platform extra data are filled in with null if there is no change. As the operation in the example, the platform url address and extra data will not be changed.
    
##### 2.6 平台拓展信息协议
##### 2.6 Platform Extra Data Protocol

    platform attributes extra_data extra data
    JSON object format string

    {

    "login":"http://example/login" Platform scan-code login request interface

    "description":"platform description"  platform description

    "image":"http://example.image.jpg" platform image，platform image displayed in yoyow app 1.1

    "h5url":"http://exampleH5.com" Platform h5 address, used to adjust the h5 page without the app jumping

    "packagename":"com.example.app" Platform android jump

    "urlscheme":"example://"  Platform ios jump

    }

##### 2.7 平台扫码登录
##### 2.7 Platform Login by Scanning Code

    App扫码授权登录，将访问拓展信息平台的扫码、登录请求接口， 发送回用户签名对象
    
    App scans code and is authorized to log in and send the scan code and login request interface of the extra data platform back to the user signature object.

    {

      {Number} yoyow - Current operating user account id

      {String} time - Signature timestamp string

      {String} sign - Signature string

      {String} state - Custom information passed in when the platform is signing (refer to Auth related 2.3 - signQR)

    }

    the interface provided by the platform must return the following information

    {

      {Number} code - operation result 0 means passing. Any non-zero condition is considered an error
      
      {String} message - Operation result description

    }

#### 3. 修改中间件配置
#### 3. Modifying Middleware Configuration
  
    ~/yoyow-node-sdk/middleware/conf/config.js

    // api server address
    apiServer: "ws://47.52.155.181:10011",

    // security request effective time, unit s
    secure_ageing: 60,

    // platform security request verification key, customized by the platform
    secure_key: "",

    // platform owner's Active Key (for acquisition method, please refer to 1. Creating Testnet Accounts)
    active_key: "",

    // platform owner's Secondary Key (for acquisition method, please refer to 1. Creating Testnet Accounts)
    secondary_key: "", 

    // platform owner's Memo Key (for acquisition method, please refer to 1. Creating Testnet Accounts)
    memo_key: "",

    // platform id(yoyow id)
    platform_id: "",

    // whether the transfer uses points
    use_csaf: true,

    // whether the transfer is transferred to the balance, otherwise it is transferred to liquid assets
    to_balance: true,

    // wallet authorization page URL
    wallet_url: "http://demo.yoyow.org:8000/#/authorize-service",

    // list of IPs allowed to access
    allow_ip: ["localhost", "127.0.0.1"]
    
#### 4. 安装中间件服务所需node库
#### 4. The Node Library Required to Install the Middleware Service
     enter ~/yoyow-node-sdk/middleware/ directory
    
     npm install
    
#### 5. 启动中间件服务
#### 5. Starting the Middleware Service

     npm start
    
启动正常情况如下图

Normal state of starting is as shown below

![Normal state of starting](/images/sdk/sdk_start.png)
     
### 请求返回 error code 状态说明
### Request for Returning Error Code Status Description
 
    1001 invalid signature type

    1002 invalid signature time

    1003 request has expired

    1004 invalid operation time

    1005 invalid operation signature

    1006 account information does not match the chain (usually after the private key is restored, using the local data of other computers or the old backup file for authorization operation)

    1007 Unauthorized platform

    2000 api underlying exception

    2001 account does not exist

    2002 invalid account

    2003 invalid transfer amount

    2004 liquid assets and points are insufficient for paying fees

    2005 insufficient liquid assets

    2006 invalid asset symbol or id

    3001 Post ID must be the previous post ID +1 of the issuer of the platform (platform post management id)
      
### 请求文档及示例
### Request Documentation and Examples

#### 1. Api 相关
#### 1. About Api 

##### 1.1. 获取指定账户信息 getAccount
##### 1.1. Get Specified Account Information getAccount
  Request type: GET
  
  Request parameters:
  
    {Number} uid - account id
    
  Request example：
  
    localhost:3000/api/v1/getAccount?uid=25638

  Return results：
  
    {
      code: result,
      message: return message,
      data: { // user information
        uid: account uid
        name: account name
        owner: owner authority
        active: active authority
        secondary: secondary authority
        memo_key: memo key public key
        reg_info: registration information
        can_post: can post or not
        can_reply: can reply to posts or not
        can_rate: can comment or not
        is_full_member: is full member or not
        is_registrar: is registrar or not
        is_admin: is administrator or not
        statistics: { //user YOYO asset details
          obj_id: asset object id
          core_balance: balance
          prepaid: liquid assets
          csaf: bonus points
          total_witness_pledge: witnesses' total collateral（collateral amount of creating witnesses by users）
          total_committee_member_pledge: committee total amount（collateral amount of creating committee by users）
          total_platform_pledge: platform total collateral（collateral amount of creating platforms by users）
          releasing_witness_pledge: witness collateral to be returned
          releasing_committee_member_pledge: committee collateral to be returned
          releasing_platform_pledge: platform collateral to be returned
        }
        assets: [ //total assets held by users
            {
                amount: asset amount,
                asset_id: asset id,
                precision: asset precision,
                symbol: asset symbol,
                description: asset description"
            }
            ...
        ]
      }
    }

##### 1.2. 获取指定账户近期活动记录 getHistory
##### 1.2. Get Recent Activity Records for a Given Account getHistory
  Request type: GET
  
  Request parameters：
  
    {Number} uid - account id

    {Number} op_type - query op type. '0' is transfer op，default is null, i.e. query all OP types

    {Number} start - query start number. When it is 0, the query starts from the latest record. The default is 0.

    {Number} limit - the length of the query，the maximum cannot exceed 100，the default is 10.

  Request example：
  
    localhost:3000/api/v1/getHistory?uid=25638&start=1220&limit=30&op_type=0

  Return results：
  
    {
      code: operation results,
      message: return message,
      data: [] Array of history objects
    }

##### 1.3. 转账到指定用户 transfer （需要安全验证的请求）
##### 1.3. Transfer to Specified User Transfer （requests requiring for security verification)
  Request Type: POST
  
  Request parameters：

    {Object} cipher - Request object ciphertext object

             {

               ct, - Ciphertext hexadecimal

               iv, - Vector hexadecimal

               s   - salt hexadecimal

             }

    Request object structure:

    {Number} uid - specified user id

    {Number} amount - transfer amount

    {Number} asset_id - asset id 

    {string} memo - memo

    {Number} time - operation time
  
  Request example: refer to security request verification
    
  return results：
  
    {
      code: operation results,
      message: return messages,
      data: {
        block_num: operation block number
        txid: operation id
      }
    }

##### 1.4. 验证块是否不可退回 confirmBlock
##### 1.4. Verify Whether the Block is Unreturnable confirmBlock
  Request Type：GET
  
  Request parameter:
  
    {Number} block_num - Verified block number
  
  Request example：
  
    localhost:3000/api/v1/confirmBlock?block_num=4303231
  
  Return results：
  
    {
      code: operation results,
      message: return massages,
      data: is the block returnable or not 
    }

##### 1.5. 发送文章 post（需要安全验证的请求）
##### 1.5. Sending Post (request  requiring for security verification)
  Request Type：POST

  request parameter：


    {Object} cipher - Request object ciphertext object

             {

               ct, - ciphertext hexadecimal

               iv, - Vecotr hexadecimal

               s   - salt hexadecimal

             }

  Request object structure:

    {Number} platform - platform account

    {Number} poster - poster account

    {Number} post_pid - post number

    {String} title - post title

    {String} body - post content

    {String} extra_data - post extra data

    {String} origin_platform - original post platform account（default null）

    {String} origin_poster - original post poster account（default null）
    
    {String} origin_post_pid - original post number（default null）

    {Number} time - operation time

  Request example: refer to security request verification
    
  return results：
  
    {
      code: operation results,
      message: return messages,
      data: {
        block_num: operation block number
        txid: operation id
      }
    }

##### 1.6. 更新文章 postUpdate（需要安全验证的请求）
##### 1.6. Updating Post postUpdate (request requiring for security verification)
Request Type：POST

  request parameters：


    {Object} cipher - Request object ciphertext object

             {

               ct, - ciphertext hexadecimal

               iv, - Vector hexadecimal

               s   - salt hexadecimal

             }

  Request object structure:

    {Number} platform - platform account

    {Number} poster - poster account

    {Number} post_pid - post number

    {String} title - post title

    {String} body - post content

    {String} extra_data - post extra data

    {Number} time - operation time

  备注：修改文章操作时，title，body 和 extra_data 必须出现至少一个，并且与原文相同字段的内容不同
  
  Note: When modifying the post operation, at least one of title, body and extra_data must appear, and the content of the same field as the original text is different.

  Request example: refer to security request verification

  return results：
  
    {
      code: operation results,
      message: return messages,
      data: {
        block_num: operation block number
        txid: operation id
      }
    }

##### 1.7. 获取文章 getPost
##### 1.7. Getting Post getPost

  Request Type：GET

  request parameters：
    
    {Number} platform - platform account

    {Number} poster -poster account

    {Number} post_pid - post number

  request example：

    http://localhost:3001/api/v1/getPost?platform=217895094&poster=210425155&post_pid=3

  return results：

    {
      code: operation results,
      message: return message,
      data: {
        "id":"1.7.12", - post objectId
        "platform":217895094, - platform account
        "poster":210425155, - poster account
        "post_pid":5, - post number
        "hash_value":"bb76a28981710f513479fa0d11fee154795943146f364da699836fb1f375875f", - post body hash value
        "extra_data":"{}", - extra info
        "title":"test title in js for update", - post title
        "body":"test boyd in js for update", - post content
        "create_time":"2018-03-12T10:22:03", - post creating time
        "last_update_time":"2018-03-12T10:23:24", - post latest update time
        "origin_platform", - original post platform account （Only exist for forwarding a post when posting）
        "origin_poster", - original post poster account （Only exist for forwarding a post when posting）
        "origin_post_pid" - original post posting number （Only exist for forwarding a post when posting）
      }
    }

##### 1.8. 获取文章列表 getPostList
##### 1.8. Getting Post List getPostList
  Request Type：GET

  request parameters：
    
    {Number} platform - platform account

    {Number} poster -poster account（default null，query all posts on the platform when it is null）

    {Number} limit - load number（default 20）

    {String} start - starting time 'yyyy-MM-ddThh:mm:ss' ISOString（When the next page is loaded, the last create_time of the currently loaded data is passed in. If it is not passed, it is loaded from the beginning.）
    

  Request example：

    http://localhost:3001/api/v1/getPostList?platform=217895094&poster=210425155&limit=2&start=2018-03-12T09:35:36

  Return results：

    {
      code: operation results,
      message: return message,
      data: [post object（Refer to the returned data structure of getting a single post）]
    }
    
##### 1.9. 获取转账二维码文本（YOYOW APP 扫码可扫此二维码）
##### 1.9. Getting Transfer QR Code Text（this QR code can be used for YOYOW APP）
  Request Type：GET

  request parameters：

    {Number} amount - receipt amount （If the receipt note is not filled, the user can enter it in the APP.）

    {String} memo - receipt note （If the receipt amount is not filled, the user can enter it in the APP.）

    {String | Number} asset - transfer asset symbol or asset ID (default is YOYO asset)

  request example：

    http://localhost:3001/api/v1/getQRReceive?amount=98&memo=new transfer&asset_id=0
    
  return results：
  
    {
      code: operation results,
      message: return message,
      data: receipt QR code string
    }

##### 1.10. 修改（仅增加白名单）授权用户资产白名单 updateAllowedAssets（需要安全验证的请求）
##### 1.10. Modify (whitelist only) Authorized User Asset Whitelist updateAllowedAssets (requires security verification request)

  Request Type：POST

  request parameters：


    {Object} cipher - Request object ciphertext object

             {

               ct, - cipertext hexadecimal

               iv, - vector hexadecimal

               s   - salt hexadecimal

             }

  Request object structure:

    {Number} uid - target account id

    {Number} asset_id - asset id

  request example：refer to Security Request Verification

  return results：
  
    {
      code: operation results,
      message: return message,
      data: {
        block_num: operation block number
        txid: operation id
      }
    }

##### 1.11. 获取指定资产信息 getAsset
##### 1.11. Getting Specified Asset Information getAsset
  Request Type：GET

  request parameters：
    
    {String | Number} search - asset symbol（capital）or asset id

  request example：

    http://localhost:3001/api/v1/getAsset?search=YOYOW

  return results：

    {
      code: operation results,
      message: return message,
      data: {
        "id":"1.3.0", - asset object id
        "asset_id":0, - asset id
        "symbol":"YOYO", - asset symbol
        "precision":5, - asset precision
        "issuer":1264, - asset issuer uid
        "options":{
          "max_supply":"200000000000000", - circulation limit
          "market_fee_percent":0, - transaction fee percentage
          "max_market_fee":"1000000000000000", - transaction fee maximum
          "issuer_permissions":0, - asset availability
          "flags":0, - asset authority
          "whitelist_authorities":[], - asset whitelist administrator list 
          "blacklist_authorities":[], - asset blacklist administrator list
          "whitelist_markets":[], - trading pair whitelist
          "blacklist_markets":[], - trading pair blacklist
          "description":"" - asset description
        },
        "dynamic_asset_data_id":"2.2.0", - asset dynamic object id
        "dynamic_asset_data":{
          "id":"2.2.0", - asset dynamic object id
          "asset_id":0,
          "current_supply":"107384564466939", - current supply of assets
          "accumulated_fees":0
        },
        "current_supply":"107384564466939",  - current supply of assets
        "accumulated_fees":0
      }
    }

##### 1.12. 获取指定平台信息 getPlatformById
##### 1.12. Getting Specified Platform Information getPlatformById

  Request Type：GET

  request parameters：
    
    {Number} uid - platform owner account uid

  request example：

    http://localhost:3001/api/v1/getPlatformById?uid=217895094

  return results：

  {

    "id": "1.6.0", - platform object id
    "owner": 217895094, - platform owner account uid
    "name": "test-yoyow", - platform name
    "sequence": 1,
    "is_valid": true, - is valid or not
    "total_votes": 0, - platform total votes
    "url": "http://demo.yoyow.org/", - platform url address
    "pledge": 1000000000, - platform collateral（YOYO）
    "pledge_last_update": "2018-02-10T01:03:57", - platform collateral last update time
    "average_pledge": 176601774, - platform average collateral
    "average_pledge_last_update": "2018-02-11T06:49:12", - platform average collateral last update time
    "average_pledge_next_update_block": 4562164, - latform average collateral next update block number
    "extra_data": "{}", - platform extra data 
    "create_time": "2018-02-10T01:03:57", - platform creating time
    "last_update_time": "2018-02-11T06:49:12" - platform latest update date

  }

#### 2. Auth 相关
#### 2. About Auth 

##### 2.1. 签名平台 sign
##### 2.1. Signature Platform sign
  Requst Type：GET

  request parameters：null

  request example：
  
    localhost:3000/auth/sign

  return results：

    {
      code: operation results,
      message: return message,
      data: {
        sign: signature results,
        time: operating time millisecond value,
        platform: signature platform owner id,
        url: wallet authorization url
      }
    }

##### 2.2 签名验证 verify
##### 2.2 Signature Verification verify

  Request Type：GET

  request parameters：

    {Number} yoyow - account id
    
    {Number} time - operating time millisecond value
    
    {String} sign - signature results

  request example：

    localhost:3000/auth/verify?sign=20724e65c0d763a0cc99436ab79b95c02fbb3f352e3f9f749716b6dac84c1dc27e5e34ff8f0499ba7d94f1d14098c6a60f21f2a24a1597791d8f7dda47559c39a0&time=1517534429858&yoyow=217895094

  return results：

    {
      code: operation results,
      message: return message,
      data: {
        verify: is the signature successful or not,
        name: signed YOYOW user name
      }
    }

##### 2.3 签名平台 返回二维码 signQR
##### 2.3 Signature Platform Returned QR Code signQR
  
  Request Type：GET

  request parameters：

    {String} state - 
    The extra data will be sent to the platform together with the user signature information when the platform login interface is invoked. It is used when the platform login interface needs a customized parameter. If there is no such requirement, it may not be transmitted.

  request example：

    localhost:3000/auth/signQR?state=platformCustomParams

  return results：

    {
      code: operation results,
      message: return message,
      data: QR code picture base64 string
    }

### 安全请求验证
### Security Request Verification

    Operations related to financial security will be verified for their effectiveness in middleware services

    使用方自定义key配置于 config 中的 secure_key 里
    The user custom key is configured in secure_key in config.

    将操作对象加密传入
    Encrypt the operation object and transmit

    Encryption exmaple (the crypto-js version of javascript; other languages use similar AES encryption)

    Default mode CBC , padding scheme Pkcs7

    transfer operation

    let key = 'customkey123456'; // this key is the same as the secure_key in the config in the middleware.

    let sendObj = {
      "uid": 9638251,
      "amount": 100,
      "memo": "hello yoyow",
      "time": Date.now()
    }

    time 字段 操作时间取当前时间毫秒值 加密操作须带有此字段 用于验证操作时效
    the operation time of time field takes the current time millisecond value. The encryption operation must have this field to verify the operation time.

    let cipher = CryptoJS.AES.encrypt(JSON.stringify(sendObj), key);

    $.ajax({
      url: 'localhost:3000/api/v1/transfer',
      type: 'POST',
      data: {
        ct: cipher.ciphertext.toString(CryptoJS.enc.Hex),
        iv: cipher.iv.toString(),
        s: cipher.salt.toString()
      },
      success: function(res){
        // do something ...
      }
    })

    PHP encryption method

    function cryptoJsAesEncrypt($passphrase, $value){
      $salt = openssl_random_pseudo_bytes(8);
      $salted = '';
      $dx = '';
      while (strlen($salted) < 48) {
          $dx = md5($dx.$passphrase.$salt, true);
          $salted .= $dx;
      }
      $key = substr($salted, 0, 32);
      $iv  = substr($salted, 32,16);
      $encrypted_data = openssl_encrypt($value, 'aes-256-cbc', $key, true, $iv);
      $data = array("ct" => bin2hex($encrypted_data), "iv" => bin2hex($iv), "s" => bin2hex($salt));
      return json_encode($data);
    }
    
    如 请求文档及示例 1.3. 转账到指定用户 transfer

    其他需要安全请求验证的操作根据文档改动sendObj
    
    Such as request document and example 1.3. Transfer to the Specified User transfer

    Other operations that require security request verification change sendObj according to the documentation
