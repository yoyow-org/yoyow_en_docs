
Node API Descriptions
=============
.. contents:: :depth: 3

Node Connection Method
-------------

Testing Environment：
:: 
  websocket interface address： ws://47.52.155.181:10011
  jsonrpc interface address： http://47.52.155.181:10011/rpc

Formal Environment：
::
  websocket interface address：ws://139.198.1.234:9000
  jsonrpc interface address： http://139.198.1.234:9000/rpc


use wscat to connect， `wscat installation method <https://www.npmjs.com/package/wscat>`_  
  wscat -c ws://47.52.155.181:10011


use curl post data to connect
  curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_accounts_by_uid", [["250926091"]]], "id": 1}' http://47.52.155.181:10011/rpc

API Classification
----------------
YOYOW的API分类与BTS类似，以下重点介绍database api和history api。
在通过websocket 请求时，参数为一个json字符串，格式如下：

YOYOW's API classification is similar to BTS. The following focuses on database api and history api. When requesting via websocket, the parameter is a json string in the following format:

    {"id":1, "method":"call", "params":[API level,"function name",[specific parameters]]}

在使用时，需要填写API等级，函数名，和具体参数3项，其中API等级可以通过websocket 发送

In use, you need to fill in API level, function name, and specific parameters, of which the API level can be obtained by sending the following strings via websocket:

    {"id":2, "method":"call", "params":[1,"history",[]]}

来获取。比如以上请求会返回：

For example, the above request will return:

::
    {
      "id": 2,
      "jsonrpc": "2.0",
      "result": 2
    }

其中result 为 2，代表着使用history api 时，API等级需要填写2.（注意：不一定每个YOYOW节点都返回同样的配置，这取决于每个节点对暴露API的限制）

