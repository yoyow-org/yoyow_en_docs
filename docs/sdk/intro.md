# YOYOW Middleware Instruction（Nodejs）

Using YOYOW middleware is the easiest way to integrate with the platform. It mainly provides three interfaces: account authorization, platform incentives and content chaining. You can use Docker one-click deployment to get the corresponding API, and easily interact with the YOYOW blockchain.

YOYOW middleware communicates with YOYOW network through the API interface of YOYOW node, which provides platform service providers with convenient access to data on the chain, ensuring that the traditional business codes can also reach the requirements of being on chain with only minimal changes. The specific diagram is as follows:
![YOYOW middleware role diagram](/images/sdk/architecture.png)

For the creation steps of the platform, please refer to: [Create a YOYOW platform account from scratch](../others/create_platform.html)


## Deployment Start

### Configuration File Description

The path to the configuration file is in the `conf/config.js` file in the code path. If you start it in docker mode, you can map the configuration file to the `/app/conf` directory in the container.

```javascript
{
    // The api server address, the testnet public api address is as follows, for the official network deployment, please change the address
    apiServer: "ws://47.52.155.181:10011",
    
    // The validity time of the security request, and the unit is "s". If the requested content exceeds the validity period, it will return 1003 the request has expired.
    secure_ageing: 60,
    
    // The platform security request verification key can be customized. For details, see "Security Access".
    secure_key: "",
    
    // Platform owner active key 
    active_key: "",
    
    // Platform owner secondary key
    secondary_key: "", 
    
    // Platform owner memo key
    memo_key: "",
    
    // Platform id (yoyow id)
    platform_id: "",
    
    // Whether to use points for the operating fee
    use_csaf: true,
    
    // Whether the transfer is transferred to the balance, otherwise it is transferred to tipping
    to_balance: false,
    
    // Wallet authorization page URL, testnet address is as follows, official network address “https://wallet.yoyow.org/#/authorize-service”
    wallet_url: "http://demo.yoyow.org:8000/#/authorize-service",
    
    // The IP list that is allowed to access； forcing the specific IP address to be specified. "*" or "0.0.0.0" is not supported at this time.
    allow_ip: ["localhost", "127.0.0.1"]
}
```

Note:

1. In the general use scenario, the middleware value needs to use the secondary key and the memo key at most, and just the secondary key and the memo key can satisfy most of the requirements. Do not write the active key into the configuration file unless you are sure you need to use the active key.
2. The middleware uses the restriction IP (`allow_ip`) and encryption request (`secure_key`) to ensure security. However, it is still strongly recommended that the intranet be deployed and isolated, and the security of the private key is quite important.
3. It is recommended to use the point deduction for the operation fee. If the deduction fails, it will directly report the error and will not automatically deduct the tipping as the fee.


### Docker One-Click Deployment
```bash
docker run -itd --name yoyow-middleware -v <Local configuration file path>:/app/conf -p 3001:3001 yoyoworg/yoyow-middleware
```

### Manual Deployment
1. clone source code
  `git clone git@github.com:yoyow-org/yoyow-node-sdk.git`
2. Modify middleware configuration；modify the file `yoyow-node-sdk/middleware/conf/config.js` with reference to the configuration file description ().
3. The node library required to install the middleware service; go to the `~/yoyow-node-sdk/middleware/` directory and find `npm install`.
4. Start middleware service
  `npm start`

Normal start as shown below
![Normal start situation as shown](/images/sdk/step4.png)

## Interface Descriptions

### Request Documentation and Examples

#### 1. Basic Query Related Interface

