
钱包 API说明
=============

.. contents:: :depth: 3

钱包连接方式
-------------

下载方式：
^^^^^^^^^^^^^^

`测试网络钱包地址 <https://github.com/yoyow-org/yoyow-core-testnet/releases>`_

`正式网络钱包地址 <https://github.com/yoyow-org/yoyow-core/releases>`_

启动方式：
^^^^^^^^^^^^^^^

wallet的使用:

可以通过交互的命令行，也可以通过websocket和http的接口。
参考 `交易指南 <https://github.com/yoyow-org/yoyow-core/wiki/%E4%BA%A4%E6%98%93%E6%89%80%E5%AF%B9%E6%8E%A5%E6%8C%87%E5%8D%97%EF%BC%88%E4%B8%AD%E6%96%87%EF%BC%89#%E5%90%AF%E5%8A%A8-yoyow-client>`_
::

    ./yoyow_client -s ws://127.0.0.1:8090/ -r 0.0.0.0:8091 -H 127.0.0.1:8093

    注意:

    使用 -s 来指定，连接到的节点程序的IP与端口；
    使用 -r 选项来开启一个websocket接口；
    使用 -H 选项来开启一个HTTP-RPC服务，方便我们其他程序进行访问。如：单独处理充值/提现的脚本程序。
    yoyow_node 只有完成同步后，才会监听RPC端口，所以请耐心等待 yoyow_node 同步完成。
    您可以启动多个client 连接同一个yoyow_node。但请注意不要使用相同的-H，会因为端口被占用而监听失败。


在本文档的测试用例中，均使用本机地址。

websocket 接口地址： ws://localhost:8091

http rpc接口地址： http://localhost:8093

**连接方法如下：**


1. 使用wscat连接 websocket 接口: 
::
    wscat -c ws://localhost:8091


2. 使用curl 连接 websocket 接口:
::
    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_accounts_by_uid", [["250926091"]]], "id": 1}' http://localhost:8091

3. 使用curl 连接 http 接口:
::
    curl --data '{"method": "call", "params": [0, "get_accounts_by_uid", [["250926091"]]], "id": 1}' http://localhost:8093

备注：websocket和http接口的区别：websocket接口同样可以使用curl获取数据，会遵循jsonrpc格式，请求和返回的json数据均需携带{"jsonrpc": "2.0"}。http 的接口不需携带{"jsonrpc": "2.0"}的标签。


2.1 工具类 API
----------------

2.1.1 calculate_account_uid
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
给定一个数，计算出对应的账户 uid

支持格式
""""""""""""""""""

JSON 

请求方式
""""""""""""""""""

WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
无

访问授权限制
""""""""""""""""""

| 访问级别: 普通接口
| 频次限制: 是

请求参数
""""""""""""""""

:n:  数字


注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {{"id":1, "method":"call", "params":[0,"calculate_account_uid",[12]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0,"calculate_account_uid",[12]], "id": 1}' http://localhost:8093


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": 3106
    }

2.1.2 suggest_brain_key
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
随机生成一个脑密钥，根据脑密钥得出一对公私钥

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
无

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


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

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0,"suggest_brain_key",[]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0,"suggest_brain_key",[]], "id": 1}' http://localhost:8091


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": {
        "brain_priv_key": "DERIC GIANTRY ALODY TELAR TIRR BOILER BULLIT REACTOR BANISH FLOCCUS SLIPPER PELANOS WEALTHY SOLE RESCRUB RELIMIT",
        "wif_priv_key": "5JXK8jhtJM8jKXcpBHeWahzkfZ9c7ske31TkMR7eMeq1uWirYVD",
        "pub_key": "YYW7jcmGpu6KEUE352VtGB9PTo38Nut5qxXitfSgG6cDmAvxz2yin"
      }
    }


2.1.3 get_transaction_id
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
返回给定交易的 tx id （交易 ID ，或称交易哈希）

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
无

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""
:trx: JSON格式的完整交易

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0,"get_transaction_id",[{"operations":[[0,{"fee":{"total":{"amount":100000,"asset_id":0}},"from":250926091,"to":223331844,"amount":{"amount":100000,"asset_id":0},"extensions":{}}]]}]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0,"suggest_brain_key",[{"operations":[[0,{"fee":{"total":{"amount":100000,"asset_id":0}},"from":250926091,"to":223331844,"amount":{"amount":100000,"asset_id":0},"extensions":{}}]]}]], "id": 1}' http://localhost:8091


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": "5ea3a6ee9f030472f83fb436836b602a3a5ed6a5"
    }


2.2 查询类 API
-----------------------

2.2.1 get_account
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
获取账户基本信息。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
无

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:account_name_or_id:   uid或者账户昵称name，例如:"250926091"

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0,"get_account",[250926091]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0,"get_account",[250926091]], "id": 1}' http://localhost:8091


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": {
        "id": "1.2.1378",
        "uid": 250926091,
        "name": "yoyo250926091",
        "owner": {
          "weight_threshold": 1,
          "account_uid_auths": [],
          "key_auths": [
            [
              "YYW7UoNSEQAUPvnvoBRVKyPAD9845esnpiK6MgHinsn5yqr5UgT5W",
              1
            ]
          ]
        },
        "active": {
          "weight_threshold": 1,
          "account_uid_auths": [],
          "key_auths": [
            [
              "YYW6fU7Th8uESW9FZnpkhYaTUwtSvn3f2TQxFVA3ef2SSiwdZES71",
              1
            ]
          ]
        },
        "secondary": {
          "weight_threshold": 1,
          "account_uid_auths": [],
          "key_auths": [
            [
              "YYW5eDSFYeiqyFRajfPP8tTZM7mfUeyc7H65zmnHtDW4SQJdwqTBD",
              1
            ]
          ]
        },
        "memo_key": "YYW7SpC4QLY1LRRxFQ2hbYHdAyQo88L8qnPJcDJkiRMugcnFGUGvo",
        "reg_info": {
          "registrar": 206336051,
          "referrer": 25997,
          "registrar_percent": 0,
          "referrer_percent": 0,
          "allowance_per_article": {
            "amount": 0,
            "asset_id": 0
          },
          "max_share_per_article": {
            "amount": 0,
            "asset_id": 0
          },
          "max_share_total": {
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
      }
    }

2.2.2 get_full_account
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
获取账户详细信息。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
无

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是

请求参数
""""""""""""""""

:account_name_or_id:   uid或者账户昵称name，例如:"250926091"

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0,"get_full_account",[["250926091"]]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "get_full_account", [["250926091"]]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": {
        "account": {
          "id": "1.2.1378",
          "uid": 250926091,
          "name": "yoyo250926091",
          "owner": {
            "weight_threshold": 1,
            "account_uid_auths": [],
            "key_auths": [
              [
                "YYW7UoNSEQAUPvnvoBRVKyPAD9845esnpiK6MgHinsn5yqr5UgT5W",
                1
              ]
            ]
          },
          "active": {
            "weight_threshold": 1,
            "account_uid_auths": [],
            "key_auths": [
              [
                "YYW6fU7Th8uESW9FZnpkhYaTUwtSvn3f2TQxFVA3ef2SSiwdZES71",
                1
              ]
            ]
          },
          "secondary": {
            "weight_threshold": 1,
            "account_uid_auths": [],
            "key_auths": [
              [
                "YYW5eDSFYeiqyFRajfPP8tTZM7mfUeyc7H65zmnHtDW4SQJdwqTBD",
                1
              ]
            ]
          },
          "memo_key": "YYW7SpC4QLY1LRRxFQ2hbYHdAyQo88L8qnPJcDJkiRMugcnFGUGvo",
          "reg_info": {
            "registrar": 206336051,
            "referrer": 25997,
            "registrar_percent": 0,
            "referrer_percent": 0,
            "allowance_per_article": {
              "amount": 0,
              "asset_id": 0
            },
            "max_share_per_article": {
              "amount": 0,
              "asset_id": 0
            },
            "max_share_total": {
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
        },
        "statistics": {
          "id": "2.5.1378",
          "owner": 250926091,
          "total_ops": 11,
          "removed_ops": 0,
          "prepaid": 0,
          "csaf": 4200683,
          "core_balance": 1098850704,
          "core_leased_in": 0,
          "core_leased_out": 0,
          "average_coins": 1099970604,
          "average_coins_last_update": "2018-04-12T12:56:00",
          "coin_seconds_earned": "136484730731520",
          "coin_seconds_earned_last_update": "2018-04-12T12:56:00",
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
          "last_voter_sequence": 1,
          "last_platform_sequence": 1,
          "total_platform_pledge": 1000000000,
          "releasing_platform_pledge": 0,
          "platform_pledge_release_block_number": 4294967295,
          "last_post_sequence": 0
        },
        "csaf_leases_in": [],
        "csaf_leases_out": [],
        "witness_votes": [],
        "committee_member_votes": []
      }
    }