Here the result is 2, which means that when using the history api, the API level needs to be filled in 2. (Note: not every YOYOW node returns the same configuration; it depends on each node's limit on exposing the API)

database的API默认可以直接通过指定API等级为0来调用，也可以使用通过

The database API can be called directly by default by specifying the API level to 0 or by querying the result value through using the following strings:

    {"id":2, "method":"call", "params":[1,"database",[]]}

查询到的result的值来调用。


Database API
----------------

1.1.1 get_required_signatures
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
根据给定的交易（可能已包含签名），和给定的备用公钥集合，返回与签署该交易有关的 3 个集合：
Return the 3 sets associated with signing the transaction based on the given transaction (which may already contain the signature), and the given set of spare public keys:
::
 备用公钥集合的一个可用子集，可以用来签署该交易
 可能还需要的公钥（不在签名中，也不在备用公钥集合中）
 交易中已包含的多余签名
 A subset of the available spare public key set, can be used to sign the transaction
 Public key that may also be needed (not in the signature, nor in the set of spare public keys)
 Extra signature already included in the transaction

Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC


Access authorization limit
""""""""""""""""""

null


Request parameters
""""""""""""""""

:trx:             transaction, may have already contained signature
:available_keys:  array of public keys 
For example：["YYW5eDSFYeiqyFRajfPP8tTZMm7fUeyc7H65zmnHtDW4SQJdwqTBD"]

Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0,"get_required_signatures",[{"operations":[[0,{"fee":{"total":{"amount":100000,"asset_id":0}},"from":250926091,"to":223331844,"amount":{"amount":100000,"asset_id":0},"extensions":{}}]]}, ["YYW5eDSFYeiqyFRajfPP8tTZM7mfUeyc7H65zmnHtDW4SQJdwqTBD"]]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_accounts_by_uid", [{"operations":[[0,{"fee":{"total":{"amount":100000,"asset_id":0}},"from":250926091,"to":223331844,"amount":{"amount":100000,"asset_id":0},"extensions":{}}]]}, ["YYW5eDSFYeiqyFRajfPP8tTZM7mfUeyc7H65zmnHtDW4SQJdwqTBD"]]], "id": 1}' http://47.52.155.181:10011/rpc

Return results
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": [
        [
          [
            "YYW5eDSFYeiqyFRajfPP8tTZM7mfUeyc7H65zmnHtDW4SQJdwqTBD"
          ],  //a subset of the available spare public key set, can be used to sign the transaction
          [
            "YYW6fU7Th8uESW9FZnpkhYaTUwtSvn3f2TQxFVA3ef2SSiwdZES71",
            "YYW7UoNSEQAUPvnvoBRVKyPAD9845esnpiK6MgHinsn5yqr5UgT5W"
          ] //public key that may also be needed (not in the signature, nor in the set of spare public keys)
        ],
        []  // extra signature already included in the transaction
      ]
    }

1.1.2 get_accounts_by_uid
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
根据 uid 返回多个账号信息。数量必须 <= 1000。

如果该 uid 不存在，对应位置结果为 null 。

Return multiple account information based on uid. The quantity must be <= 1000.

If the uid does not exist, the corresponding position result is null .

Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC


Access authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""

:account_uids:   uid array，length is less than 1000 for example：["250926091"]

Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0,"get_accounts_by_uid",[["250926091"]]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_accounts_by_uid", [["250926091"]]], "id": 1}' http://47.52.155.181:10011/rpc


Return results
""""""""""""""""
::

    {
        "id": 1,
        "jsonrpc": "2.0",
        "result": [
        {
            "id": "1.2.1378",
            "uid": 250926091,
            "name": "yoyo250926091",
            "owner":
            {
                "weight_threshold": 1,
                "account_uid_auths": [],
                "key_auths": [
                    ["YYW7UoNSEQAUPvnvoBRVKyPAD9845esnpiK6MgHinsn5yqr5UgT5W", 1]
                ]
            },
            "active":
            {
                "weight_threshold": 1,
                "account_uid_auths": [],
                "key_auths": [
                    ["YYW6fU7Th8uESW9FZnpkhYaTUwtSvn3f2TQxFVA3ef2SSiwdZES71", 1]
                ]
            },
            "secondary":
            {
                "weight_threshold": 1,
                "account_uid_auths": [],
                "key_auths": [
                    ["YYW5eDSFYeiqyFRajfPP8tTZM7mfUeyc7H65zmnHtDW4SQJdwqTBD", 1]
                ]
            },
            "memo_key": "YYW7SpC4QLY1LRRxFQ2hbYHdAyQo88L8qnPJcDJkiRMugcnFGUGvo",
            "reg_info":
            {
                "registrar": 206336051,
                "referrer": 25997,
                "registrar_percent": 0,
                "referrer_percent": 0,
                "allowance_per_article":
                {
                    "amount": 0,
                    "asset_id": 0
                },
                "max_share_per_article":
                {
                    "amount": 0,
                    "asset_id": 0
                },
                "max_share_total":
                {
                    "amount": 0,
                    "asset_id": 0
                },
                "buyout_percent": 10000
            },
            "can_post": true,
            "can_reply": false,
            "can_rate": false,
            "is_full_member": true,
            "is_registrar": false,
            "is_admin": false,
            "create_time": "2018-04-03T08:21:00",
            "last_update_time": "2018-04-03T08:21:00",
            "active_data": "{}",
            "secondary_data": "{}",
            "statistics": "2.5.1378"
        }]
    }






1.1.3 get_account_balances
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
根据 uid和资产类型查询资产余额。
Query the asset balance based on uid and asset type.


Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC



Access authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""

:uid:   uid，for example:"250926091"
:assets:    a list of asset type id, with 0 representing core assets. For example: [0,1]. If the value is empty ([]), return all asset balances in the account

Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0,"get_account_balances",["250926091", [0,1]]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_account_balances", ["250926091", [0,1]]], "id": 1}' http://47.52.155.181:10011/rpc


Return results
""""""""""""""""
::

    {
        "id": 1,
        "jsonrpc": "2.0",
        "result": [
        {
            "amount": 1099970704,
            "asset_id": 0
        },
        {
            "amount": 0,
            "asset_id": 1
        }]
    }





1.1.4 get_post
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
根据平台所有者 uid 、发帖者 uid 、帖子 pid 返回帖子信息。
Return post information based on platform owner uid, poster uid, and post pid.

Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC



Access authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""

:platform_owner:   platform owner id
:poster_uid:   poster id
:post_pid:   post id (for exmaple：1)

Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0,"get_post",["223331844",223331844,0,1]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_post", [["250926091"]]], "id": 1}' http://47.52.155.181:10011/rpc


Return results
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": {
        "id": "1.7.14",
        "platform": 223331844,
        "poster": 223331844,
        "post_pid": 1,
        "hash_value": "asdfasdfasdfasdf",
        "extra_data": "{}",
        "title": "post a",
        "body": "post b",
        "create_time": "2018-05-03T12:40:39",
        "last_update_time": "2018-05-03T12:40:39"
      }
    }




1.1.5 get_posts_by_platform_poster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
根据平台所有者 uid 、 发帖者 uid 、发帖时间段 查询帖子列表。
Query the list of posts according to the platform owner uid, poster uid, post time period.

Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC


Access authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""

:platform_owner: platform owner id
:poster_uid: poster id; poster_uid can be null, query all users' posts at this time.
:create_time_range: The time limit is limited by two time points whose time orders are not limited. The query range is like this: the earliest time < Posting time <= latest time.

:limit: the number is limited, not exceeding 100.

Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0,"get_posts_by_platform_poster",[223331844, null, ["2018-04-03T12:42:36","2018-05-03T12:42:36"], 100]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_accounts_by_uid", [223331844, null, ["2018-04-03T12:42:36","2018-05-03T12:42:36"], 100]], "id": 1}' http://47.52.155.181:10011/rpc


Return results
""""""""""""""""

结果按时间排序，最新的排最前。时间相同的，按实际入块顺序，后入块的排在前面。
The results are sorted by time, with the latest one being the top. If the time is the same, the results are sorted by the actual order of receiving blocks, with the later block reception being in the front.
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": [
        {
          "id": "1.7.14",
          "platform": 223331844,
          "poster": 223331844,
          "post_pid": 1,
          "hash_value": "asdfasdfasdfasdf",
          "extra_data": "{}",
          "title": "post a",
          "body": "post b",
          "create_time": "2018-05-03T12:40:39",
          "last_update_time": "2018-05-03T12:40:39"
        }
      ]
    }




1.1.6 get_required_fee_data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
给定一组操作，返回操作需要的手续费信息。该 API 只支持核心资产。
Give a set of operations, return the fee information required for the operation. The API only supports core assets.

wherein，
::
    required_fee_data
    {
       account_uid_type fee_payer_uid; // payer uid
       int64_t          min_fee;       // 最低总费用，单位是核心资产去掉小数点后的值（与 asset 类型用法相同）；The lowest total cost and the unit is the value of core asset after being removed the part after the decimal point (same usage as the asset type)
       int64_t          min_real_fee;  // 最低真实费用（不能用币天抵扣的部分），单位同上 The lowest real cost (the part that cannot be deducted using tokens), and the unit is the same as above
    };


Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC



Access authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""

:ops:   uid array，the length is less than 1000; for example：["250926091"]

Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "get_required_fee_data", [[[0,{"fee":{"total":{"amount":200000,"asset_id":0},"options":{"from_balance":{"amount":200000,"asset_id":0}}},"from":236542328,"to":228984329,"amount":{"amount":100000,"asset_id":0},"extensions":{"from_balance":{"amount":100000,"asset_id":0},"to_balance":{"amount":100000,"asset_id":0}}}]]]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_required_fee_data", [[[0,{"fee":{"total":{"amount":200000,"asset_id":0},"options":{"from_balance":{"amount":200000,"asset_id":0}}},"from":236542328,"to":228984329,"amount":{"amount":100000,"asset_id":0},"extensions":{"from_balance":{"amount":100000,"asset_id":0},"to_balance":{"amount":100000,"asset_id":0}}}]]]], "id": 1}' http://47.52.155.181:10011/rpc

Return results
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": [
        {
          "fee_payer_uid": 236542328,
          "min_fee": 20000,
          "min_real_fee": 0
        }
      ]
    }






1.1.7 get_full_accounts_by_uid
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
根据一组账户 uid 获取对应信息。
Get the corresponding information based on a set of account uid.

Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC



Access authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""

:uids:   uid array，the length is less than 1000; for example：["250926091"]
:options:   options array 