##### 1.1. Get the Specified Account Information 
getAccount

 Request Type：GET

 Request Parameters：

    {Number} uid - account id

 Request Example：

    localhost:3000/api/v1/getAccount?uid=25638

 Return Results：

    {
      code: results,
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
          prepaid: tipping
          csaf: bonus points
          total_witness_pledge: witnesses' total collateral (collateral amount of creating witnesses by users)
          total_committee_member_pledge: committee total collateral（collateral amount of creating committee by users）
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

##### 1.2. Get Recent Activity Records for a Given account 
getHistory
 Request Type：GET

 Request Parameters：

    {Number} uid - account id
    {Number} op_type - query op type '0' is transfer op, default is null, i.e. query all op types
    {Number} start - query start number. When it is 0, the query starts from the latest record. The default is 0.
    {Number} limit - the length of the query，the maximum cannot exceed 100, the default is 10.

 Request Example：

`localhost:3000/api/v1/getHistory?uid=25638&start=1220&limit=30&op_type=0`

 Return Results：
```
    {
      code: operation results,
      message: return message,
      data: [] array of history objects
    }
```

##### 1.3. Verify Whether Block is Unreturnable 
confirmBlock

 Request Type：GET

Request Parameters：

    {Number} block_num - Verified block number

 Request Example：

    localhost:3000/api/v1/confirmBlock?block_num=4303231

 Return Results：
```
    {
      code: operation results,
      message: return message,
      data: is the block returnable or not 
    }
```

##### 1.4. Get Specified Asset Information 
getAsset

  Request Type：GET

  Request Parameters：

```
{String | Number} search - asset symbol（capital）or asset id
```

  Request Example：

```
http://localhost:3001/api/v1/getAsset?search=YOYOW
```

  Return Results：

```

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

```


##### 1.5. Get Specified Platform Information 
getPlatformById

  Request Type：GET

  Request Parameters：
	{Number} uid - platform owner account uid

  Request Example：

```
http://localhost:3001/api/v1/getPlatformById?uid=217895094

```

  Return Results：

```
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
    "average_pledge_next_update_block": 4562164, - platform average collateral next update block number
    "extra_data": "{}", - platform extra data 
    "create_time": "2018-02-10T01:03:57", - platform creating time
    "last_update_time": "2018-02-11T06:49:12" - platform latest update date

  }
```

#### 

#### 2. Platform Incentives Related Interface
##### 2.1. Transfer to Specified User 
transfer （requiring security verification request）

 Request Type：POST

 Request Parameters：

```
 {Object} cipher - Request ciphertext object，the format is as follows
{
  ct, - ciphertext hexadecimal
  iv, - vector hexadecimal
  s   - salt hexadecimal
}
```

Request Object Structure:
```
{Number} uid - specified user id
{Number} amount - transfer amount
{Number} asset_id - asset id 
{string} memo - memo
{Number} time - operation time
```
Request Example：refer to security request verification

```
localhost:3000/api/v1/transfer
```

 Return Results：

```
{
  code: operation results,
  message: return message,
  data: {
    block_num: operation block number
    txid: operation id
  }
}
```



##### 2.2. Get Transfer QR Code Text
getQRReceive（this QR code can be used for YOYOW APP）

  Request Type：GET

  Request Parameters：

```
{Number} amount - receipt amount （If the receipt note is not filled, the user can enter it in the APP）

{String} memo - receipt memo （If the receipt amount is not filled, the user can enter it in the APP）

{String | Number} asset - transfer asset symbol or asset ID（default is YOYO asset）
```

  Request Example：

```
http://localhost:3001/api/v1/getQRReceive?amount=98&memo=new transfer&asset_id=0
```

 Return Results：

```
{
  code: operation results,
  message: return message,
  data: receipt QR code string
}
```

##### 2.3. Modify (whitelist only) Authorized User Asset Whitelist 
updateAllowedAssets (requiring security verification request)

If the user has enabled asset whitelisting, then UIA (user-issued assets) needs to be added to the user's asset whitelist before transactions such as transfers can be made.


  Request Type：POST

  Request Parameters：

```
{Object} cipher - Request object ciphertext object

{
  ct, - cipertext hexadecimal
  iv, - vector hexadecimal
  s   - salt hexadecimal
}
```

  Request Object Structure:

```
{Number} uid - target account id

{Number} asset_id - asset id
```

  Request Example：refer to Security Request Verification
```
localhost:3000/api/v1/updateAllowedAssets
```

 Return Results：

```
{
  code: operation results,
  message: return message,
  data: {
    block_num: operation block number
    txid: operation id
  }
}

```

#### 3. Content Chaining Related Interface

##### 3.1. Send Post (requiring for security verification request)

  Request Type：POST

  Request Parameters：

    {Object} cipher - Request object ciphertext object
    
    {
      ct, - ciphertext hexadecimal
      iv, - vecotr hexadecimal
      s   - salt hexadecimal
    }

  Request Object Structure:

    {Number} platform - platform account
    {Number} poster - poster account
    {Number} post_pid - post number
    {String} title - post title
    {String} body - post content
    {String} extra_data - post extra data
    {String} hash_value - hash value, the defalut value will be the sha256 value of the body content if not provided.
    {Number} origin_platform - original post platform account（default null）
    {Number} origin_poster - original post poster account（default null）
    {Number} origin_post_pid - original post number（default null）
    {Number} time - operation time

  Request Example: refer to Security Request Verification

```
localhost:3000/api/v1/post
```

 Return Results：

    {
      code: operation results,
      message: return message,
      data: {
        block_num: operation block number
        txid: operation id
      }
    }

##### 3.2. Update Post 
postUpdate (requiring security verification request)

Request Type：POST

  Request Parameters：

```
{Object} cipher - Request object ciphertext object

{
  ct, - ciphertext hexadecimal
  iv, - vector hexadecimal
  s   - salt hexadecimal
}
```
Request Object Structure:
```
{Number} platform - platform account

{Number} poster -  poster account

{Number} post_pid - post number

{String} title - post title

{String} body - post content

{String} extra_data - post extra data

{Number} time - operation time
```
  
  Note: When modifying the post operation, at least one of title, body and extra_data must appear, and the content of the same field as the original text is different.
  
  Request Example: refer to Security Request Verification

```
localhost:3000/api/v1/postUpdate
```

 Return Results：
```
{
  code: operation results,
  message: return message,
  data: {
    block_num: operation block number
    txid: operation id
  }
}
```

##### 3.3. Get Post 
getPost

  Request Type：GET

  Request Parameters：

    {Number} platform - platform account 
    {Number} poster -poster account
    {Number} post_pid - post number

  Request Example：

    http://localhost:3001/api/v1/getPost?platform=217895094&poster=210425155&post_pid=3

  Return Results：

```
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
```

##### 3.4. Get Post List 
getPostList

  Request Type：GET

  Request Parameters：
```
{Number} platform - platform account
{Number} poster - poster account（default null，query all posts on the platform when it is null）
{Number} limit - load number（default 20）
{String} start - starting time 'yyyy-MM-ddThh:mm:ss' ISOString（When the next page is loaded, the last create_time of the currently loaded data is passed in. If it is not passed, it is loaded from the beginning.）
```


  Request Example：

    http://localhost:3001/api/v1/getPostList?platform=217895094&poster=210425155&limit=2&start=2018-03-12T09:35:36

  Return Results：

```
{
  code: operation results,
  message: return message,
  data: [post object（Refer to the returned data structure of getting a single post）]
}
```


#### 4. About Auth 
##### 4.1. Signature Platform 
sign

  Requst Type：GET

  Request Parameters：null

  Request Example：

    localhost:3000/auth/sign

  Return Results：

```
 {
      code: operation results,
      message: return message,
      data: {
        sign: signature results,
        time: operation time millisecond value,
        platform: platform owner id,
        url: wallet authorization url
      }
    }
```

##### 4.2 Signature Verification 
verify

  Request Type：GET

  Request Parameters：

    {Number} yoyow - account id
    {Number} time - operation time millisecond value
    {String} sign - signature results

  Request Example：

    localhost:3000/auth/verify?sign=20724e65c0d763a0cc99436ab79b95c02fbb3f352e3f9f749716b6dac84c1dc27e5e34ff8f0499ba7d94f1d14098c6a60f21f2a24a1597791d8f7dda47559c39a0&time=1517534429858&yoyow=217895094

  Return Results：

    {
      code: operation results,
      message: return message,
      data: {
        verify: is the signature successful or not,
        name: signed YOYOW user name
      }
    }


##### 4.3 Signature Platform Returned QR Code 
signQR

  Request Type：GET

  Request Parameters：

    {String} state - The extra data will be sent to the platform together with the user signature information when the platform login interface is invoked. It is used when the platform login interface needs a customized parameter. If there is no such requirement, it may not be transmitted.

  Request Example：

    localhost:3000/auth/signQR?state=platformCustomParams

  Return Results：
```
{
  code: operation results,
  message: return message,
  data: QR code picture base64 string
}
```

##### 4.4 Platform Extra Data Protocol Descriptions

platform attributes extra_data extra data
    JSON object format string
    
```javascript
{
    "login":"http://example/login" //Platform QR code scanning login request interface
    "description":"platform description"  //platform description
    "image":"http://example.image.jpg" //platform image，platform image displayed in yoyow app 1.1
    "h5url":"http://exampleH5.com" //Platform h5 address, used to adjust the h5 page without the app jumping
    "packagename":"com.example.app" //Platform android jump
    "urlscheme":"example://"  //Platform ios jump
}
```

##### 4.5 Platform Login by Scanning QR Code

When the wallet App scans QR code and it will access and post signature object to "login" url in extra data.

```
{
  {Number} yoyow - Current operating user account id
  {String} time - Signature timestamp string
  {String} sign - Signature string
  {String} state - Custom information passed in when the platform is signing (refer to About Auth 4.3 - signQR)
}
```

the interface provided by the platform must return the following information
```
{
  {Number} code - operation result 0 means passing. Any non-zero condition is considered an error
  {String} message - operation result description
}
```
### Request for Returning Error Code Status Description
```
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

2004 tipping and points are insufficient for paying fees

2005 insufficient tipping

2006 invalid asset symbol or id

3001 Post ID must be the previous post ID +1 of the issuer of the platform (platform post management id)
```

### Security Request Verification

Operations related to financial security, such as transfer, posting, and other write operations, will be verified for their effectiveness in the middleware service. The information of such requests needs to be converted into ciphertext by encryption and then sent to the middleware service. The encryption method uses symmetric encryption AES, and the key is `secure_key` in the configuration file.

Encryption example (crypto-js version of javascript, other languages use similar AES encryption)

Default mode CBC , padding scheme Pkcs7

For example：transfer operation
```javascript
    let key = 'customkey123456'; // This key is the same as the secure_key in the config in the middleware.

    let sendObj = {
      "uid": 9638251,
      "amount": 100,
      "asset_id": 0,
      "memo": "hello yoyow",
      "time": Date.now()  //time field 
      The operation time takes the current time millisecond value. 
      Encryption must have this field for verifying the operation time
    }

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
```
PHP encryption
```php
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
```

For other operations that require secure request verification, change sendObj according to the documentation