2.2.3 get_relative_account_history
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
获取账户历史。


支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
无

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


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

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0,"get_relative_account_history",["250926091",null,10,10,0]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0,"get_relative_account_history",["250926091",null,10,10,0]], "id": 1}' http://localhost:8091


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": [
        {
          "memo": "",
          "description": "Transfer 1.20000 YOYO from 250926091 to 209414065   (Fee: 0.20000 YOYO)",
          "sequence": 11,
          "op": {
            "id": "1.12.46722",
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
                "from": 250926091,
                "to": 209414065,
                "amount": {
                  "amount": 120000,
                  "asset_id": 0
                }
              }
            ],
            "result": [
              0,
              {}
            ],
            "block_timestamp": "2018-04-12T12:56:21",
            "block_num": 5946192,
            "trx_in_block": 0,
            "op_in_trx": 0,
            "virtual_op": 690
          }
        },
        {
          "memo": "",
          "description": "Transfer 10 YOYO from 250926091 to 209414065   (Fee: 0.20000 YOYO)",
          "sequence": 10,
          "op": {
            "id": "1.12.46721",
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
                "from": 250926091,
                "to": 209414065,
                "amount": {
                  "amount": 1000000,
                  "asset_id": 0
                }
              }
            ],
            "result": [
              0,
              {}
            ],
            "block_timestamp": "2018-04-12T12:55:57",
            "block_num": 5946184,
            "trx_in_block": 0,
            "op_in_trx": 0,
            "virtual_op": 689
          }
        }
      ]
    }



2.2.4 list_account_balances
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
获取账户余额。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
无

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:account:   uid或者账户昵称name，例如:"250926091"

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0,"list_account_balances",["250926091"]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0,"list_account_balances",["250926091"]], "id": 1}' http://localhost:8091

返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": [
        {
          "amount": 1098850704,
          "asset_id": 0
        }
      ]
    }

2.2.5 list_accounts_by_name
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
根据名称查找账号UID。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
无

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:lowerbound:   以此作为起始名称开始查询，设为空串则从头开始查
:limit:  返回数量限制，最多不能超过 1001

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0,"list_accounts_by_name",["yoyo",10]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0,"list_accounts_by_name",["yoyo",10]], "id": 1}' http://localhost:8091

返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": [
        [
          "yoyo10007071",
          10007071
        ],
        [
          "yoyo100090928",
          100090928
        ],
        [
          "yoyo100361976",
          100361976
        ],
        [
          "yoyo100459405",
          100459405
        ],
        [
          "yoyo100501159",
          100501159
        ],
        [
          "yoyo100583445",
          100583445
        ],
        [
          "yoyo100603302",
          100603302
        ],
        [
          "yoyo100735531",
          100735531
        ],
        [
          "yoyo10124233",
          10124233
        ],
        [
          "yoyo101530854",
          101530854
        ]
      ]
    }



2.2.6 get_witness
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
获取见证人信息。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
无

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:owner_account:   参数可以是 uid 或者账户昵称。

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0,"get_witness",["132826789"]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0,"get_witness",["132826789"]], "id": 1}' http://localhost:8091


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": {
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
        "total_votes": 1023267564,
        "by_pledge_position": "0",
        "by_pledge_position_last_update": "0",
        "by_pledge_scheduled_time": "45370982250075664161773192435",
        "by_vote_position": "0",
        "by_vote_position_last_update": "0",
        "by_vote_scheduled_time": "332544857826054970738151567847",
        "last_confirmed_block_num": 8168,
        "last_aslot": 8599,
        "total_produced": 25,
        "total_missed": 0,
        "url": ""
      }
    }



2.2.7 list_witnesses
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
查询指定借出人的币龄租借（借出）清单。


结果按借入人  uid 从小到大排序

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
无

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:lower_bound:   以此作为起始 uid 开始查询，设为 0 则从头开始查
:limit:  返回数量限制，最多不能超过 101
:order_by:   排序类型。取值范围[0,1,2]。 0:按uid由大到小排序；1:按得票数从多到少排序；2:按抵押从多到少排序