Options The array can have the following parameters
::
    {
    optional fetch_account_object;
    optional fetch_statistics;
    optional fetch_csaf_leases_in;
    optional fetch_csaf_leases_out;
    optional fetch_voter_object;
    optional fetch_witness_object;
    optional fetch_witness_votes;
    optional fetch_committee_member_object;
    optional fetch_committee_member_votes;
    optional fetch_platform_object;
    optional fetch_platform_votes;
    optional fetch_assets;
    optional fetch_balances;
    }

Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "get_full_accounts_by_uid", [["250926091"],{}]]}

    {"id":1, "method":"call", "params":[0, "get_full_accounts_by_uid", [["223331844"],{"fetch_assets": true}]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_full_accounts_by_uid", [["250926091"],{}]], "id": 1}' http://47.52.155.181:10011/rpc


Return results
""""""""""""""""
::

    {
        "id": 1,
        "jsonrpc": "2.0",
        "result": [
            [250926091,
            {
                "account":
                {
                    "id": "0.0.0",
                    "uid": 0,
                    "name": "",
                    "owner":
                    {
                        "weight_threshold": 0,
                        "account_uid_auths": [],
                        "key_auths": []
                    },
                    "active":
                    {
                        "weight_threshold": 0,
                        "account_uid_auths": [],
                        "key_auths": []
                    },
                    "secondary":
                    {
                        "weight_threshold": 0,
                        "account_uid_auths": [],
                        "key_auths": []
                    },
                    "memo_key": "YYW1111111111111111111111111111111114T1Anm",
                    "reg_info":
                    {
                        "registrar": 1264,
                        "referrer": 1264,
                        "registrar_percent": 0,
                        "referrer_percent": 0,
                        "allowance_per_article":
                        {
                            "amount": 0,
                            "asset_id": 0
                        },
                        "max_share_per_article":
                        {
                            "amount": 0,
                            "asset_id": 0
                        },
                        "max_share_total":
                        {
                            "amount": 0,
                            "asset_id": 0
                        },
                        "buyout_percent": 10000
                    },
                    "can_post": true,
                    "can_reply": false,
                    "can_rate": false,
                    "is_full_member": false,
                    "is_registrar": false,
                    "is_admin": false,
                    "create_time": "1970-01-01T00:00:00",
                    "last_update_time": "1970-01-01T00:00:00",
                    "active_data": "{}",
                    "secondary_data": "{}",
                    "statistics": "2.5.0"
                },
                "statistics":
                {
                    "id": "0.0.0",
                    "owner": 31120496,
                    "total_ops": 0,
                    "removed_ops": 0,
                    "prepaid": 0,
                    "csaf": 0,
                    "core_balance": 0,
                    "core_leased_in": 0,
                    "core_leased_out": 0,
                    "average_coins": 0,
                    "average_coins_last_update": "1970-01-01T00:00:00",
                    "coin_seconds_earned": "0",
                    "coin_seconds_earned_last_update": "1970-01-01T00:00:00",
                    "total_witness_pledge": 0,
                    "releasing_witness_pledge": 0,
                    "witness_pledge_release_block_number": 4294967295,
                    "last_witness_sequence": 0,
                    "uncollected_witness_pay": 0,
                    "witness_last_confirmed_block_num": 0,
                    "witness_last_aslot": 0,
                    "witness_total_produced": 0,
                    "witness_total_missed": 0,
                    "witness_last_reported_block_num": 0,
                    "witness_total_reported": 0,
                    "total_committee_member_pledge": 0,
                    "releasing_committee_member_pledge": 0,
                    "committee_member_pledge_release_block_number": 4294967295,
                    "last_committee_member_sequence": 0,
                    "can_vote": true,
                    "is_voter": false,
                    "last_voter_sequence": 0,
                    "last_platform_sequence": 0,
                    "total_platform_pledge": 0,
                    "releasing_platform_pledge": 0,
                    "platform_pledge_release_block_number": 4294967295,
                    "last_post_sequence": 0
                },
                "csaf_leases_in": [],
                "csaf_leases_out": [],
                "witness_votes": [],
                "committee_member_votes": []
            }]
        ]
    }



Return field description
"""""""""""""""""""""""""""""""""""
return the structure definition of full_account in map as：

::

   full_account
   {
      account;                   // account basic info
      statistics;                // account dynamic info
      csaf_leases_in;            // fee token age borrowing details
      csaf_leases_out;           // fee token age lending details
      voter;                     // summary of account voting information
      witness;                   // witness information
      witness_votes;             // witness vote details (voting votes)
      committee_member;          // committee candidate info
      committee_member_votes;    // committee election voting details (voting votes)
      platform;                  // platform information owned by this account
      platform_votes;            // platform voting details (voting votes)
      assets;                    // this account is the asset issuer's asset type id list
      balances;                  // balance sheet

   };


1.1.8 get_witness_by_account
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
给定一个账户的 uid ，返回对应的见证人信息
Give the uid of an account, return the corresponding witness information

Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC



Access authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""

:account:   uid array，the length is less than 1000; for example：["250926091"]


Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0,"get_witness_by_account",["132826789"]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_witness_by_account", ["132826789"], "id": 1}' http://47.52.155.181:10011/rpc


Return results
""""""""""""""""
::

    {
        "id": 1,
        "jsonrpc": "2.0",
        "result":
        {
            "id": "1.5.31",
            "account": 132826789,
            "name": "yoyo132826789",
            "sequence": 1,
            "is_valid": true,
            "signing_key": "YYW1111111111111111111111111111111114T1Anm",
            "pledge": "7500000000",
            "pledge_last_update": "2017-09-05T11:39:03",
            "average_pledge": "7500000000",
            "average_pledge_last_update": "2017-09-06T12:05:36",
            "average_pledge_next_update_block": 4294967295,
            "total_votes": 719683655,
            "by_pledge_position": "0",
            "by_pledge_position_last_update": "0",
            "by_pledge_scheduled_time": "45370982250075664161773192435",
            "by_vote_position": "0",
            "by_vote_position_last_update": "0",
            "by_vote_scheduled_time": "472822140789228182032488184547",
            "last_confirmed_block_num": 8168,
            "last_aslot": 8599,
            "total_produced": 25,
            "total_missed": 0,
            "url": ""
        }
    }


Return field descriptions
"""""""""""""""""""""""""""""""""""
只有当 options 中对应选项为 true 时，返回结果中才包含对应字段数据。
其中，币龄借入明细、借出明细只返回前 100 条

如果 uid 不存在，则返回 map 中没有相应 uid 。

The corresponding field data is only included in the returned result if the corresponding option in options is true.
Among them, the token age borrowing details and lending details are only returned for the top 100 items.

If uid does not exist, there is no corresponding uid in the returned map.


1.1.9 get_witnesses
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
给定一组 uid ，返回对应的见证人信息
Give a set of uids, return the corresponding witness information

Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC



Access authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""

:account_uids:   uid array，for example：[132826789,25997]

Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "get_witnesses", [[132826789,25997]]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_witnesses", [[132826789,25997]]], "id": 1}' http://47.52.155.181:10011/rpc


Return results
""""""""""""""""
::

    {
        "id": 1,
        "jsonrpc": "2.0",
        "result": [
        {
            "id": "1.5.31",
            "account": 132826789,
            "name": "yoyo132826789",
            "sequence": 1,
            "is_valid": true,
            "signing_key": "YYW1111111111111111111111111111111114T1Anm",
            "pledge": "7500000000",
            "pledge_last_update": "2017-09-05T11:39:03",
            "average_pledge": "7500000000",
            "average_pledge_last_update": "2017-09-06T12:05:36",
            "average_pledge_next_update_block": 4294967295,
            "total_votes": 719683655,
            "by_pledge_position": "0",
            "by_pledge_position_last_update": "0",
            "by_pledge_scheduled_time": "45370982250075664161773192435",
            "by_vote_position": "0",
            "by_vote_position_last_update": "0",
            "by_vote_scheduled_time": "472822140789228182032488184547",
            "last_confirmed_block_num": 8168,
            "last_aslot": 8599,
            "total_produced": 25,
            "total_missed": 0,
            "url": ""
        },
        {
            "id": "1.5.1",
            "account": 25997,
            "name": "init1",
            "sequence": 1,
            "is_valid": true,
            "signing_key": "YYW71suPihtG7jJAGiVBCkd63ECHYebQaPa894oy3r54zk3eM1itt",
            "pledge": 1000000000,
            "pledge_last_update": "2017-09-12T21:02:45",
            "average_pledge": 1000000000,
            "average_pledge_last_update": "2017-09-13T21:20:30",
            "average_pledge_next_update_block": 4294967295,
            "total_votes": 0,
            "by_pledge_position": "0",
            "by_pledge_position_last_update": "0",
            "by_pledge_scheduled_time": "340282366580656096882718510549",
            "by_vote_position": "0",
            "by_vote_position_last_update": "0",
            "by_vote_scheduled_time": "340282366920938463463374607431768211455",
            "last_confirmed_block_num": 5937330,
            "last_aslot": 6308879,
            "total_produced": 513249,
            "total_missed": 32165,
            "url": ""
        }]
    }





1.1.10 lookup_witnesses
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
列出当前有效的见证人清单。
List current valid witnesses

Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC



Access authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""

:lower_bound_uid:  Start the query with this as the starting uid, set it to 0 and start from the beginning.
:limit:  Return quantity limit, up to 101
:ops:  Sort type; value range [0, 1, 2]. 
0:Sort by uid from big to small; 1: Sort by number of votes; 2: Sort by collateral amount

Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "lookup_witnesses", [0,2,1]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "lookup_witnesses", [0,2,1]], "id": 1}' http://47.52.155.181:10011/rpc


Return results
""""""""""""""""
::

    {
        "id": 1,
        "jsonrpc": "2.0",
        "result": [
        {
            "id": "1.5.31",
            "account": 132826789,
            "name": "yoyo132826789",
            "sequence": 1,
            "is_valid": true,
            "signing_key": "YYW1111111111111111111111111111111114T1Anm",
            "pledge": "7500000000",
            "pledge_last_update": "2017-09-05T11:39:03",
            "average_pledge": "7500000000",
            "average_pledge_last_update": "2017-09-06T12:05:36",
            "average_pledge_next_update_block": 4294967295,
            "total_votes": 701297305,
            "by_pledge_position": "0",
            "by_pledge_position_last_update": "0",
            "by_pledge_scheduled_time": "45370982250075664161773192435",
            "by_vote_position": "0",
            "by_vote_position_last_update": "0",
            "by_vote_scheduled_time": "485218414514968154552378399456",
            "last_confirmed_block_num": 8168,
            "last_aslot": 8599,
            "total_produced": 25,
            "total_missed": 0,
            "url": ""
        },
        {
            "id": "1.5.1",
            "account": 25997,
            "name": "init1",
            "sequence": 1,
            "is_valid": true,
            "signing_key": "YYW71suPihtG7jJAGiVBCkd63ECHYebQaPa894oy3r54zk3eM1itt",
            "pledge": 1000000000,
            "pledge_last_update": "2017-09-12T21:02:45",
            "average_pledge": 1000000000,
            "average_pledge_last_update": "2017-09-13T21:20:30",
            "average_pledge_next_update_block": 4294967295,
            "total_votes": 0,
            "by_pledge_position": "0",
            "by_pledge_position_last_update": "0",
            "by_pledge_scheduled_time": "340282366580656096882718510549",
            "by_vote_position": "0",
            "by_vote_position_last_update": "0",
            "by_vote_scheduled_time": "340282366920938463463374607431768211455",
            "last_confirmed_block_num": 5935462,
            "last_aslot": 6307011,
            "total_produced": 513079,
            "total_missed": 32165,
            "url": ""
        }]
    }



1.1.11 get_committee_member_by_account
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Give a uid, return the corresponding committee candidate information

Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC



Access authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""

:account:   uid; for example："250926091"


Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "get_committee_member_by_account", [25997]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_committee_member_by_account", [25997], "id": 1}' http://47.52.155.181:10011/rpc


Return results
""""""""""""""""
::

    {
        "id": 1,
        "jsonrpc": "2.0",
        "result":
        {
            "id": "1.4.0",
            "account": 25997,
            "name": "init1",
            "sequence": 1,
            "is_valid": true,
            "pledge": 0,
            "total_votes": 0,
            "url": ""
        }
    }





1.1.12 get_committee_members
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Get the corresponding information based on a set of account uid.

Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC



Access authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""

:committee_member_uids:   uid array; for example：[25997,26264] 

Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "get_committee_members", [[25997,26264]]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_committee_members", [[25997,26264]]], "id": 1}' http://47.52.155.181:10011/rpc


Return results
""""""""""""""""
::

    {
        "id": 1,
        "jsonrpc": "2.0",
        "result": [
        {
            "id": "1.4.0",
            "account": 25997,
            "name": "init1",
            "sequence": 1,
            "is_valid": true,
            "pledge": 0,
            "total_votes": 0,
            "url": ""
        },
        {
            "id": "1.4.1",
            "account": 26264,
            "name": "init2",
            "sequence": 1,
            "is_valid": true,
            "pledge": 0,
            "total_votes": 0,
            "url": ""
        }]
    }





1.1.13 lookup_committee_members
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
列出当前有效的候选理事清单
List the current valid committee candidate list

Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC



Access authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""

:lower_bound_uid:   Start the query with this as the starting uid, set it to 0 and start from the beginning
:limit:  Return quantity limit, up to 101
:ops:   Sort type, value range [0,1,2] 
0:Sort by uid from big to small; 1: Sort by number of votes; 2: Sort by collateral amount

Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "lookup_committee_members", [0,2,1]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "lookup_committee_members", [0,2,1]], "id": 1}'


