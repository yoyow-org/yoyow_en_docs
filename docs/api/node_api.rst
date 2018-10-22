
节点API说明
=============
.. contents:: :depth: 3

节点连接方式
-------------

测试环境：
:: 
  websocket 接口地址： ws://47.52.155.181:10011
  jsonrpc 接口地址： http://47.52.155.181:10011/rpc

正式环境：
::
  websocket 接口地址：ws://139.198.1.234:9000
  jsonrpc 接口地址： http://139.198.1.234:9000/rpc


使用wscat连接， `wscat安装方法 <https://www.npmjs.com/package/wscat>`_  
  wscat -c ws://47.52.155.181:10011


使用curl post data 连接
  curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_accounts_by_uid", [["250926091"]]], "id": 1}' http://47.52.155.181:10011/rpc

API分类
----------------
YOYOW的API分类与BTS类似，以下重点介绍database api和history api。
在通过websocket 请求时，参数为一个json字符串，格式如下：

    {"id":1, "method":"call", "params":[API等级,"函数名",[具体参数]]}

在使用时，需要填写API等级，函数名，和具体参数3项，其中API等级可以通过websocket 发送

    {"id":2, "method":"call", "params":[1,"history",[]]}

来获取。比如以上请求会返回：
::
    {
      "id": 2,
      "jsonrpc": "2.0",
      "result": 2
    }

其中result 为 2，代表着使用history api 时，API等级需要填写2.（注意：不一定每个YOYOW节点都返回同样的配置，这取决于每个节点对暴露API的限制）

database的API默认可以直接通过指定API等级为0来调用，也可以使用通过

    {"id":2, "method":"call", "params":[1,"database",[]]}

查询到的result的值来调用。


database API
----------------

1.1.1 get_required_signatures
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
根据给定的交易（可能已包含签名），和给定的备用公钥集合，返回与签署该交易有关的 3 个集合：
::
 备用公钥集合的一个可用子集，可以用来签署该交易
 可能还需要的公钥（不在签名中，也不在备用公钥集合中）
 交易中已包含的多余签名

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC


访问授权限制
""""""""""""""""""

无


请求参数
""""""""""""""""

:trx:             交易，可能已包含签名
:available_keys:  公钥的数组 例如：["YYW5eDSFYeiqyFRajfPP8tTZMm7fUeyc7H65zmnHtDW4SQJdwqTBD"]

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0,"get_required_signatures",[{"operations":[[0,{"fee":{"total":{"amount":100000,"asset_id":0}},"from":250926091,"to":223331844,"amount":{"amount":100000,"asset_id":0},"extensions":{}}]]}, ["YYW5eDSFYeiqyFRajfPP8tTZM7mfUeyc7H65zmnHtDW4SQJdwqTBD"]]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_accounts_by_uid", [{"operations":[[0,{"fee":{"total":{"amount":100000,"asset_id":0}},"from":250926091,"to":223331844,"amount":{"amount":100000,"asset_id":0},"extensions":{}}]]}, ["YYW5eDSFYeiqyFRajfPP8tTZM7mfUeyc7H65zmnHtDW4SQJdwqTBD"]]], "id": 1}' http://47.52.155.181:10011/rpc

返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": [
        [
          [
            "YYW5eDSFYeiqyFRajfPP8tTZM7mfUeyc7H65zmnHtDW4SQJdwqTBD"
          ],  //备用公钥集合的一个可用子集，可以用来签署该交易
          [
            "YYW6fU7Th8uESW9FZnpkhYaTUwtSvn3f2TQxFVA3ef2SSiwdZES71",
            "YYW7UoNSEQAUPvnvoBRVKyPAD9845esnpiK6MgHinsn5yqr5UgT5W"
          ] //还需要的公钥（不在签名中，也不在备用公钥集合中）
        ],
        []  // 交易中已包含的多余签名
      ]
    }

1.1.2 get_accounts_by_uid
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
根据 uid 返回多个账号信息。数量必须 <= 1000。

如果该 uid 不存在，对应位置结果为 null 。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC


访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""

:account_uids:   uid数组，长度小于1000 例如：["250926091"]

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0,"get_accounts_by_uid",[["250926091"]]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_accounts_by_uid", [["250926091"]]], "id": 1}' http://47.52.155.181:10011/rpc


返回结果
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


支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC



访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""

:uid:   uid，例如:"250926091"
:assets:    资产种类id的列表,0代表核心资产。例如：[0,1]。如果该值为空([]) 则返回该账户里的所有资产余额

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0,"get_account_balances",["250926091", [0,1]]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_account_balances", ["250926091", [0,1]]], "id": 1}' http://47.52.155.181:10011/rpc


返回结果
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

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC



访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""

:platform_owner:   平台owner的id
:poster_uid:   poster的id
:post_pid:   post的id 例如：1

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0,"get_post",["223331844",223331844,0,1]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_post", [["250926091"]]], "id": 1}' http://47.52.155.181:10011/rpc


返回结果
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

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC


访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""

:platform_owner:   平台owner的id
:poster_uid:   poster的id。poster_uid可以为 null ，此时查询所有用户的帖子。
:create_time_range:   限制时间段，由两个时间点组成，先后不限，查询范围为 最早时间 < 发帖时间 <= 最晚时间
:limit:   限制条数，不能超过 100

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0,"get_posts_by_platform_poster",[223331844, null, ["2018-04-03T12:42:36","2018-05-03T12:42:36"], 100]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_accounts_by_uid", [223331844, null, ["2018-04-03T12:42:36","2018-05-03T12:42:36"], 100]], "id": 1}' http://47.52.155.181:10011/rpc


返回结果
""""""""""""""""