注意事项
""""""""""""""""
接口采用分页设计，若要获取全部的见证人，可以循环调用，直至返回见证人数小于limit为止。

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0,"list_witnesses",["132"]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0,"list_witnesses",["132"]], "id": 1}' http://localhost:8091


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": [
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
          "last_confirmed_block_num": 5994633,
          "last_aslot": 6366418,
          "total_produced": 518458,
          "total_missed": 32186,
          "url": ""
        },
        {
          "id": "1.5.2",
          "account": 26264,
          "name": "init2",
          "sequence": 1,
          "is_valid": true,
          "signing_key": "YYW71suPihtG7jJAGiVBCkd63ECHYebQaPa894oy3r54zk3eM1itt",
          "pledge": 1000000000,
          "pledge_last_update": "2017-09-12T21:02:51",
          "average_pledge": 1000000000,
          "average_pledge_last_update": "2017-09-13T21:20:36",
          "average_pledge_next_update_block": 4294967295,
          "total_votes": 0,
          "by_pledge_position": "0",
          "by_pledge_position_last_update": "0",
          "by_pledge_scheduled_time": "340282366580656096882718510549",
          "by_vote_position": "0",
          "by_vote_position_last_update": "0",
          "by_vote_scheduled_time": "340282366920938463463374607431768211455",
          "last_confirmed_block_num": 5994632,
          "last_aslot": 6366417,
          "total_produced": 518439,
          "total_missed": 32198,
          "url": ""
        },
        {
          "id": "1.5.3",
          "account": 26460,
          "name": "init3",
          "sequence": 1,
          "is_valid": true,
          "signing_key": "YYW71suPihtG7jJAGiVBCkd63ECHYebQaPa894oy3r54zk3eM1itt",
          "pledge": 1000000000,
          "pledge_last_update": "2017-09-12T21:02:54",
          "average_pledge": 1000000000,
          "average_pledge_last_update": "2017-09-13T21:20:39",
          "average_pledge_next_update_block": 4294967295,
          "total_votes": 0,
          "by_pledge_position": "0",
          "by_pledge_position_last_update": "0",
          "by_pledge_scheduled_time": "340282366580656096882718510549",
          "by_vote_position": "0",
          "by_vote_position_last_update": "0",
          "by_vote_scheduled_time": "340282366920938463463374607431768211455",
          "last_confirmed_block_num": 5994636,
          "last_aslot": 6366421,
          "total_produced": 518427,
          "total_missed": 32161,
          "url": ""
        },
        {
          "id": "1.5.4",
          "account": 26861,
          "name": "init4",
          "sequence": 1,
          "is_valid": true,
          "signing_key": "YYW71suPihtG7jJAGiVBCkd63ECHYebQaPa894oy3r54zk3eM1itt",
          "pledge": 1000000000,
          "pledge_last_update": "2017-09-12T21:03:00",
          "average_pledge": 1000000000,
          "average_pledge_last_update": "2017-09-13T21:20:45",
          "average_pledge_next_update_block": 4294967295,
          "total_votes": 0,
          "by_pledge_position": "0",
          "by_pledge_position_last_update": "0",
          "by_pledge_scheduled_time": "340282366580656096882718510549",
          "by_vote_position": "0",
          "by_vote_position_last_update": "0",
          "by_vote_scheduled_time": "340282366920938463463374607431768211455",
          "last_confirmed_block_num": 5994640,
          "last_aslot": 6366425,
          "total_produced": 518441,
          "total_missed": 32137,
          "url": ""
        },
        {
          "id": "1.5.5",
          "account": 27027,
          "name": "init5",
          "sequence": 1,
          "is_valid": true,
          "signing_key": "YYW71suPihtG7jJAGiVBCkd63ECHYebQaPa894oy3r54zk3eM1itt",
          "pledge": 1000000000,
          "pledge_last_update": "2017-09-12T21:05:15",
          "average_pledge": 1000000000,
          "average_pledge_last_update": "2017-09-13T21:23:00",
          "average_pledge_next_update_block": 4294967295,
          "total_votes": 0,
          "by_pledge_position": "0",
          "by_pledge_position_last_update": "0",
          "by_pledge_scheduled_time": "340282366580656096882718510549",
          "by_vote_position": "0",
          "by_vote_position_last_update": "0",
          "by_vote_scheduled_time": "340282366920938463463374607431768211455",
          "last_confirmed_block_num": 5994639,
          "last_aslot": 6366424,
          "total_produced": 518387,
          "total_missed": 32190,
          "url": ""
        }
      ]
    }



2.2.8 get_committee_member
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
获取理事成员信息。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
无

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:owner_account:   uid 或者账户昵称。 例如："25997"

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0,"get_committee_member",["25997"]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0,"get_committee_member",["25997"]], "id": 1}' http://localhost:8091


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": {
        "id": "1.4.0",
        "account": 25997,
        "name": "init1",
        "sequence": 1,
        "is_valid": true,
        "pledge": 0,
        "total_votes": 567814657,
        "url": ""
      }
    }


2.2.9 list_committee_members
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
列出当前有效的候选理事清单。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
无

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""
:lower_bound:   以此作为起始 uid 开始查询，设为 0 则从头开始查
:limit:  返回数量限制，最多不能超过 101
:order_by:   排序类型。取值范围[0,1,2]。 0:按uid由大到小排序；1:按得票数从多到少排序；2:按抵押从多到少排序


注意事项
""""""""""""""""
接口采用分页设计，若要获取全部的理事会，可以循环调用，直至返回理事会人数小于limit为止。

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0,"list_committee_members",[0,5,1]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0,"list_committee_members",[0,5,1]], "id": 1}' http://localhost:8091


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
          "total_votes": 567814657,
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
        },
        {
          "id": "1.4.2",
          "account": 26460,
          "name": "init3",
          "sequence": 1,
          "is_valid": true,
          "pledge": 0,
          "total_votes": 0,
          "url": ""
        },
        {
          "id": "1.4.3",
          "account": 26861,
          "name": "init4",
          "sequence": 1,
          "is_valid": true,
          "pledge": 0,
          "total_votes": 0,
          "url": ""
        },
        {
          "id": "1.4.4",
          "account": 27027,
          "name": "init5",
          "sequence": 1,
          "is_valid": true,
          "pledge": 0,
          "total_votes": 0,
          "url": ""
        }
      ]
    }



2.2.10 list_committee_proposals
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
列出所有尚未成功执行的理事会提案，包含正在投票表决的、已表决通过但还没到执行时间的。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
无

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


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

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "list_committee_proposals", []]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "list_committee_proposals", []], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": []
    }



2.2.11 get_platform_count
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
获取网络上平台的总数量

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
无

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


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

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "get_platform_count", []]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "get_platform_count", []], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": 6
    }


2.2.12 get_platform
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
根据平台所有人（owner）账号，获取平台对象信息

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
无

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:owner_account:  平台所有人账号

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "get_platform", ["250926091"]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "update_committee_account", [0, "get_platform", ["250926091"]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": {
        "id": "1.6.3",
        "owner": 250926091,
        "name": "NoPlatform",
        "sequence": 1,
        "is_valid": true,
        "total_votes": 0,
        "url": "www.example2.com",
        "pledge": 1000000000,
        "pledge_last_update": "2018-04-03T09:30:48",
        "average_pledge": 396825,
        "average_pledge_last_update": "2018-04-03T09:34:48",
        "average_pledge_next_update_block": 5684416,
        "extra_data": "{}",
        "create_time": "2018-04-03T09:30:48",
        "last_update_time": "2018-04-03T09:34:48"
      }
    }

2.2.13 list_platforms
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
按平台拥有者进行查询，列出当前有效的平台清单。


支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
无

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:lower_bound:  以此作为起始 uid 开始查询，设为 0 则从头开始查
:limit:  返回数量限制，最多不能超过 100
:order_by:   排序类型。取值范围[0,1,2]。 0:按uid由大到小排序；1:按得票数从多到少排序；2:按抵押从多到少排序


注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "list_platforms", [0,5,1]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "list_platforms", [0, "list_platforms", [0,5,1]], "id": 1}' http://localhost:8091/rpc


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
          "extra_data": "{\"login\":\"http://demo.yoyow.org:3000/authLogin\"}",
          "create_time": "2018-02-10T01:03:57",
          "last_update_time": "2018-02-11T06:49:12"
        },
        {
          "id": "1.6.5",
          "owner": 223331844,
          "name": "baidu",
          "sequence": 1,
          "is_valid": true,
          "total_votes": 0,
          "url": "",
          "pledge": 1000000000,
          "pledge_last_update": "2018-04-16T02:52:36",
          "average_pledge": 0,
          "average_pledge_last_update": "2018-04-16T02:52:36",
          "average_pledge_next_update_block": 6050467,
          "extra_data": "",
          "create_time": "2018-04-16T02:52:36",
          "last_update_time": "1970-01-01T00:00:00"
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
        }
      ]
    }





2.2.14 get_asset
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
根据给定的资产代码或者 id ，返回资产详细信息。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
无

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:asset_name_or_id:  资产符号或者资产id

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "get_asset", [ 3]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "get_asset", [ 3]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

  {
    "id": "1.3.3",
    "asset_id": 3,
    "symbol": "WOWO",
    "precision": 4,
    "issuer": 223331844,
    "options": {
      "max_supply": "1000000000000000",
      "market_fee_percent": 0,
      "max_market_fee": "1000000000000000",
      "issuer_permissions": 79,
      "flags": 0,
      "whitelist_authorities": [],
      "blacklist_authorities": [],
      "whitelist_markets": [],
      "blacklist_markets": [],
      "description": ""
    },
    "dynamic_asset_data_id": "2.3.3"
  }


2.2.15 list_assets
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

分页查询资产详细信息。


返回结果按资产代码的 ASCII 码顺序排序

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
无

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""
:lower_bound_symbol:  以此作为起始代码开始查询，顺序按资产代码的 ASCII 码排序
:limit:  返回数量限制，最多不能超过 101

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "list_assets", ["YOY", 4]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "list_assets", ["YOY", 4]], "id": 1}' http://localhost:8091/rpc


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
          "description": "卢俊义"
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
          "current_supply": "106901076031525",
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




2.3 钱包/私钥管理类 API
---------------------------------------


2.3.1 save_wallet_file
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
保存钱包文件，会保存到yoyo_client的执行文件夹下

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
wallet需要处于unlock状态

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:wallet_filename:   字符串，为备份的文件名。


注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "save_wallet_file", ["t3.json"]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "save_wallet_file", ["t3.json"]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
        "id": 1,
        "jsonrpc": "2.0",
        "result":null
    }



2.3.2 set_password
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
设置钱包密码

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
wallet需要处于new或者unlocked状态