Return results
""""""""""""""""
::

    {
        "id": 1,
        "jsonrpc": "2.0",
        "result": [
        {
            "id": "1.4.0",
            "account": 25997,
            "name": "init1",
            "sequence": 1,
            "is_valid": true,
            "pledge": 0,
            "total_votes": 0,
            "url": ""
        },
        {
            "id": "1.4.1",
            "account": 26264,
            "name": "init2",
            "sequence": 1,
            "is_valid": true,
            "pledge": 0,
            "total_votes": 0,
            "url": ""
        }]
    }





1.1.14 list_committee_proposals
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
列出所有尚未成功执行的理事会提案，包含正在投票表决的、已表决通过但还没到执行时间的。
List all the committee proposals that have not been successfully implemented, including those that are being voted on, have been voted through but have not yet reached the execution time.

Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC



Access authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""
null

Precautions
""""""""""""""""
无

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "list_committee_proposals", []]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "list_committee_proposals", []], "id": 1}' http://47.52.155.181:10011/rpc


Return results
""""""""""""""""
::

    {
        "id": 1,
        "jsonrpc": "2.0",
        "result": []
    }





1.1.15 lookup_accounts_by_name
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
根据名称查找账号UID。
Find the account UID by name.
普通账户名称目前为yoyo+uid
The normal account name is currently yoyo+uid

Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC



Access authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""
:lower_bound_name:   Start the query with this as the starting name, set it to an empty string and start from the beginning.
:limit:  Return quantity limit, up to 1001

Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "lookup_accounts_by_name", ["",2]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "lookup_accounts_by_name", ["",2]], "id": 1}' http://47.52.155.181:10011/rpc

Return results
""""""""""""""""
::

    {
        "id": 1,
        "jsonrpc": "2.0",
        "result": [
            ["abit", 209414065],
            ["agaoye", 209415129]
        ]
    }