结果按时间排序，最新的排最前。时间相同的，按实际入块顺序，后入块的排在前面。
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

其中，
::
    required_fee_data
    {
       account_uid_type fee_payer_uid; // 付费人 uid
       int64_t          min_fee;       // 最低总费用，单位是核心资产去掉小数点后的值（与 asset 类型用法相同）；
       int64_t          min_real_fee;  // 最低真实费用（不能用币天抵扣的部分），单位同上
    };


支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC



访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""

:ops:   uid数组，长度小于1000 例如：["250926091"]

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "get_required_fee_data", [[[0,{"fee":{"total":{"amount":200000,"asset_id":0},"options":{"from_balance":{"amount":200000,"asset_id":0}}},"from":236542328,"to":228984329,"amount":{"amount":100000,"asset_id":0},"extensions":{"from_balance":{"amount":100000,"asset_id":0},"to_balance":{"amount":100000,"asset_id":0}}}]]]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_required_fee_data", [[[0,{"fee":{"total":{"amount":200000,"asset_id":0},"options":{"from_balance":{"amount":200000,"asset_id":0}}},"from":236542328,"to":228984329,"amount":{"amount":100000,"asset_id":0},"extensions":{"from_balance":{"amount":100000,"asset_id":0},"to_balance":{"amount":100000,"asset_id":0}}}]]]], "id": 1}' http://47.52.155.181:10011/rpc

返回结果
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

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC



访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""

:uids:   uid数组，长度小于1000 例如：["250926091"]
:options:   options 数组 

Options 数组可以有如下参数
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

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "get_full_accounts_by_uid", [["250926091"],{}]]}

    {"id":1, "method":"call", "params":[0, "get_full_accounts_by_uid", [["223331844"],{"fetch_assets": true}]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_full_accounts_by_uid", [["250926091"],{}]], "id": 1}' http://47.52.155.181:10011/rpc


返回结果
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



返回字段说明
"""""""""""""""""""""""""""""""""""
返回 map 中 full_account 的结构定义为：

::

   full_account
   {
      account;                   // 账户基本信息
      statistics;                // 账户动态信息
      csaf_leases_in;            // 手续费币龄借入明细
      csaf_leases_out;           // 手续费币龄借出明细
      voter;                     // 账户投票信息汇总
      witness;                   // 见证人信息
      witness_votes;             // 见证人投票明细（投出票）
      committee_member;          // 候选理事信息
      committee_member_votes;    // 理事会选举投票明细（投出票）
      platform;                  // 该账户拥有的平台信息
      platform_votes;            // 平台投票明细（投出票）
      assets;                    // 该账户为资产发行人的资产类型 id 清单
      balances;                  // 余额表

   };


1.1.8 get_witness_by_account
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
给定一个账户的 uid ，返回对应的见证人信息

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC



访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""

:account:   uid数组，长度小于1000 例如：["250926091"]


注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0,"get_witness_by_account",["132826789"]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_witness_by_account", ["132826789"], "id": 1}' http://47.52.155.181:10011/rpc


返回结果
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


返回字段说明
"""""""""""""""""""""""""""""""""""
只有当 options 中对应选项为 true 时，返回结果中才包含对应字段数据。
其中，币龄借入明细、借出明细只返回前 100 条

如果 uid 不存在，则返回 map 中没有相应 uid 。


1.1.9 get_witnesses
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
给定一组 uid ，返回对应的见证人信息

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC



访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""

:account_uids:   uid数组，例如：[132826789,25997]

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "get_witnesses", [[132826789,25997]]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_witnesses", [[132826789,25997]]], "id": 1}' http://47.52.155.181:10011/rpc


返回结果
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

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC



访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""

:lower_bound_uid:  以此作为起始 uid 开始查询，设为 0 则从头开始查
:limit:  返回数量限制，最多不能超过 101
:ops:  排序类型; 取值范围[0,1,2]。 0:按uid由大到小排序；1:按得票数从多到少排序；2:按抵押从多到少排序

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "lookup_witnesses", [0,2,1]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "lookup_witnesses", [0,2,1]], "id": 1}' http://47.52.155.181:10011/rpc