new状态为wallet第一次运行，未曾设置password的状态。

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:password:   密码字符串 例如："1234"

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "set_password", ["1234"]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "set_password", ["1234"]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": null
    }

2.3.3 unlock
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
解锁钱包

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
wallet处于locked状态

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:password:   密码字符串 例如："1234"

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "unlock", ["1234"]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "unlock", ["1234"]], "id": 1}'


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": null
    }



2.3.4 lock
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
锁定钱包

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
无

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


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

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "lock", []]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "lock", []], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": null
    }

2.3.5 import_key
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
将一个私钥导入钱包，并指定一个相关账号。私钥和账号并不一定要有关联。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
wallet需要处于unlocked状态

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""
:account_name_or_id:   账号 uid 或者昵称
:wif_key:  私钥字符串

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "lock", []]}{"id":1, "method":"call", "params":[0, "import_key", ["250926091","5JLaW7u3EC4vVLbTmLo1XeSBGiTeRtqER1UsoLtYbFNnBafgPKG"]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "lock", []]}{"id":1, "method":"call", "params":[0, "import_key", ["250926091","5JLaW7u3EC4vVLbTmLo1XeSBGiTeRtqER1UsoLtYbFNnBafgPKG"]], "id": 1}' http://localhost:8091/rpc

返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": true
    }

2.3.6 dump_private_keys
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
列出钱包内所有私钥及对应公钥。


支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
wallet需要处于unlocked状态

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


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

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "dump_private_keys",[]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "dump_private_keys",[]], "id": 1}' http://localhost:8091/rpc

返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": [
        [
          "YYW5eDSFYeiqyFRajfPP8tTZM7mfUeyc7H65zmnHtDW4SQJdwqTBD",
          "5HvABsnYU1U7misWHq9mc6mE8QovBiy8H5rVZc3zKztgZsPfFMB"
        ],
        [
          "YYW6fU7Th8uESW9FZnpkhYaTUwtSvn3f2TQxFVA3ef2SSiwdZES71",
          "5JLaW7u3EC4vVLbTmLo1XeSBGiTeRtqER1UsoLtYbFNnBafgPKG"
        ]
      ]
    }


2.3.7 list_my_accounts_cached
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
列出钱包文件中所有缓存的账户（导入私钥时指定的账户）的信息。

注：该缓存信息不一定与链上数据同步。要想进行同步，请重新打开钱包文件。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
wallet需要处于unlocked状态

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


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

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "list_my_accounts_cached",[]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params": [0, "list_my_accounts_cached",[]], "id": 1}' http://localhost:8091/rpc


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
          "owner": {
            "weight_threshold": 1,
            "account_uid_auths": [],
            "key_auths": [
              [
                "YYW7UoNSEQAUPvnvoBRVKyPAD9845esnpiK6MgHinsn5yqr5UgT5W",
                1
              ]
            ]
          },
          "active": {
            "weight_threshold": 1,
            "account_uid_auths": [],
            "key_auths": [
              [
                "YYW6fU7Th8uESW9FZnpkhYaTUwtSvn3f2TQxFVA3ef2SSiwdZES71",
                1
              ]
            ]
          },
          "secondary": {
            "weight_threshold": 1,
            "account_uid_auths": [],
            "key_auths": [
              [
                "YYW5eDSFYeiqyFRajfPP8tTZM7mfUeyc7H65zmnHtDW4SQJdwqTBD",
                1
              ]
            ]
          },
          "memo_key": "YYW7SpC4QLY1LRRxFQ2hbYHdAyQo88L8qnPJcDJkiRMugcnFGUGvo",
          "reg_info": {
            "registrar": 206336051,
            "referrer": 25997,
            "registrar_percent": 0,
            "referrer_percent": 0,
            "allowance_per_article": {
              "amount": 0,
              "asset_id": 0
            },
            "max_share_per_article": {
              "amount": 0,
              "asset_id": 0
            },
            "max_share_total": {
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
        }
      ]
    }




2.4 操作/交易类 API
-----------------------------
以下操作涉及密钥权限的，需要导入相关的私钥，同时，保证wallet需处于解锁（unlocked）状态


2.4.1 transfer
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
根据uid列表 查询平台

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
需要转出人active key

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:from:  转出人（UID或昵称）
:to:  转入人（UID或昵称）
:amount:  金额，如果金额为小数建议使用字符串传参
:asset_symbol:   币种, 资产类型，当前只有"YOYO"
:memo:   备注（不带备注的话用空串""）
:broadcast:  是否广播，true or false


注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "transfer",[250926091, 209414065, "10", "YOYO", "feho", true]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "transfer",[250926091, 209414065, "10", "YOYO", "feho", true]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": {
        "ref_block_num": 57170,
        "ref_block_prefix": 852086171,
        "expiration": "2018-04-15T03:18:33",
        "operations": [
          [
            0,
            {
              "fee": {
                "total": {
                  "amount": 20898,
                  "asset_id": 0
                },
                "options": {
                  "from_csaf": {
                    "amount": 20898,
                    "asset_id": 0
                  }
                }
              },
              "from": 250926091,
              "to": 209414065,
              "amount": {
                "amount": 1000000,
                "asset_id": 0
              },
              "memo": {
                "from": "YYW7SpC4QLY1LRRxFQ2hbYHdAyQo88L8qnPJcDJkiRMugcnFGUGvo",
                "to": "YYW8PK8NtXW6JjtxqPV8QTgw4ejPEg4FgVeNV1maZDGzzNoEbgmr2",
                "nonce": "7783743918290282490",
                "message": "4468a7f3a5ac7fbf8125856381673030"
              }
            }
          ]
        ],
        "signatures": [
          "1f0a075215760089cf879b67ee6ba0aaaffa9408cd48c9040eee562909a8d67f5f7bbbb6401aabc69c00cd5d212f65b41204651f33442dc5b5b0056ce38f06c10e"
        ]
      }
    }


2.4.2 create_witness
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
创建见证人。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""
需要见证人所有者的active key

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""
:owner_account:  账号（UID或昵称）
:block_signing_key:  出块签名公钥,
:pledge_amount:  抵押金额
:pledge_asset_symbol:   抵押币种（YOYO）
:url: 介绍链接
:broadcast:  是否广播

其中：签名公钥为 YYW1111111111111111111111111111111114T1Anm 表示暂时离线

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "create_witness", ["223331844", "YYW1111111111111111111111111111111114T1Anm","1000000", "YOYO", "http://www.yoyow.org", true]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "create_witness", ["223331844", "YYW1111111111111111111111111111111114T1Anm","100", "YOYO", "http://www.yoyow.org", true]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
      "ref_block_num": 58457,
      "ref_block_prefix": 2372452101,
      "expiration": "2018-05-03T11:28:36",
      "operations": [[
          13,{
            "fee": {
              "total": {
                "amount": 100000000,
                "asset_id": 0
              }
            },
            "account": 223331844,
            "block_signing_key": "YYW1111111111111111111111111111111114T1Anm",
            "pledge": {
              "amount": "100000000000",
              "asset_id": 0
            },
            "url": "http://www.yoyow.org"
          }
        ]
      ],
      "signatures": [
        "202857a37e91889a1c6124a2e3405eff00647b315aa55db7989334e187a5a92c1f0cb4bb00531fa525e53f26403e8bd323a9e46f8289b0039ed2caeb951f70eb28"
      ]
    }



2.4.3 update_witness
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
修改见证人信息。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
需要见证人所有者的active key

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:witness_account:  账号（UID或昵称）
:block_signing_key:  新的出块签名公钥，不需修改则输入 null
:pledge_amount:  新的抵押金额，不需修改则输入 null
:pledge_asset_symbol:   新的抵押币种（YOYO），不需修改则输入 null
:url: 新的介绍链接，不需修改则输入 null
:broadcast:  是否广播