1.1.16 get_platform_by_account
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
给定一个 uid ，返回对应的账户拥有的平台信息
Give a uid, return the platform information owned by the corresponding account

Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC



Access and authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""

:account:  one account uid

Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "get_platform_by_account", [224006453]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_platform_by_account", [224006453]], "id": 1}' http://47.52.155.181:10011/rpc


Return results
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": {
        "id": "1.6.4",
        "owner": 224006453,
        "name": "dwgMarket",
        "sequence": 1,
        "is_valid": true,
        "total_votes": 0,
        "url": "www.cad1688.com",
        "pledge": 1000000000,
        "pledge_last_update": "2018-04-04T08:38:24",
        "average_pledge": 0,
        "average_pledge_last_update": "2018-04-04T08:38:24",
        "average_pledge_next_update_block": 5712088,
        "extra_data": "{}",
        "create_time": "2018-04-04T08:38:24",
        "last_update_time": "1970-01-01T00:00:00"
      }
    }



1.1.17 get_platforms
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Give a set of uids, return the corresponding platform information; uid is the owner id of the platform

Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC



Access authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""

:account_uids:   uid list [224006453,217895094]

Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "get_platforms", [[224006453,217895094]]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_platforms", [[224006453,217895094]]], "id": 1}' http://47.52.155.181:10011/rpc


Return results
""""""""""""""""
::

    {
        "id": 1,
        "jsonrpc": "2.0",
        "result": [
        {
            "id": "1.6.4",
            "owner": 224006453,
            "name": "dwgMarket",
            "sequence": 1,
            "is_valid": true,
            "total_votes": 0,
            "url": "www.cad1688.com",
            "pledge": 1000000000,
            "pledge_last_update": "2018-04-04T08:38:24",
            "average_pledge": 0,
            "average_pledge_last_update": "2018-04-04T08:38:24",
            "average_pledge_next_update_block": 5712088,
            "extra_data": "{}",
            "create_time": "2018-04-04T08:38:24",
            "last_update_time": "1970-01-01T00:00:00"
        },
        {
            "id": "1.6.0",
            "owner": 217895094,
            "name": "test-yoyow",
            "sequence": 1,
            "is_valid": true,
            "total_votes": 0,
            "url": "http://demo.yoyow.org/",
            "pledge": 1000000000,
            "pledge_last_update": "2018-02-10T01:03:57",
            "average_pledge": 176601774,
            "average_pledge_last_update": "2018-02-11T06:49:12",
            "average_pledge_next_update_block": 4562164,
            "extra_data": "{\"login\":\"http://192.168.1.184:8280/login\"}",
            "create_time": "2018-02-10T01:03:57",
            "last_update_time": "2018-02-11T06:49:12"
        }]
    }




1.1.18 lookup_platforms
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
按平台拥有者进行查询，列出当前有效的平台清单。
Query by platform owner to list the current valid platforms

Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC



Access authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""

:lower_bound_uid: Start the query with this as the starting uid, set it to 0 and start from the beginning.
:limit:  Return quantity limit, up to 101
:ops:   Sort type, value range [0,1,2] 
0: Sort by uid from big to small; 1: Sort by number of votes; 2: Sort by collateral amount

Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "lookup_platforms", [0,2,1]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "lookup_platforms", [0,2,1]], "id": 1}' http://47.52.155.181:10011/rpc


Return results
""""""""""""""""
::

    {
        "id": 1,
        "jsonrpc": "2.0",
        "result": [
        {
            "id": "1.6.0",
            "owner": 217895094,
            "name": "test-yoyow",
            "sequence": 1,
            "is_valid": true,
            "total_votes": 0,
            "url": "http://demo.yoyow.org/",
            "pledge": 1000000000,
            "pledge_last_update": "2018-02-10T01:03:57",
            "average_pledge": 176601774,
            "average_pledge_last_update": "2018-02-11T06:49:12",
            "average_pledge_next_update_block": 4562164,
            "extra_data": "{\"login\":\"http://192.168.1.184:8280/login\"}",
            "create_time": "2018-02-10T01:03:57",
            "last_update_time": "2018-02-11T06:49:12"
        },
        {
            "id": "1.6.4",
            "owner": 224006453,
            "name": "dwgMarket",
            "sequence": 1,
            "is_valid": true,
            "total_votes": 0,
            "url": "www.cad1688.com",
            "pledge": 1000000000,
            "pledge_last_update": "2018-04-04T08:38:24",
            "average_pledge": 0,
            "average_pledge_last_update": "2018-04-04T08:38:24",
            "average_pledge_next_update_block": 5712088,
            "extra_data": "{}",
            "create_time": "2018-04-04T08:38:24",
            "last_update_time": "1970-01-01T00:00:00"
        }]
    }





