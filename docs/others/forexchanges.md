
# 交易所对接指南
# Instructions for Exchanges

本文主要介绍交易所对接YOYOW主链的细则。

We here describe how to integrate your exchange with the YOYOW blockchain step-by-step.

## 准备工作
## Preparation

### 硬件设备
### Hardware

推荐的硬件配置：4GB内存，20G硬盘，两核CPU 即可

The recommended hardware by now is a VPS with 2GB of RAM and 20GB HDD. Single core is OK.

当前支持的平台：

* Ubuntu 16.04 LTS 64 bit
* Windows Servers 64 bit

下文以Ubuntu 16.04 为例进行介绍

Supported platforms:
* Ubuntu 16.04 LTS 64 bit
* Windows Servers 64 bit

We'll take Ubuntu as an example in this document.

### 服务
### Serivices

禁止掉默认的 time-syncing daemon(已过期) ，安装 NTPD:

Disable the default time-syncing daemon (timedated) and install NTPD:
```
sudo timedatectl set-ntp false
sudo apt-get -y install ntp
```

### 账号
### Account

在https://wallet.yoyow.org 上注册一个YOYOW主网账号。指南请见：[这里](https://steemit.com/cn/@peterchen145/yoyow-online-wallet-sign-up-tutorial).

Register an account in https://wallet.yoyow.org. Instruction is [here](https://steemit.com/cn/@peterchen145/yoyow-online-wallet-sign-up-tutorial).