其中，抵押金额和币种必须同时出现或者同时不出现，目前币种只能是 YOYO 


注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "update_witness", ["223331844", null,"100345", "YOYO", null, true]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "update_witness", ["223331844", null,"100345", "YOYO", null, true]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

  {
    "id": 1,
    "jsonrpc": "2.0",
    "result": {
      "ref_block_num": 13103,
      "ref_block_prefix": 3050749194,
      "expiration": "2018-05-04T04:17:42",
      "operations": [
        [
          14,
          {
            "fee": {
              "total": {
                "amount": 1000000,
                "asset_id": 0
              },
              "options": {
                "from_csaf": {
                  "amount": 1000000,
                  "asset_id": 0
                }
              }
            },
            "account": 223331844,
            "new_pledge": {
              "amount": "10034500000",
              "asset_id": 0
            }
          }
        ]
      ],
      "signatures": [
        "1f6503a1e7dd15d1d9d5fe9cdaddddea39acf40071bd5621458b9abf3e0c8709f63fedfac89adc571fcc8af20fe6beb9f94d93d47256d3170b314e87153492357e"
      ]
    }
  }


2.4.4 create_committee_member
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
创建候选理事身份。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
否

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:owner_account:  账号（UID或昵称）
:pledge_amount:  抵押金额
:pledge_asset_symbol:   抵押币种（YOYO）
:url: 介绍链接
:broadcast:  是否广播