1.1.19 get_platform_count
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
返回平台总数量
Return the total number of platforms

Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC



Access authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""
null

Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "get_platform_count", []]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_platform_count", []], "id": 1}' http://47.52.155.181:10011/rpc


Return results
""""""""""""""""
::

    {
        "id": 1,
        "jsonrpc": "2.0",
        "result": 5
    }





1.1.20 get_assets
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
给定一组资产 id ，返回对应的资产的详细信息。
Give a set of asset ids, return the details of the corresponding assets.

Parameters：
asset_ids a set of assets id

Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC



Access authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""

:asset_ids:   asset id array; for the time being, only the core asset YOYO is accepted，for example： [0]


Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "get_assets", [[0]]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_assets", [[0]]], "id": 1}' http://47.52.155.181:10011/rpc


Return results
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": [
        {
          "id": "1.3.0",
          "asset_id": 0,
          "symbol": "YOYO",
          "precision": 5,
          "issuer": 1264,
          "options": {
            "max_supply": "200000000000000",
            "market_fee_percent": 0,
            "max_market_fee": "1000000000000000",
            "issuer_permissions": 0,
            "flags": 0,
            "whitelist_authorities": [],
            "blacklist_authorities": [],
            "whitelist_markets": [],
            "blacklist_markets": [],
            "description": ""
          },
          "dynamic_asset_data_id": "2.2.0",
          "dynamic_asset_data": {
            "id": "2.2.0",
            "asset_id": 0,
            "current_supply": "106899730634997",
            "accumulated_fees": 0
          }
        }
      ]
    }

    返回结果中的 dynamic_asset_data 字段包括资产动态数据明细。



1.1.21 list_assets
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
分页查询资产详细信息。返回结果按资产代码的 ASCII 码顺序排序。
Query asset details by page. The returned results are sorted in ASCII code order of the asset code.

Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC



Access authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""

:lower_bound_symbol:   Start with this as the starting code
:limit:   Return quantity limit, up to 101

Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "list_assets", ["YOY",4]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "list_assets", ["YOY",4]], "id": 1}' http://47.52.155.181:10011/rpc


Return results
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": [
        {
          "id": "1.3.91",
          "asset_id": 91,
          "symbol": "YOYES",
          "precision": 2,
          "issuer": 215074501,
          "options": {
            "max_supply": 1200,
            "market_fee_percent": 0,
            "max_market_fee": 1200,
            "issuer_permissions": 79,
            "flags": 0,
            "whitelist_authorities": [],
            "blacklist_authorities": [],
            "whitelist_markets": [],
            "blacklist_markets": [],
            "description": ""
          },
          "dynamic_asset_data_id": "2.2.91",
          "dynamic_asset_data": {
            "id": "2.2.91",
            "asset_id": 91,
            "current_supply": 0,
            "accumulated_fees": 0
          }
        },
        {
          "id": "1.3.130",
          "asset_id": 130,
          "symbol": "YOYIO",
          "precision": 2,
          "issuer": 254208024,
          "options": {
            "max_supply": 1258000000,
            "market_fee_percent": 0,
            "max_market_fee": 1258000000,
            "issuer_permissions": 79,
            "flags": 0,
            "whitelist_authorities": [],
            "blacklist_authorities": [],
            "whitelist_markets": [],
            "blacklist_markets": [],
            "description": "环保节能"
          },
          "dynamic_asset_data_id": "2.2.130",
          "dynamic_asset_data": {
            "id": "2.2.130",
            "asset_id": 130,
            "current_supply": 1258000000,
            "accumulated_fees": 0
          }
        },
        {
          "id": "1.3.0",
          "asset_id": 0,
          "symbol": "YOYO",
          "precision": 5,
          "issuer": 1264,
          "options": {
            "max_supply": "200000000000000",
            "market_fee_percent": 0,
            "max_market_fee": "1000000000000000",
            "issuer_permissions": 0,
            "flags": 0,
            "whitelist_authorities": [],
            "blacklist_authorities": [],
            "whitelist_markets": [],
            "blacklist_markets": [],
            "description": ""
          },
          "dynamic_asset_data_id": "2.2.0",
          "dynamic_asset_data": {
            "id": "2.2.0",
            "asset_id": 0,
            "current_supply": "106899950291573",
            "accumulated_fees": 0
          }
        },
        {
          "id": "1.3.2",
          "asset_id": 2,
          "symbol": "YOYOW",
          "precision": 5,
          "issuer": 25638,
          "options": {
            "max_supply": "1000000000000",
            "market_fee_percent": 0,
            "max_market_fee": "1000000000000",
            "issuer_permissions": 79,
            "flags": 0,
            "whitelist_authorities": [],
            "blacklist_authorities": [],
            "whitelist_markets": [],
            "blacklist_markets": [],
            "description": ""
          },
          "dynamic_asset_data_id": "2.2.2",
          "dynamic_asset_data": {
            "id": "2.2.2",
            "asset_id": 2,
            "current_supply": 0,
            "accumulated_fees": 0
          }
        }
      ]
    }




1.1.22 lookup_asset_symbols
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
给定一组资产代码或 id ，返回对应的资产的详细信息。
Give a set of asset codes or ids, return the details of the corresponding assets.

Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC



Access authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""

:symbols_or_ids:   Array form, the symbol code or ID of the asset to be retrieved, for example: ["YOYO"] or [0]

Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "lookup_asset_symbols", [[0]]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "lookup_asset_symbols", [[0]]], "id": 1}' http://47.52.155.181:10011/rpc
    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "lookup_asset_symbols", [["YOYO"]]], "id": 1}' http://47.52.155.181:10011/rpc


Return results
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": [
        {
          "id": "1.3.0",
          "asset_id": 0,
          "symbol": "YOYO",
          "precision": 5,
          "issuer": 1264,
          "options": {
            "max_supply": "200000000000000",
            "market_fee_percent": 0,
            "max_market_fee": "1000000000000000",
            "issuer_permissions": 0,
            "flags": 0,
            "whitelist_authorities": [],
            "blacklist_authorities": [],
            "whitelist_markets": [],
            "blacklist_markets": [],
            "description": ""
          },
          "dynamic_asset_data_id": "2.2.0",
          "dynamic_asset_data": {
            "id": "2.2.0",
            "asset_id": 0,
            "current_supply": "106900048605605",
            "accumulated_fees": 0
          }
        }
      ]
    }

History API
----------------

1.2.1 get_relative_account_history
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

获取账户历史。
Get account history

Supported format
""""""""""""""""
JSON 