得到账号私钥。指南请见：[这里](https://steemit.com/cn/@peterchen145/yoyow-keys-tutorial-yoyow). 我们总共需要3对密钥：

Get the private keys of your account. Instruction is [here](https://steemit.com/cn/@peterchen145/yoyow-keys-tutorial-yoyow). We'll need 3 keys for integration:

* 资金密钥: 管理资金进出
* 零钱密钥：用于收集积分来抵扣手续费
* 备注密钥：用来加密/解密memos

* Active key: for transferring funds out
* Secondary key: for collecting points to pay fees
* Memo key: for encrypting/decrypting memos

### 基本逻辑
### Business Logic

交易所可以使用一个或多个账号处理充值/提现。

An exchange uses an account (or more) for processing deposits/withdrawals.

* 充值：给每个用户分配一个独立的、唯一的标识，如：用户在交易所平台ID，或对该ID进行hash 等。 任意用户在进行充值时，需在`MEMO`上填写该唯一标识。交易所因此可以知道此笔充值来自于哪位客户。
* Deposits: assign a unique identifier to every customer, such as user ID or salted hash of user ID. Every customer transfer funds to the exchange's account with different identifiers as `MEMO` so the exchange can know a deposit is from which customer.

* 提现：从交易所账号中转移资产到用户的账号中，MEMO功能也是必要的，因为用户可能的提现目标为另一家交易所。
* Withdrawals: transfer funds from the exchange's account to an account the customer requested, with an optional memo that the customer may have requested. An optional memo is needed, because some customers may want to withdraw funds to another exchange directly.

## 安装
## Installation

请在([此处](https://github.com/yoyow-org/yoyow-core/releases))下载"最新版"的可执行文件。下载后，解压文件
如：
Please download the "Latest Version" of the executable files ([here](https://github.com/yoyow-org/yoyow-core/releases)). After downloading, extract the files.
```
https://github.com/yoyow-org/yoyow-core/releases/download/v0.2.1-180313/yoyow-node-v0.2.1-ubuntu-20180313.tgz
https://github.com/yoyow-org/yoyow-core/releases/download/v0.2.1-180313/yoyow-client-v0.2.1-ubuntu-20180313.tgz
tar xzf yoyow-node-v0.2.1-ubuntu-20180313.tgz
tar xzf yoyow-client-v0.2.1-ubuntu-20180313.tgz
```
注： 请注意替换最新版本的node 与 client程序 ！

Notes: please remember to replace the latest version of node and client program.

## 启动YOYOW节点
## Starting YOYOW Node

节点程序yoyow_node需要持续运行，可选方案之一为使用`screen`，安装如下：

The node need to be always running. One way to achieve this is to run it with screen. The installation is as follow:
```
sudo apt-get -y install screen
```

通过 `screen`启动yoyow_node:

Start yoyow_node with screen:
```
screen -S yoyow_node
./yoyow_node --rpc-endpoint 127.0.0.1:8090
```

注:
Note:
1. 启动时使用 `--rpc-endpoint` ，我们就可以通过RPC对其进行调用. 在本例中yoyow_node将监听`127.0.0.1` 及 `8090`端口。

Start the node with `--rpc-endpoint` so we can interact with it via RPC call. In the example the node will listen on address `127.0.0.1` and port `8090`.

2. 以下参数表示每账号保留多少条历史记录供查询，默认值是 1000 。
   对交易所来说，如果充值、提现记录较多，可考虑设置成一个较大的值，比如：

The following parameters indicate how many history records are kept for each account. The default value is 1000. For exchanges, if there are more recharge and withdrawal records, consider setting a larger value, e.g.:
```
max-ops-per-account = 1000
```
修改为

modify to
```
max-ops-per-account = 1000000
```
则会保留一百万条数据。更早的数据会从内存中被删除而无法快速查询（但仍然记录在链上）。

It will retain one million data. Earlier data is deleted from memory and cannot be queried quickly (but still recorded on the chain).

3. 以下两个参数会大量减少运行需要的内存，原理是不保存与交易所账户无关的历史数据索引。

The following two parameters will greatly reduce the memory required for the operation. The principle is not to save the historical data index that is not related to the exchange account.
```
track-account = [25638]
partial-operations = true
```

请将 "25638" 替换成你需要的账户数字 ID 。
注： config.ini 里默认 track-account 前面有个“#”符号，需要删除。

Please replace "25638" with the account ID you need. Note: The default track-account in config.ini has a "#" symbol and needs to be deleted.

如果需要监控多个账户，则使用如下配置：

If you need to monitor multiple accounts, use the following configuration:
```
track-account = [25638,25997]
partial-operations = true
```

节点程序会从P2P网络中下载数据，当数据下载完成后，console里会每3秒钟打印一则新的消息，示意如下：

The node then will download blocks from the P2P network. When it's done (in sync), in the console there will be new messages showing every 3 seconds, like these:
```
789216ms th_a       application.cpp:574           handle_block         ] Got block: #355797 00056dd54878c05849e2dcd731c9ee364398b0d2 time: 2017-09-18T19:13:09 latency: 216 ms from: 27662/init8  irreversible: 355787 (-10)
792527ms th_a       application.cpp:574           handle_block         ] Got block: #355798 00056dd613f39f1ad6c0c3fdb00a56c60f44ab1c time: 2017-09-18T19:13:12 latency: 526 ms from: 499381505/yoyo499381505  irreversible: 355788 (-10)
```

如果需要终止节点程序，Ctrl + C 即可，或者发送 SIGINT or SIGTERM 信号给进程。执行后需要等待少许时间，节点会自动停止。

If need to terminate the node, press Ctrl+C, or send SIGINT or SIGTERM signal to the process, then wait for a while, the node will stop by itself.


## 启动 YOYOW Client
## Starting YOYOW Client
### 初次使用
### First Run
启动另一个`screen` ，在其中运行`yoyow_client`程序:

Start another`screen` session, run`yoyow_client` inside:

```
screen -S yoyow_client
./yoyow_client -s ws://127.0.0.1:8090/ -H 127.0.0.1:8091
```

注意:
Note:
1. 使用 `-s` 来指定，连接到的节点程序的IP与端口

Use `-s` option to connect the client to the node.

2. 使用 `-H` 选项来开启一个HTTP-RPC服务，方便我们其他程序进行访问。如：单独处理充值/提现的脚本程序。

Use `-H` option to start a HTTP-RPC service so we can interact with the client from another process, E.G. a script to process funds deposit/withdrawal.

3. yoyow_node 只有完成同步后，才会监听RPC端口，所以请耐心等待 yoyow_node 同步完成。

The node won't start listening on the RPC port until finished replaying, so please be patient in this case.

4. 您可以启动多个client 连接同一个yoyow_node。但请注意不要使用相同的`-H`，会因为端口被占用而监听失败。

You can connect multiple clients to one node, but don't use the same `-H` option.

当第一次运行`yoyow_client`时，当其连接上node程序，会打印：

For the first time when running `yoyow_client`, after connected to the node, it will show:

```
Please use the set_password method to initialize a new wallet before continuing
new >>>
```

我们需要按如下所示设置密码，程序会自动创建一个加密的钱包文件：

So we need to set a password as shown below, and it will be used to create and encrypt a wallet file:

```
new >>> set_password my-password
set_password my-password
null
locked >>>
```

当已经存在钱包文件时，如果我们运行`yoyow_client`程序，它同样会显示`locked >>>`

When there is already a wallet file, if we run `yoyow_client`, it will show `locked >>>` as well.

此时我们需要解锁钱包：

Then we need to unlock the wallet:

```
locked >>> unlock my-password
unlock my-password
null
unlocked >>>
```

导入3对密钥至client，他们会被加密存储于钱包文件中，命令如下：
Now we import the 3 private keys into the client, they will be encrypted then saved in the wallet file. The syntax is:

```
import_key [account_ID] [wif_private_key]
```
如： 您的账号ID是 `123456789`,且拥有了`资金私钥`、`零钱私钥`、`备注私钥`。需要执行`import_key`三次，每次导入1个私钥：

Assume your account ID is `123456789`, and you have the `Active Key`, `Secondary Key` and `Memo Key`, we need to execute `import_key` command 3 times, with one time for one key. For example:


```
unlocked >>> import_key 123456789 5Hqwx3xXMYZ55Pko9nzw34234234nXHcGfNQjNEL23424w7Py
import_key 123456789 5Hqwx3xXMYZ55Pko9nzw34234234nXHcGfNQjNEL23424w7Py
2993104ms th_a       wallet.cpp:820                save_wallet_file     ] saving wallet to file wallet.json
true
unlocked >>> import_key 123456789 5JKoYzQ4sYZoDYwreyrsfsd32466MsCFNoxRE23nExaRi6SY3
import_key 123456789 5JKoYzQ4sYZoDYwreyrsfsd32466MsCFNoxRE23nExaRi6SY3
2993104ms th_a       wallet.cpp:820                save_wallet_file     ] saving wallet to file wallet.json
true
unlocked >>> import_key 123456789 5HttjgBSb45368989etfhsserVtt69cWcExteq6RktpAYXNTT
import_key 123456789 5HttjgBSb45368989etfhsserVtt69cWcExteq6RktpAYXNTT
2993104ms th_a       wallet.cpp:820                save_wallet_file     ] saving wallet to file wallet.json
true
unlocked >>>
```

### 命令： `info`
### Command: `info`

可以通过 `info` 命令，查看网络状态:

We can check network status with `info` command:

```
unlocked >>> info
info
{
  "head_block_num": 377867,
  "head_block_id": "0005c40b41f6d79d762b1ff81c7affc7ae82a894",
  "head_block_time": "2017-09-18T19:37:39",
  "head_block_age": "0 second old",
  "last_irreversible_block_num": 377857,
  "chain_id": "3505e367fe6cde243f2a1c39bd8e58557e23271dd6cbf4b29a8dc8c44c9af8fe",
  "participation": "100.00000000000000000",
  "active_witnesses": [[
  ...
}
```

### 命令： `get_block`
### Command: `get_block`

可通过 `get_block` 查看某个具体块号的相信信息. 格式如下:

We can get the details of a specified block with `get_block` command. The syntax is:

```
get_block [block_number]
```
如:
For example:

```
unlocked >>> get_block 1
```

### 命令： `get_full_account` 
### Command: `get_full_account` 

可通过 `get_full_account` 命令查看某账号详细信息:

We can check the account info with `get_full_account` command:

```
unlocked >>> get_full_account 123456789
get_full_account 123456789
{
  "account": {
    "uid": 123456789,
    ...
  },
  "statistics": {
    "owner": 123456789,
    "total_ops": 30220,
    "prepaid": 0,
    "csaf": 37424828,
    "core_balance": "44672014515",
    "core_leased_in": 0,
    ...
```

注： 对于接入来讲，"statistics" 数据部分比较有用

Note: the "statistics" data is useful for integration.

* "csaf" 是『积分』，通过持有YOYOW产生，可以用来抵扣等额的手续费。YOYOW资产的精度为5，单位为YOYO，所以`"csaf": 37424828` 意味着可抵扣 `374.24828 YOYO`等额的手续费。
* "csaf" means "points", which are generated by holding YOYO and will be used to pay transaction fees. Balances have 5 decimal digits in YOYOW, and the currency is YOYO, so `"csaf": 37424828` means `374.24828 YOYO`.

* "core_balance" 是账号资产的『余额』. `"core_balance": "44672014515"` 意为该账号余额有 `446,720.14515 YOYO`.

* "core_balance" is balance of the account. `"core_balance": "44672014515"` means `446,720.14515 YOYO`.

* 请留意：若返回值内的数字大于`2^32`，将会有引号将其引住。如上例中`core_balance`的值有引号而`csaf`的值没有

* Please be aware that numbers will be surrounded with quotation marks when bigger than `2^32`, as shown above for `core_balance` but not for `csaf`.

### 命令： `transfer`
### Command: `transfer`

 `transfer` 命令可以用来转移资产，格式如下:

We can use the `transfer` command to transfer funds. The syntax is:

```
transfer [from] [to] [amount] YOYO [memo] [broadcast]
```
如:
For example:
```
unlocked >>> transfer 123456789 987654321 1.2345 YOYO "thisismemo" true
```

Note:

* `amount` 最多只能有5位精度。
* `amount` can only have at most 5 decimal digits.
* 如果设置 `broadcast` 为 `true`, 则签名过的交易会被广播至P2P网络；若为 `false` 则用来测试，不广播。
* If setting `broadcast` to `true`, the signed transaction will be broadcast to the P2P network. If using `false` for testing, it will not be broadcast.


### 命令： `get_transaction_id` 
### Command: `get_transaction_id` 

可以使用 `get_transaction_id` 命令得到一笔交易的hash值. 格式如下:

We can use the `get_transaction_id` command to get the hash of a transaction. The syntax is:

```
get_transaction_id [transaction_in_json]
```
这个命令在接入中也是很有用的

This command is useful for integration.

### 命令: `get_relative_account_history` 
### Command: `get_relative_account_history` 

可以使用 `get_relative_account_history` 命令来查看某账号的操作历史，格式如下:

We can use the `get_relative_account_history` command to check our transaction history. The syntax is:

```
get_relative_account_history [account] [operation_type] [start] [limit] [end]
```
如: For example:
```
unlocked >>> get_relative_account_history 123456789 null 1 10 10
unlocked >>> get_relative_account_history 123456789 0 11 10 20
```

注: Note:

* 对于 `operation_type`, 值为 `null` 时，则返回所有操作类型；为 `0` 时可获得所有`transfer`操作.
* For `operation_type`, use `null` to get all operations, use `0` to get `transfer` only.

* 对于 `end`, 值为 `0` 时，可得到最多的最近操作记录.

* For `end`, use `0` to get the most recent records.

* 返回结果的数量会在end - start 范围之内；如果`limit`值比end - start 要小，则返回满足的条件的最新操作记录。

* Result will be in range of `[start, end]`; if `limit` is smaller than the number of records in `[start, end]`, the latest records will be returned.

* 返回结果的排序方式为： 最新的优先

* Result is sorted in "latest first" order.

### 命令： `collect_csaf` 
### Command: `collect_csaf` 

可以使用 `collect_csaf` 命令来收集积分，可用于抵扣各操作的手续费。格式如下：

We can use the `collect_csaf` command to collect points which will be needed to pay transaction fees. The syntax is:

```
collect_csaf [from_account] [to_account] [amount] YOYO [broadcast]
```
如: For example:
```
unlocked >>> collect_csaf 123456789 123456789 10 YOYO true
```

注:
Note:

* 如果该账号内有一定数量的YOYO，它将随着时间自动积累积分。积累的速度与账号余额间呈线性关系。通常对于交易所而言，积累的积分足够支付日常手续费用。
* If you have some YOYO in your account, it will accumulate points as time goes by. The accumulation speed has a linear relationship with account balance. Usually for exchanges the accumulated points should be enough to pay transaction fees.
* 积分需要通过该命令领取后才能使用
* Points need to be collected before can be used to pay transaction fees.
* 尽管在底层实现上，账号里的资产（如余额、零钱）同样可以用来支付手续费，但是当前`yoyow_client`的实现上会只尝试进行积分抵扣手续费（raw-transaction signing除外）.如果账号中没有足够的积分，多数的命令会失败。所以保持账号中一定数量的积分是很有必要的。
* Although funds in balance can be used to pay transaction fees as well in the back end, current implementation of `yoyow_client` will only try to pay fees with points (except raw-transaction signing). If you don't have enough points in the account, most commands will fail. So it's important to keep a certain amount of points in the account.

### 如何关闭
### How to Close

在Ubuntu下，可以Ctrl + D 来关闭client

Press Ctrl+D if the client is running in Ubuntu.

## 通过 HTTP-RPC 接入Client
## Accessing the Client via HTTP-RPC


当 HTTP-RPC 开启的时候，我们可以通过HTTP-RPC 来连接client。所有的上述命令都是可用的，如：

When HTTP-RPC is enabled, we can access the client via HTTP-RPC call. All commands are usable. For example:

```
curl -d '{"jsonrpc": "2.0", "method": "info", "params": [], "id": 1}' http://127.0.0.1:8091/rpc
curl -d '{"jsonrpc": "2.0", "method": "transfer", "params": [123456789,123456789,"1","YOYO",null,true], "id": 1}' http://127.0.0.1:8091/rpc
curl -d '{"jsonrpc": "2.0", "method": "get_relative_account_history", "params": [123456789,0,1,10,10], "id": 1}' http://127.0.0.1:8091/rpc
```

注: Note:

* 使用 `http` 协议
* Use `http` as protocol

* 请求 `/rpc` 路径，而不是 `/`
* Request `/rpc` but not `/`

* 与交互式CLI不同，HTTP-RPC调用采用json格式
* Not like the interactive CLI, the results of HTTP-RPC calls are formatted in json

* 在所有request中，amount 的精度都是5；在response里，amount没有小数点，其数值被乘以了`10^5`
* In the request, amounts have 5 decimal digits; in the response, amounts have no decimal point. Instead, amounts are multiplied by `10^5`.

* 金额的传入，金额建议使用字符串。建议使用普通的小数格式，比如"12345.6789"，如果使用科学计数法的格式比如1.23456789E4这种可能会有问题
* When entering the amount, it is recommended to use a string for the amount. It is recommended to use the normal decimal format, such as "12345.6789". Problems may occur if using the scientific notation format such as 1.23456789E4.


## 处理充值
## Processing Deposits

### 检查节点状态
### Checking Node Status

通过`info`命令，得到`last_irreversible_block_num`.在这个值之前的区块才是可信区块

Get the `last_irreversible_block_num` data with the``info` command (via HTTP-RPC). Only blocks earlier than this block is reliable.

### 检查账号历史
### Checking Account History

1. 首先，使用 `get_relative_account_history` 命令/API 来得到最新的 sequence number:

Firstly, Use `get_relative_account_history` command/API to get the latest sequence number:

```
curl -d '{"jsonrpc": "2.0", "method": "get_relative_account_history", "params": [123456789,0,0,1,0], "id": 1}' http://127.0.0.1:8091/rpc
```

在返回值中，如果`response["result"]`为空，则意为该账号下目前没有充值进来；若不为空，则，可以取`response["result"][0]["sequence"]`作为`maximum_sequence`。

If the result, `response["result"]` is empty, it means we have no deposit at all. If it's not empty, we get `response["result"][0]["sequence"]` as `maximum_sequence`.

如果`maximum_sequence`比上次存储的最新sequence值更大，则意为有最新的充值记录需要处理

If `maximum_sequence` is bigger than the last sequence we've saved, it means there are new records to be processed.


2. 使用`get_relative_account_history`命令/API 来检查最新记录。如：
如自行记录的sequence 值为100，`maximum_sequence` 为200，则我们可以从`101`开始,请求100个记录，至`200`结束，参考命令如下：

Use `get_relative_account_history` command/API to check for new records. For example, if last recorded sequence is 100, and `maximum_sequence` is 200, we can check from `101`, for at most 100 records, to `200`:

```
curl -d '{"jsonrpc": "2.0", "method": "get_relative_account_history", "params": [123456789,0,101,100,200], "id": 1}' http://127.0.0.1:8091/rpc
```

返回值中, `result=response["result"]`, 则result为数组. 若数组为空，则意味着没有最新的充值请求。若不为空, 则数组的第N个元素 `result[N]` 格式应该如下:

The returned result, `result=response["result"]`, is an array. If the array is empty, it means no new deposit. If it's not empty, the N'th record `result[N]` should be like:

```
    {
      "memo": "a1b2c3d4",
      "description": "Transfer 100 YOYO from 204501630 to 123456789 -- Memo: a1b2c3d4   (Fee: 0.20898 YOYO)",
      "sequence": 101,
      "op": {
        "op": [
          0,
          {
            "fee": {
              "total": {
                "amount": 20898,
                "asset_id": 0
              },
              "options": {
                "from_balance": {
                  "amount": 20898,
                  "asset_id": 0
                }
              }
            },
            "from": 204501630,
            "to": 123456789,
            "amount": {
              "amount": 10000000,
              "asset_id": 0
            },
            "memo": {
              "from": "YYW6U528P71X6V87765245356aPBPpDwwRp7urUiXYtFLHmrXRsN3u",
              "to": "YYW5eA89yqwerhdfghrjtr3452376trtyU6LD7a1kmvwYa5h51rDxr",
              "nonce": "3457645755345345",
              "message": "0938457345937abcdef3098945"
            },
            "extensions": {
              "from_balance": {
                "amount": 10000000,
                "asset_id": 0
              },
              "to_balance": {
                "amount": 10000000,
                "asset_id": 0
              }
            }
          }
        ],
        "result": [
          0,
          {
          }
        ],
        "block_timestamp": "2017-09-08T11:14:15",
        "block_num": 223355,
        "trx_in_block": 0,
        "op_in_trx": 0,
        "virtual_op": 8795
      }
    },
```

* 取 `result[N]["op"]["block_num"]`, 若比 `last_irreversible_block_num`小, 意味着是可信的, 需要被处理.
* Get `result[N]["op"]["block_num"]`, if it's smaller than `last_irreversible_block_num`, then it's reliable, and need to be processed.

* 取 `result[N]["op"]["op"][0]`, 如 == `0`, 则是一个transfer请求. (实际上肯定会等于0，因为请求参数里已约定只取transfer记录)
* Get `result[N]["op"]["op"][0]`, if it's `0`, then it's a transfer. (Although it should always be 0 if we request transfers only)

* 取 `result[N]["op"]["op"][1]["to"]`, 验证是否与自身account ID 相同。若相同，即为一个充值请求。
* Get `result[N]["op"]["op"][1]["to"]`, if it's the same as our account ID, then this transfer is a deposit.

* 取 `result[N]["op"]["op"][1]["amount"]["asset_id"]` 验证其是否 ==  `0`, 若是，即表示该资产为`YOYO`
* Check if `result[N]["op"]["op"][1]["amount"]["asset_id"]` is `0`. If yes, it means it's `YOYO` asset.

* 取 `result[N]["op"]["op"][1]["amount"]["amount"]`, 该值为充值数量. 切记：该数字精度为5.
* Get `result[N]["op"]["op"][1]["amount"]["amount"]`, it's the amount. Remember to add the decimal point (5 digits).

* 取 `result[N]["memo"]`, 为该转账记录的MEMO信息。此处已经被解密过了，它可以用来作为充值客户的识别符。
* Get `result[N]["memo"]`, it is the memo info of the transfer records. It should have been decrypted already, and it can be an identifier to the customer.

* 保存 `result[N]["sequence"]` 作为最新的sequence序号，下一次循环时需使用。
* Save `result[N]["sequence"]` as the latest processed sequence number for use in the next loop. 

* 保存 `result[N]["op"]["trx_in_block"]` 留作后续使用。
* Save `result[N]["op"]["trx_in_block"]` for later use.

* 使用 `get_block` 命令/API 来获取此次转账的txid ,如下：(记得修改params值为 `block_num`):
* Use `get_block` command/API to get this transfer's transaction ID/Hash (change the parameter to the `block_num`):

```
curl -d '{"jsonrpc": "2.0", "method": "get_block", "params": [160000], "id": 1}' http://127.0.0.1:8091/rpc
```
  记录返回值为 `new_response`, 保存 `new_response["result"]["transaction_ids"][trx_in_block]` 作为本次充值的txid 留作后用.
  
  From the response `new_response`, save `new_response["result"]["transaction_ids"][trx_in_block]` as the transaction ID/hash of this deposit for future use.

注: Note:

* 为了保证可以正确解密MEMO，client 需要出于 `unlocked`状态。同时，memo private key 需要已导入至钱包中
* To be able to decrypt memo, the client need to be `unlocked`, and the memo private key need to be in the wallet.

## 处理提现请求
## Process Withdrawal Requests

### 检查节点状态
### Check Node Status

安全起见，我们只在节点状态正常的情况下处理提现。
通过`info`命令/API来检验。

To be safe, we can only process withdrawal requests when node status is normal. Check with the `info` command/API.

* `head_block_time` 应当在15秒以内
* `head_block_time` should be no more than 15 seconds old

* `participation` 应大于 `80`, 意味着80%的区块生产者在线且状态正常。
* `participation` should be more than `80`, which means 80% of block producers are online and at normal statuses.

### 检查账号资产与积分
### Checking Account Balance and Points


使用 `get_full_account` 命令/API.

Check with the `get_full_account` command/API.

如果积分不足以支付网络手续费，请使用`collect_csaf`命令/API 领取更多积分。

If points are not enough for paying transaction fee, collect more points with `collect_csaf` command/API.


### 发送资产
### Sending Funds

1. 使用 `transfer` 命令/API 来发送资产.

Use the `transfer` command/API to send out funds.

注: 请注意资产精度.

Note: take care of decimal digits.

保存返回的json值供后续使用.

Save the returned json for future use.

2. 获取txid通过 `get_transaction_id` 命令/API, 保存供后续使用.

Get the transaction ID/hash with `get_transaction_id` command/API, save it for future use.

### Re-check / Follow-up

类似于充值的步骤，当发现一个新的transfer，保存txid,block num等信息供后续使用。

Similar to the deposit processing steps, when found an outgoing transfer, save the transaction ID/hash, block number and etc. for future use.

### 故障排查
### Trouble Shooting

每笔交易都有`expiration`字段。如果某笔交易因为某种原因，没有被打包到任意块中，且不可逆区块(`last_irreversible_block_num`)的时间戳比交易的`expiration`字段还要晚。则该笔交易不会打包在当前这条链上，此时尝试重新转账是安全的。

Every transaction has an `expiration` field. If the transaction hasn't been included in any block for some reason, and the timestamp of block whose number is `last_irreversible_block_num` is later than the `expiration` field of the transaction, the transaction won't be included in current chain, so it's safe to try to transfer again.


## 示例代码
## Sample Code

* Ruby: https://github.com/yoyow-org/yoyow-core/blob/master/scripts/exchange.rb