注意事项
""""""""""""""""
查询到的资产实际只有YOYO可用。

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "create_committee_member", ["223331844","1000", "YOYO", "http://www.yoyow.org", true]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "create_committee_member", ["223331844","1000", "YOYO", "http://www.yoyow.org", true]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""

::

  {
    "id": 1,
    "jsonrpc": "2.0",
    "result": {
      "ref_block_num": 13153,
      "ref_block_prefix": 1417310192,
      "expiration": "2018-05-04T04:20:12",
      "operations": [
        [
          8,
          {
            "fee": {
              "total": {
                "amount": 10000000,
                "asset_id": 0
              }
            },
            "account": 223331844,
            "pledge": {
              "amount": 100000000,
              "asset_id": 0
            },
            "url": "http://www.yoyow.org"
          }
        ]
      ],
      "signatures": [
        "1f2b34fe5e2437be46d83ec2f0f4482e1b5df509131131c41eeb16e484df5e4ea96df19f82be294433bc751e84d6dcc28073e758ad7de1ca48c4b36fb2d41b2def"
      ]
    }
  }


2.4.5 update_committee_member
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
修改候选理事信息。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
否

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:committee_member_account:  账号（UID或昵称）
:pledge_amount:  新的抵押金额，不需修改则输入 null
:pledge_asset_symbol:   新的抵押币种（YOYO），不需修改则输入 null
:url: 新的介绍链接，不需修改则输入 null
:broadcast:  是否广播

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "update_committee_member", ["223331844", "10234", "YOYO", null, true]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "update_committee_account", ["250926091","10000", "YOYO", null, true]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

  {
    "id": 1,
    "jsonrpc": "2.0",
    "result": {
      "ref_block_num": 13189,
      "ref_block_prefix": 2763581564,
      "expiration": "2018-05-04T04:22:00",
      "operations": [
        [
          9,
          {
            "fee": {
              "total": {
                "amount": 1000000,
                "asset_id": 0
              },
              "options": {
                "from_csaf": {
                  "amount": 1000000,
                  "asset_id": 0
                }
              }
            },
            "account": 223331844,
            "new_pledge": {
              "amount": 1023400000,
              "asset_id": 0
            }
          }
        ]
      ],
      "signatures": [
        "20506ea2aadb44a57ae4bb60c71b0c2002f89410d4941ed83d3323c4bed2f883ee4d045c9a326e331b49770db32799c63b854a67dd4ff998b74f6b457cb7d9157e"
      ]
    }
  }



2.4.6 set_voting_proxy
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
设置投票代理。

账户A设置账户B为投票代理，则B的投票对象得到的票数为A的有效票数+B的有效票数。 A 称之为委托人，B 称之为代理人

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
需要委托人账号的active key

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:account_to_modify:  委托人账号（UID或昵称）
:voting_account:  代理人账号（用UID或昵称设置代理，null为取消代理）
:broadcast:  是否广播

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "set_voting_proxy", ["250926091", "abit", true]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "set_voting_proxy", ["250926091", "abit", true]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": {
        "ref_block_num": 18995,
        "ref_block_prefix": 2835940919,
        "expiration": "2018-04-16T02:06:36",
        "operations": [
          [
            5,
            {
              "fee": {
                "total": {
                  "amount": 100000,
                  "asset_id": 0
                },
                "options": {
                  "from_csaf": {
                    "amount": 100000,
                    "asset_id": 0
                  }
                }
              },
              "voter": 223331844,
              "proxy": 250926091
            }
          ]
        ],
        "signatures": [
          "1f793459c8c7e06e80b2b34d2d13a0fb46e5d4f839953f6fae96af16acf389b51c534c35d2f85fe5d9f8e7316b1bb66941c2591e31afe7e5bbfee8802877ad7af0"
        ]
      }
    }




2.4.7 update_witness_votes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
见证人投票。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
需要投票人的active key

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:voting_account:  账号（UID或昵称）
:witnesses_to_add:  增加支持的见证人清单（UID或昵称）
:witnesses_to_remove:   移除支持的见证人清单（UID或昵称）
:broadcast:  是否广播

witnesses_to_add和witnesses_to_remove两个清单可以都为空"[]"，表示刷新投票意向。

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "update_witness_votes", ["250926091", ["abit"], [], true]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "update_witness_votes", ["250926091", ["abit"], [], true]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": {
        "ref_block_num": 18961,
        "ref_block_prefix": 1229162670,
        "expiration": "2018-04-16T02:04:54",
        "operations": [
          [
            15,
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
              "witnesses_to_add": [
                209414065
              ],
              "witnesses_to_remove": []
            }
          ]
        ],
        "signatures": [
          "206badbed989fcf01c93a2eda807976bae29f2e95ca2dcaa83f645be6c3bffcbc178199f4e4816801643cc9ee158fc4e8f450c2082763ac163e1b875bfb82f3a25"
        ]
      }
    }




2.4.8 update_committee_member_votes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
理事会选举投票。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
需要投票人的active key

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:voting_account:  投票人账号（UID或昵称）
:committee_members_to_add:  数组，增加支持的候选理事清单（UID或昵称）
:committee_members_to_remove:  数组，移除支持的候选理事清单（UID或昵称）
:broadcast:  是否广播

committee_members_to_add，committee_members_to_remove两个清单可以都为空"[]"，表示刷新投票意向。

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "update_committee_member_votes", ["250926091", ["init1"], [],  true]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "update_committee_member_votes", ["250926091", ["init1"], [],  true]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": {
        "ref_block_num": 19152,
        "ref_block_prefix": 1139468448,
        "expiration": "2018-04-16T02:14:27",
        "operations": [
          [
            10,
            {
              "fee": {
                "total": {
                  "amount": 100000,
                  "asset_id": 0
                },
                "options": {
                  "from_csaf": {
                    "amount": 100000,
                    "asset_id": 0
                  }
                }
              },
              "voter": 250926091,
              "committee_members_to_add": [
                25997
              ],
              "committee_members_to_remove": []
            }
          ]
        ],
        "signatures": [
          "1f35562e4301c20f293977ffe27399ccf961fc3d5c0c9d928730ed5af03af24637599e30d070032bae887d9db3201c891b1c362dd0324e8bd9b02064d679a65be3"
        ]
      }
    }



2.4.9 collect_csaf_with_time
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
领取积分，需指定时间参数，领取积累到指定时间的积分。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
需要领取者的Secondary key

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:from:  领取账号（UID或昵称）
:to:  接收账号（UID或昵称）
:amount:   领取金额
:asset_symbol: 领取币种( 币种只能是 YOYO )
:time:  指定时间，例如："2018-04-16T02:44:00" ，该时间为UTC时间，且不得早于当前链上新出块的时间5分钟。
:broadcast:  是否广播

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "collect_csaf_with_time", ["223331844", "223331844", "0.5", "YOYO", "2018-04-16T02:44:00" true]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "collect_csaf_with_time", ["223331844", "223331844", "0.5", "YOYO", "2018-04-16T02:44:00" true]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": {
        "ref_block_num": 19813,
        "ref_block_prefix": 1809327617,
        "expiration": "2018-04-16T02:47:30",
        "operations": [
          [
            6,
            {
              "fee": {
                "total": {
                  "amount": 100000,
                  "asset_id": 0
                },
                "options": {
                  "from_csaf": {
                    "amount": 100000,
                    "asset_id": 0
                  }
                }
              },
              "from": 223331844,
              "to": 223331844,
              "amount": {
                "amount": 50000,
                "asset_id": 0
              },
              "time": "2018-04-16T02:44:00"
            }
          ]
        ],
        "signatures": [
          "1f250855fcc4e4ef093c14990411b1cfd41f97de43447e1b6a21cbe26eb95f6c9671b7c0d5ba4365d76018d277086c34c1d73a1f90c817f4d073852c6f041daf72",
          "2061c58d04a7ad9f60af1f145c837f57475d3d1785754527753b1144c1bef445240faa079b5927956be10693711b392b7a52fb55439addacbcee94a40e61f13f84"
        ]
      }
    }



2.4.10 collect_csaf
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
领取积分，领取积累到当前时间（分钟）的积分。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
需要领取者的Secondary key

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:from:  领取账号（UID或昵称）
:to:  接收账号（UID或昵称）
:amount:   领取金额
:asset_symbol: 领取币种( 币种只能是 YOYO )
:broadcast:  是否广播

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "collect_csaf", ["250926091", "250926091", 1, "YOYO", true]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "collect_csaf", ["250926091", "250926091", 1, "YOYO", true]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": {
        "ref_block_num": 19646,
        "ref_block_prefix": 555752677,
        "expiration": "2018-04-16T02:39:09",
        "operations": [
          [
            6,
            {
              "fee": {
                "total": {
                  "amount": 100000,
                  "asset_id": 0
                },
                "options": {
                  "from_csaf": {
                    "amount": 100000,
                    "asset_id": 0
                  }
                }
              },
              "from": 250926091,
              "to": 250926091,
              "amount": {
                "amount": 100000,
                "asset_id": 0
              },
              "time": "2018-04-16T02:37:00"
            }
          ]
        ],
        "signatures": [
          "203a417b25f10110d8143d7476976abbcbb3490f13432630366e5b0d1d8d7580573c8595e93109af4a55282756b8b4916ae055147cceae1bc7b85f2b0a7f2fa042",
          "2054d3b25618ddaeae499297a483d5490bac77f35bac7dd850645400d7f8001a2265cd997ff62db54740e9fcda52b0bbbaf5aa6d12d3fbcd65a71e2ccf6baa1e1a"
        ]
      }
    }




2.4.11 create_platform
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
创建平台

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
需要申请者的Active key

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:owner_account:  创建者账号
:name:  平台名称
:pledge_amount:   抵押数量，当前不得少于10000 YOYO
:pledge_asset_symbol: 抵押币种（YOYO）
:url: 平台链接
:extra_data:  平台额外数据
:broadcast:  是否广播

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "create_platform", ["223331844", "yoyo.club", "10000", "YOYO", "", "", true]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "create_platform", ["223331844", "yoyow.club", "10000", "YOYO", "", "", true]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": {
        "ref_block_num": 19954,
        "ref_block_prefix": 1357577324,
        "expiration": "2018-04-16T02:54:33",
        "operations": [
          [
            20,
            {
              "fee": {
                "total": {
                  "amount": 100007811,
                  "asset_id": 0
                }
              },
              "account": 223331844,
              "pledge": {
                "amount": 1000000000,
                "asset_id": 0
              },
              "name": "baidu",
              "url": "",
              "extra_data": ""
            }
          ]
        ],
        "signatures": [
          "20534af4af03c6d4001c797dde6ac438a6b3d31c77b94cb8e4b6519e681a289c69370057de58412bb5e3ba8320ab975d33012bb92b20509e3daee6582affce8e80"
        ]
      }
    }



2.4.12 update_platform
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
修改平台信息。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
否

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:platform_account:  账号（UID或昵称）
:name: 新的平台名称，不需修改则输入 null
:pledge_amount:  新的抵押金额，不需修改则输入 null
:pledge_asset_symbol:   新的抵押币种（YOYO），不需修改则输入 null
:url: 新的介绍链接，不需修改则输入 null
:extra_data:  新的平台额外数据
:broadcast:  是否广播

注：
抵押金额和币种必须同时出现或者同时不出现，目前币种只能是 YOYO。
抵押金额为 0 则为关闭平台


注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "update_platform", ["223331844", "NUUUU", null, null, "http://www.example.com", "http://www.example.com", true]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "update_platform", ["223331844", "NUUUU", null, null, "http://www.example.com", "http://www.example.com", true]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": {
        "ref_block_num": 20262,
        "ref_block_prefix": 1534083365,
        "expiration": "2018-04-16T03:09:57",
        "operations": [
          [
            21,
            {
              "fee": {
                "total": {
                  "amount": 1053709,
                  "asset_id": 0
                },
                "options": {
                  "from_csaf": {
                    "amount": 1053709,
                    "asset_id": 0
                  }
                }
              },
              "account": 223331844,
              "new_name": "NUUUU",
              "new_url": "http://www.example.com",
              "new_extra_data": "http://www.example.com"
            }
          ]
        ],
        "signatures": [
          "202e8e53a7e58d4b60c7bf7b0d3f8076a6c9b8f7c472c48e61463cff68228e2cf643404bf954f1c7596deb05630942c95057ff397f31753bff069e5754894efcad"
        ]
      }
    }



2.4.13 update_platform_votes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
为平台投票

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
需要投票者的Active key

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:voting_account:  投票人账号（UID或昵称）
:platforms_to_add:  增加支持的平台清单（UID或昵称）
:platforms_to_remove:   移除支持的平台清单（UID或昵称）
:broadcast:  是否广播

latforms_to_add，platforms_to_remove 两个清单可以都为空，表示刷新投票意向。

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "update_platform_votes", ["250926091", ["223331844"], [], true]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "update_platform_votes", ["250926091", ["223331844"], [], true]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": {
        "ref_block_num": 20494,
        "ref_block_prefix": 3288350547,
        "expiration": "2018-04-16T03:21:33",
        "operations": [
          [
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
          ]
        ],
        "signatures": [
          "20274d50cf4905fe072e3257632335546c386721f2d608cf3939316f7167ddbea55a28616cc790b00aea5bc89b6649e56c04c8121f50a97c2ca4b3f587ac5e922e"
        ]
      }
    }




2.4.14 account_auth_platform
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
账户对平台授权。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
主控密钥

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:account:  授权账号（UID或昵称）
:platform_owner:  平台所有者账号（UID或昵称）
:broadcast:  是否广播

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "account_auth_platform", ["250926091", "223331844", true]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "account_auth_platform", ["250926091", "223331844", true]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": {
        "ref_block_num": 26612,
        "ref_block_prefix": 1858930703,
        "expiration": "2018-07-05T02:34:57",
        "operations": [
          [
            23,
            {
              "fee": {
                "total": {
                  "amount": 10000,
                  "asset_id": 0
                },
                "options": {
                  "from_csaf": {
                    "amount": 10000,
                    "asset_id": 0
                  }
                }
              },
              "uid": 223331844,
              "platform": 250926091
            }
          ]
        ],
        "signatures": [
          "200ee643c33b074ad002bc6f4b477ff48dbee76bd3aa1c3c5b3a4c064b1f39581e61f93e957128c1c20737eafa2e09c56f6d0f97468676cfa2a15d04f50da0774c"
        ]
      }
    }




2.4.15 account_cancel_auth_platform
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
账户取消对平台授权。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
主控密钥

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""
:account:  授权账号（UID或昵称）
:platform_owner:  平台所有者账号（UID或昵称）
:broadcast:  是否广播


注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "account_cancel_auth_platform", ["250926091", "223331844", true]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "account_cancel_auth_platform", ["250926091", "223331844", true], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
      "id": 1,
      "jsonrpc": "2.0",
      "result": {
        "ref_block_num": 26736,
        "ref_block_prefix": 2510951750,
        "expiration": "2018-07-05T02:41:09",
        "operations": [
          [
            24,
            {
              "fee": {
                "total": {
                  "amount": 10000,
                  "asset_id": 0
                },
                "options": {
                  "from_csaf": {
                    "amount": 10000,
                    "asset_id": 0
                  }
                }
              },
              "uid": 223331844,
              "platform": 250926091
            }
          ]
        ],
        "signatures": [
          "2021240ab7c321fad009693c8a598363aa840fcde0f07cc54d72b8c3b7d6e116d4357127328faea805aff7d632fb0a09e2ce6ac203d4a97b2dd30c139548939d38"
        ]
      }
    }


2.4.16 create_asset
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
创建资产

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
需要申请者的Active key

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:issuer:  创建人UID
:symbol:  要创建资产的符号
:precision:   精度（保留几位小数）
:common: 选项，详见下文选项参数结构
:initial_supply:  初始流通量为整型表示，即：实际金额 = initial_supply / ( 10 ^ precision )
:broadcast:  是否广播

其中选项参数结构
::
    asset_options {
          // 该资产在任何给定时间可能存在的最大供应量。 这可以和 GRAPHENE_MAX_SHARE_SUPPLY 一样大 
          // 特别说明：该最大供应量为最小单位Token的数量，比如发行的最大供应量为30000，精度设为2，则实际的供应量为30000/(10^2)=300个Token，最小的交易单位为0.01。
          max_supply = GRAPHENE_MAX_SHARE_SUPPLY;
          // （预留字段，暂未使用，必须为 0 ）
          market_fee_percent = 0;
          // （预留字段，暂未使用，必须为 0 ）
          max_market_fee = GRAPHENE_MAX_SHARE_SUPPLY;

          // 发行人有权更新的标志
          issuer_permissions = UIA_ASSET_ISSUER_PERMISSION_MASK;
          // 此权限上的当前活动标志
          flags = 0;

          // 一组白名单的账户可以使用此资产。 如果whitelist_authorities不为空，则只有whitelist_authorities中的帐户才可以持有，使用或转让资产。
          whitelist_authorities;
          // 一组黑名单的帐户，不可以持有，使用此资产。 
          blacklist_authorities;

          // 定义该资产可能在市场上交易的资产（预留字段，暂未使用）
          whitelist_markets;
          // 定义该资产不得在市场上交易的资产，不得重叠白名单 （预留字段，暂未使用）
          blacklist_markets;

          // 描述该资产的含义/目的的数据，费用将按照描述的大小进行收费。
          string description;
       };
其中， issuer_permisisons 和 flags 两个字段，是以整数方式表示，该整数转换为二进制后，每一位代表一个权限或者标志。目前定义如下：
::
      enum asset_issuer_permission_flags
         {
            white_list           = 0x02,    // 白名单，如果启用，则该资产发行人可以控制他人是否可以使用该资产（这里的“使用”包含转账等操作）
            override_authority   = 0x04,    // 强制转账，如果启用，则该资产发行人可以强制转走或者收回其他人账户里的该类资产
            transfer_restricted  = 0x08,    // 限制转账，如果启用，则转账的发起者或者接收者必须是该资产发行人
            issue_asset          = 0x200,   // 发行资产，如果启用，则该资产发行人可以增加一定数量的该类资产到某账户，同时增加该资产的当前流通总量
            change_max_supply    = 0x400,   // 修改流通量上限，如果启用，则该资产发行人可以修改该资产的流通量上限
         };

目前尚未使用的位，在 issuer_permissions 和 flags 两个字段中必须为 0 。
在flags 字段里，某一位如果为 1 ，则表示该资产启用了这一位对应的参数；如果为 0 ，则表示没有启用。
在issuer_permissions 字段里，如果某一位如果为 0 ，则表示资产发行人可以修改 flags 字段里对应的参数位；如果为 1 ，则表示不可修改。

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "create_asset", ["250926091","TOTOTO", 4, {"max_supply":300000,"mare_percent":0,"max_market_fee":0,"issuer_permissions":4}ket_fe, 200000, true]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "create_asset", ["250926091","TOTOTO", 4, {"max_supply":300000,"market_fee_percent":0,"max_market_fee":0,"issuer_permissions":4}, 200000, true]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
      "ref_block_num": 57735,
      "ref_block_prefix": 998946957,
      "expiration": "2018-07-06T04:31:06",
      "operations": [[
          25,{
            "fee": {
              "total": {
                "amount": 50000000,
                "asset_id": 0
              }
            },
            "issuer": 250926091,
            "symbol": "TOTOTO",
            "precision": 4,
            "common_options": {
              "max_supply": 300000,
              "market_fee_percent": 0,
              "max_market_fee": 0,
              "issuer_permissions": 4,
              "flags": 0,
              "whitelist_authorities": [],
              "blacklist_authorities": [],
              "whitelist_markets": [],
              "blacklist_markets": [],
              "description": ""
            },
            "extensions": {
              "initial_supply": 200000
            }
          }
        ]
      ],
      "signatures": [
        "1f32dc80319019ff0c5eff27c9d1e15c7371fcc2825969b8e42a6429fb44c2ac3a75344b382e00c9f7f4af06600b0aff93aa380bf81b94f62b1abe0bfc807fcb6f"
      ]
    }






2.4.17 update_asset
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
更新资产信息。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
需要资产所有者的Active key

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:symbol:  资产符号
:new_issuer:  新的资产所有人
:new_options:   新的资产选项（见create_asset 中的 common参数结构），不需修改则输入 null
:broadcast:  是否广播

注意事项
""""""""""""""""
只有资产发行人才能使用这个功能。