Request method
""""""""""""""""
WebSocket; JSON-RPC


Access authorization limit
""""""""""""""""""
null


Request parameters
""""""""""""""""

:account:   can be uid or account nickname
:op_type:   the type of limited operation, see the type of operation. When the value is null, all operation types are returned; when 0, all transfer operations are available.
:start:   Query start number（sequence number）
:limit:   Return the total number of results
:end:  When the value is 0, the most recent history of operations can be obtained.


返回结果的数量会在end - start 范围之内；如果limit值比end - start 要小，则返回满足limit条件的最新操作记录。
返回结果的排序方式为： 最新的优先

The number of returned results will be in the end - start range; if the limit value is smaller than end - start, the latest operation record that satisfies the limit condition is returned.
The returned results are sorted in the way that the latest ones are returned first.

Precautions
""""""""""""""""
null

Call sample and debug tools
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[2, "get_relative_account_history", [223331844, null, 1,3,10]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_relative_account_history", [223331844, null, 1,3,10]], "id": 1}' http://47.52.155.181:10011/rpc


Return results
""""""""""""""""
返回列表中每条数据是 pair 类型，pair 中第一个元素为该条记录在该账号历史中的序列号（sequence），第二个元素为具体操作

Each piece of data in the returned list is a pair type. The first element in the pair is the sequence number recorded in the account history, and the second element is the specific operation.

::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": [
        [
          10,
          {
            "id": "1.12.48157",
            "op": [
              0,
              {
                "fee": {
                  "total": {
                    "amount": 20000,
                    "asset_id": 0
                  },
                  "options": {
                    "from_csaf": {
                      "amount": 20000,
                      "asset_id": 0
                    }
                  }
                },
                "from": 217895094,
                "to": 223331844,
                "amount": {
                  "amount": "200000000000",
                  "asset_id": 0
                },
                "extensions": {
                  "from_balance": {
                    "amount": "200000000000",
                    "asset_id": 0
                  },
                  "to_balance": {
                    "amount": "200000000000",
                    "asset_id": 0
                  }
                }
              }
            ],
            "result": [
              0,
              {}
            ],
            "block_timestamp": "2018-05-02T09:24:30",
            "block_num": 6515279,
            "trx_in_block": 0,
            "op_in_trx": 0,
            "virtual_op": 2715
          }
        ],
        [
          9,
          {
            "id": "1.12.47189",
            "op": [
              22,
              {
                "fee": {
                  "total": {
                    "amount": 200000,
                    "asset_id": 0
                  },
                  "options": {
                    "from_csaf": {
                      "amount": 200000,
                      "asset_id": 0
                    }
                  }
                },
                "voter": 236542328,
                "platform_to_add": [
                  223331844
                ],
                "platform_to_remove": []
              }
            ],
            "result": [
              0,
              {}
            ],
            "block_timestamp": "2018-04-16T08:14:57",
            "block_num": 6053313,
            "trx_in_block": 0,
            "op_in_trx": 0,
            "virtual_op": 1157
          }
        ],
        [
          8,
          {
            "id": "1.12.47149",
            "op": [
              22,
              {
                "fee": {
                  "total": {
                    "amount": 200000,
                    "asset_id": 0
                  },
                  "options": {
                    "from_csaf": {
                      "amount": 200000,
                      "asset_id": 0
                    }
                  }
                },
                "voter": 250926091,
                "platform_to_add": [
                  223331844
                ],
                "platform_to_remove": []
              }
            ],
            "result": [
              0,
              {}
            ],
            "block_timestamp": "2018-04-16T03:19:36",
            "block_num": 6049807,
            "trx_in_block": 0,
            "op_in_trx": 0,
            "virtual_op": 1117
          }
        ]
      ]
    }