返回结果
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
给定一个 uid ，返回对应的候选理事信息

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC



访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""

:account:   uid 例如："250926091"


注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "get_committee_member_by_account", [25997]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_committee_member_by_account", [25997], "id": 1}' http://47.52.155.181:10011/rpc


返回结果
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
根据一组账户 uid 获取对应信息。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC



访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""

:committee_member_uids:   uid数组 例如：[25997,26264] 

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "get_committee_members", [[25997,26264]]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_committee_members", [[25997,26264]]], "id": 1}' http://47.52.155.181:10011/rpc


返回结果
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

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC



访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""

:lower_bound_uid:   以此作为起始 uid 开始查询，设为 0 则从头开始查
:limit:  返回数量限制，最多不能超过 101
:ops:   排序类型取值范围[0,1,2]。 0:按uid由大到小排序；1:按得票数从多到少排序；2:按抵押从多到少排序

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "lookup_committee_members", [0,2,1]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "lookup_committee_members", [0,2,1]], "id": 1}'


返回结果
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

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC



访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""
无

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "list_committee_proposals", []]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "list_committee_proposals", []], "id": 1}' http://47.52.155.181:10011/rpc


返回结果
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
普通账户名称目前为yoyo+uid

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC



访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""
:lower_bound_name:   以此作为起始名称开始查询，设为空串则从头开始查
:limit:  返回数量限制，最多不能超过 1001

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "lookup_accounts_by_name", ["",2]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "lookup_accounts_by_name", ["",2]], "id": 1}' http://47.52.155.181:10011/rpc

返回结果
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

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC



访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""

:account:  一个账户 uid

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "get_platform_by_account", [224006453]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_platform_by_account", [224006453]], "id": 1}' http://47.52.155.181:10011/rpc


返回结果
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
给定一组 uid ，返回对应的平台信息，uid为平台的所有者id

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC



访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""

:account_uids:   uid 列表 [224006453,217895094]

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "get_platforms", [[224006453,217895094]]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_platforms", [[224006453,217895094]]], "id": 1}' http://47.52.155.181:10011/rpc


返回结果
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


支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC



访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""

:lower_bound_uid:   以此作为起始 uid 开始查询，设为 0 则从头开始查
:limit:  返回数量限制，最多不能超过 101
:ops:   排序类型取值范围[0,1,2]。 0:按uid由大到小排序；1:按得票数从多到少排序；2:按抵押从多到少排序

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "lookup_platforms", [0,2,1]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "lookup_platforms", [0,2,1]], "id": 1}' http://47.52.155.181:10011/rpc


返回结果
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

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC



访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""
无

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "get_platform_count", []]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_platform_count", []], "id": 1}' http://47.52.155.181:10011/rpc


返回结果
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


参数：
asset_ids 一组资产 id

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC



访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""

:asset_ids:   资产id数组，暂时只要核心资产YOYO，例如： [0]


注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "get_assets", [[0]]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_assets", [[0]]], "id": 1}' http://47.52.155.181:10011/rpc


返回结果
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

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC



访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""

:lower_bound_symbol:   以此作为起始代码开始查询
:limit:   返回数量限制，最多不能超过 101

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "list_assets", ["YOY",4]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "list_assets", ["YOY",4]], "id": 1}' http://47.52.155.181:10011/rpc


返回结果
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

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC



访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""

:symbols_or_ids:   数组形式，要检索的资产的符号代码或ID，例如：["YOYO"] 或者 [0]

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[0, "lookup_asset_symbols", [[0]]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "lookup_asset_symbols", [[0]]], "id": 1}' http://47.52.155.181:10011/rpc
    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "lookup_asset_symbols", [["YOYO"]]], "id": 1}' http://47.52.155.181:10011/rpc


返回结果
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


支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC


访问授权限制
""""""""""""""""""
无


请求参数
""""""""""""""""

:account:   可以是 uid 或者账户昵称
:op_type:   限制操作类型，参见操作类型。值为 null 时，则返回所有操作类型；为 0 时可获得所有transfer操作.
:start:   查询起始编号（sequence number）
:limit:   返回结果总数
:end:  值为 0 时，可得到最多的最近操作记录.


返回结果的数量会在end - start 范围之内；如果limit值比end - start 要小，则返回满足limit条件的最新操作记录。
返回结果的排序方式为： 最新的优先

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://47.52.155.181:10011
    {"id":1, "method":"call", "params":[2, "get_relative_account_history", [223331844, null, 1,3,10]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_relative_account_history", [223331844, null, 1,3,10]], "id": 1}' http://47.52.155.181:10011/rpc


返回结果
""""""""""""""""
返回列表中每条数据是 pair 类型，pair 中第一个元素为该条记录在该账号历史中的序列号（sequence），第二个元素为具体操作

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