只有当前流通量为零时，才能修改精度。

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "update_asset", ["WOWO", null, {"max_supply":"2000000000"}, true]]}
JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "update_asset", ["WOWO", null, {"max_supply":"2000000000"}, true]]}, "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

  {
    "id": 1,
    "jsonrpc": "2.0",
    "result": {
      "ref_block_num": 12862,
      "ref_block_prefix": 653302827,
      "expiration": "2018-05-04T04:05:39",
      "operations": [
        [
          26,
          {
            "fee": {
              "total": {
                "amount": 50000000,
                "asset_id": 0
              }
            },
            "issuer": 223331844,
            "asset_to_update": 3,
            "new_options": {
              "max_supply": 2000000000,
              "market_fee_percent": 0,
              "max_market_fee": "1000000000000000",
              "issuer_permissions": 79,
              "flags": 0,
              "whitelist_authorities": [],
              "blacklist_authorities": [],
              "whitelist_markets": [],
              "blacklist_markets": [],
              "description": ""
            }
          }
        ]
      ],
      "signatures": [
        "2030b2b084ab8e47bc2da5c863475776eac2cde1feba4cb3575eb7a7e86f96c9594a9778280c740098a5eec26f84b57993d925d2630c1a3dc31395c9938a676089"
      ]
    }
  }



2.4.18 enable_allowed_assets
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
账户主动启用或停用账户端资产白名单。

该白名单默认为停用状态，停用白名单即 账户可以发送和接收任何资产。

白名单处于启用状态时，该账户只能发送和接收名单内的资产，同时可使用 update_allowed_assets 命令更新白名单。

从停用状态变更为启用状态时，白名单中默认只有“核心资产”，即 YOYO 。

从启用状态停用白名单后，数据清除。重新启用时需重新添加需要的资产。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
需要资产所有者的Active key

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:account:  账号（UID或昵称）
:enable:  是否启用（ true 为启用， false 为停用 ）
:broadcast:  是否广播

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "enable_allowed_assets", ["250926091", false, true]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "issue_asset", "250926091", false, true]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
      "ref_block_num": 56000,
      "ref_block_prefix": 3273656390,
      "expiration": "2018-07-06T03:04:21",
      "operations": [[
          34,{
            "fee": {
              "total": {
                "amount": 100000,
                "asset_id": 0
              },
              "options": {
                "from_csaf": {
                  "amount": 100000,
                  "asset_id": 0
                }
              }
            },
            "account": 250926091,
            "enable": false
          }
        ]
      ],
      "signatures": [
  
  
  
  
  "1f7e06ed28f6017e9a25ee1d165ec1aae330fb87abc9486c264c30f7de2e8d2369077509d61d96df016fbeec55bbd541532b90f35bf2e18278c87369fa87d7158b"
      ]
    }
  


2.4.19 update_allowed_assets
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
更新账户端资产白名单。

只有白名单处于开启状态时，才能更新。
不能移除 YOYO 。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
需要资产所有者的Active key

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:account:  账号（UID或昵称）
:assets_to_add:  添加到白名单的资产清单（资产代码或 id ）
:assets_to_remove:   从白名单移除的资产清单（资产代码或 id ）
:broadcast:  是否广播

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "issue_asset", ["250926091", "100000", "WOWO", "memo", true]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "issue_asset", ["250926091", "100000", "WOWO", "memo", true]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::
  
    {
      "ref_block_num": 56441,
      "ref_block_prefix": 3725987382,
      "expiration": "2018-07-06T03:26:24",
      "operations": [[
          35,{
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
            "account": 250926091,
            "assets_to_add": [
              3
            ],
            "assets_to_remove": []
          }
        ]
      ],
      "signatures": [
        "200f4362763eef9a5f0de6c461d2dcde77e665ae5ac2d462c89dd1f4ba22b35b7b21662e306d1e52c530815983f284d79b3191ebd4d3fea43f135aff4e21fe39a3",
        "1f199078220bf0d6c2fcf28e0bd4fbf9afc1eb7db241997173a469d88d538d36c03c3299136cf42a030810c5adf250be990029391c0ad7ae8268e5bee4850f07bd"
      ]
    }


2.4.20 issue_asset
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
分配发行的资产给某个账号

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
需要资产所有者的Active key

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:to_account:  发行到的目标账号
:amount:  数量
:symbol:   资产符号
:memo:  备注
:broadcast:  是否广播

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "issue_asset", ["250926091", "100000", "WOWO", "memo", true]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "issue_asset", ["250926091", "100000", "WOWO", "memo", true]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::
  
  {
    "ref_block_num": 55598,
    "ref_block_prefix": 1065296620,
    "expiration": "2018-05-03T09:05:39",
    "operations": [[
        27,{
          "fee": {
            "total": {
              "amount": 2008984,
              "asset_id": 0
            }
          },
          "issuer": 223331844,
          "asset_to_issue": {
            "amount": 1000000000,
            "asset_id": 3
          },
          "issue_to_account": 250926091,
          "memo": {
            "from": "YYW8EeMDaSmDg8zLXL272kcm4W7vUF4c4tBkfQpV7N79PBjgGjkDN",
            "to": "YYW7SpC4QLY1LRRxFQ2hbYHdAyQo88L8qnPJcDJkiRMugcnFGUGvo",
            "nonce": "3388232258004121975",
            "message": "fc712e427e57aae3d6819e3427b7f90a"
          }
        }
      ]
    ],
    "signatures": [
      "205e29664c74590e1af73aa645374dc25f2900758e5213a3120fdf285d7940d5e66339311a97513cf78bd794916619f5813c940a9e7bc4b942fbd9fed49a2a7d27"
    ]
  }


2.4.21 reserve_asset
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
销毁自己账户中指定数量的指定资产。
操作完成后，该资产类型的流通总量相应减少。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
需要资产所有者的Active key

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:from:  账号（UID或昵称）
:amount:  金额
:symbol:   币种（资产代码）
:broadcast:  是否广播

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "reserve_asset", ["250926091", "1000", "WOWO", true]]}

JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "reserve_asset", ["250926091", "1000", "WOWO", true]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

    {
      "ref_block_num": 56703,
      "ref_block_prefix": 1905534427,
      "expiration": "2018-07-06T03:39:30",
      "operations": [[
          28,{
            "fee": {
              "total": {
                "amount": 10000,
                "asset_id": 0
              },
              "options": {
                "from_csaf": {
                  "amount": 10000,
                  "asset_id": 0
                }
              }
            },
            "payer": 250926091,
            "amount_to_reserve": {
              "amount": 10000000,
              "asset_id": 3
            }
          }
        ]
      ],
      "signatures": [
        "1f6a2f416a525ae4754a98388cb4464f60f1b77659c5a742d8b76a80d581484ce5681c24aa51c7379acaa8247df49d962cb28b5910e179a7de9c8a48e49fe7cc5a"
      ]
    }



2.4.22 override_transfer
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
强制转账。资产发行人强制某账户将一定数量资产转到另一账户。

只有资产发行人才能使用这个功能。

备注用发行人备注私钥加密，转入人可解密备注，转出人不可解密。

支持格式
""""""""""""""""
JSON 

请求方式
""""""""""""""""
WebSocket; JSON-RPC

所需密钥权限
""""""""""""""""""
需要资产发行人的Active key和memo key

访问授权限制
""""""""""""""""""
| 访问级别: 普通接口
| 频次限制: 是


请求参数
""""""""""""""""

:from:  转出人（UID或昵称）
:to:  转入人（UID或昵称）
:amount:  金额
:symbol:   币种（资产代码）
:memo:  备注（不带备注的话用空串""）
:broadcast:  是否广播

注意事项
""""""""""""""""
无

调用样例及调试工具
"""""""""""""""""""""""""""""""""
WebSocket:
::

    wscat -c ws://localhost:8091
    {"id":1, "method":"call", "params":[0, "override_transfer", ["216494599", "250926091", 2, "SOSO", "memo", true]]}


JSON-RPC:
::

    curl --data '{"jsonrpc": "2.0", "method": "call", "params":[0, "override_transfer", ["216494599", "250926091", 2, "SOSO", "memo", true]], "id": 1}' http://localhost:8091/rpc


返回结果
""""""""""""""""
::

  {
    "ref_block_num": 61983,
    "ref_block_prefix": 2781522721,
    "expiration": "2018-07-06T08:03:30",
    "operations": [[
        30,{
          "fee": {
            "total": {
              "amount": 2008984,
              "asset_id": 0
            },
            "options": {
              "from_csaf": {
                "amount": 2008984,
                "asset_id": 0
              }
            }
          },
          "issuer": 223331844,
          "from": 216494599,
          "to": 250926091,
          "amount": {
            "amount": 20000,
            "asset_id": 283
          },
          "memo": {
            "from": "YYW5jiVsASqAyUibUb8awgDapFqfjQJo3SfvTEPJHxir8r3ggVtZU",
            "to": "YYW8Z8rtp7oEZFJi7Ar9HC4r4d15jXCVTQ2CAosjSnkSFdbc74GJ3",
            "nonce": "9585190871224727019",
            "message": "b0e7b1997c2142125c0369c4304a33c1"
          }
        }
      ]
    ],
    "signatures": [
      "20008db33ea71863fc7f4b97763a34e96e8b9d87714b1445d33c34a6b7bb3015c14d0a130c1ee74946c19b5e992c02249872e1bf601ca24380ebef22e5119a2894"
    ]
  }
