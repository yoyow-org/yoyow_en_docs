# Instructions for Exchanges

We here describe how to integrate your exchange with the YOYOW blockchain step-by-step.

## Preparation

### Hardware

The recommended hardware by now is a VPS with 2GB of RAM and 20GB HDD. Single core is OK.

Supported platforms:
* Ubuntu 16.04 LTS 64 bit
* Windows Servers 64 bit

We'll take Ubuntu as an example in this document.

### Serivices

Disable the default time-syncing daemon (timedated) and install NTPD:
```
sudo timedatectl set-ntp false
sudo apt-get -y install ntp
```

### Account

Register an account in https://wallet.yoyow.org. Instruction is [here](https://steemit.com/cn/@peterchen145/yoyow-online-wallet-sign-up-tutorial).

Get the private keys of your account. Instruction is [here](https://steemit.com/cn/@peterchen145/yoyow-keys-tutorial-yoyow). We'll need 3 keys for integration:

* Active key: for transferring funds out
* Secondary key: for collecting points to pay fees
* Memo key: for encrypting/decrypting memos

### Business Logic

An exchange uses an account (or more) for processing deposits/withdrawals.

* Deposits: assign a unique identifier to every customer, such as user ID or salted hash of user ID. Every customer transfer funds to the exchange's account with different identifiers as `MEMO` so the exchange can know a deposit is from which customer.

* Withdrawals: transfer funds from the exchange's account to an account the customer requested, with an optional memo that the customer may have requested. An optional memo is needed, because some customers may want to withdraw funds to another exchange directly.

## Installation

Please download the "Latest Version" of the executable files ([here](https://github.com/yoyow-org/yoyow-core/releases)). After downloading, extract the files.
For example,
```
https://github.com/yoyow-org/yoyow-core/releases/download/v0.2.1-180313/yoyow-node-v0.2.1-ubuntu-20180313.tgz
https://github.com/yoyow-org/yoyow-core/releases/download/v0.2.1-180313/yoyow-client-v0.2.1-ubuntu-20180313.tgz
tar xzf yoyow-node-v0.2.1-ubuntu-20180313.tgz
tar xzf yoyow-client-v0.2.1-ubuntu-20180313.tgz
```

Notes: please remember to replace the latest version of node and client program.

## Starting YOYOW Node

The node need to be always running. One way to achieve this is to run it with screen. The installation is as follow:
```
sudo apt-get -y install screen
```

Start yoyow_node with screen:
```
screen -S yoyow_node
./yoyow_node --rpc-endpoint 127.0.0.1:8090
```

Note:
1. Start the node with `--rpc-endpoint` so we can interact with it via RPC call. In the example the node will listen on address `127.0.0.1` and port `8090`.

2. The following parameters indicate how many history records are kept for each account. The default value is 1000. For exchanges, if there are more recharge and withdrawal records, consider setting a larger value, e.g.:
```
max-ops-per-account = 1000
```
modify to
```
max-ops-per-account = 1000000
```
It will retain one million data. Earlier data is deleted from memory and cannot be queried quickly (but still recorded on the chain).

3. The following two parameters will greatly reduce the memory required for the operation. The principle is not to save the historical data index that is not related to the exchange account.
```
track-account = [25638]
partial-operations = true
```

Please replace "25638" with the account ID you need. 
Note: The default track-account in config.ini has a "#" symbol and needs to be deleted.

If you need to monitor multiple accounts, use the following configuration:
```
track-account = [25638,25997]
partial-operations = true
```

The node then will download blocks from the P2P network. When it's done (in sync), in the console there will be new messages showing every 3 seconds, like these:
```
789216ms th_a       application.cpp:574           handle_block         ] Got block: #355797 00056dd54878c05849e2dcd731c9ee364398b0d2 time: 2017-09-18T19:13:09 latency: 216 ms from: 27662/init8  irreversible: 355787 (-10)
792527ms th_a       application.cpp:574           handle_block         ] Got block: #355798 00056dd613f39f1ad6c0c3fdb00a56c60f44ab1c time: 2017-09-18T19:13:12 latency: 526 ms from: 499381505/yoyo499381505  irreversible: 355788 (-10)
```

If need to terminate the node, press Ctrl+C, or send SIGINT or SIGTERM signal to the process, then wait for a while, the node will stop by itself.


## Starting YOYOW Client
### First Run

Start another`screen` session, run`yoyow_client` inside:

```
screen -S yoyow_client
./yoyow_client -s ws://127.0.0.1:8090/ -H 127.0.0.1:8091
```

Note:
1. Use `-s` option to connect the client to the node.

2. Use `-H` option to start a HTTP-RPC service so we can interact with the client from another process, E.G. a script to process funds deposit/withdrawal.

3. The node won't start listening on the RPC port until finished replaying, so please be patient in this case.

4. You can connect multiple clients to one node, but don't use the same `-H` option.

For the first time when running `yoyow_client`, after connected to the node, it will show:

```
Please use the set_password method to initialize a new wallet before continuing
new >>>
```

So we need to set a password as shown below, and it will be used to create and encrypt a wallet file:

```
new >>> set_password my-password
set_password my-password
null
locked >>>
```

When there is already a wallet file, if we run `yoyow_client`, it will show `locked >>>` as well.

Then we need to unlock the wallet:

```
locked >>> unlock my-password
unlock my-password
null
unlocked >>>
```

Now we import the 3 private keys into the client, they will be encrypted then saved in the wallet file. The syntax is:

```
import_key [account_ID] [wif_private_key]
```
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

### Command: `info`

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

### Command: `get_block`

We can get the details of a specified block with `get_block` command. The syntax is:

```
get_block [block_number]
```
For example:

```
unlocked >>> get_block 1
```

### Command: `get_full_account` 

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

Note: the "statistics" data is useful for integration.

* "csaf" means "points", which are generated by holding YOYO and will be used to pay transaction fees. Balances have 5 decimal digits in YOYOW, and the currency is YOYO, so `"csaf": 37424828` means `374.24828 YOYO`.

* "core_balance" is balance of the account. `"core_balance": "44672014515"` means `446,720.14515 YOYO`.

* Please be aware that numbers will be surrounded with quotation marks when bigger than `2^32`, as shown above for `core_balance` but not for `csaf`.

### Command: `transfer`

We can use the `transfer` command to transfer funds. The syntax is:

```
transfer [from] [to] [amount] YOYO [memo] [broadcast]
```
For example:
```
unlocked >>> transfer 123456789 987654321 1.2345 YOYO "thisismemo" true
```

Note:

* `amount` can only have at most 5 decimal digits.
* If setting `broadcast` to `true`, the signed transaction will be broadcast to the P2P network. If using `false` for testing, it will not be broadcast.


### Command: `get_transaction_id` 

We can use the `get_transaction_id` command to get the hash of a transaction. The syntax is:

```
get_transaction_id [transaction_in_json]
```
This command is useful for integration.

### Command: `get_relative_account_history` 

We can use the `get_relative_account_history` command to check our transaction history. The syntax is:

```
get_relative_account_history [account] [operation_type] [start] [limit] [end]
```
For example:
```
unlocked >>> get_relative_account_history 123456789 null 1 10 10
unlocked >>> get_relative_account_history 123456789 0 11 10 20
```

Note:

* For `operation_type`, use `null` to get all operations, use `0` to get `transfer` only.

* For `end`, use `0` to get the most recent records.

* Result will be in range of `[start, end]`; if `limit` is smaller than the number of records in `[start, end]`, the latest records will be returned.

* Result is sorted in "latest first" order.

### Command: `collect_csaf` 

We can use the `collect_csaf` command to collect points which will be needed to pay transaction fees. The syntax is:

```
collect_csaf [from_account] [to_account] [amount] YOYO [broadcast]
```
For example:
```
unlocked >>> collect_csaf 123456789 123456789 10 YOYO true
```

Note:

* If you have some YOYO in your account, it will accumulate points as time goes by. The accumulation speed has a linear relationship with account balance. Usually for exchanges the accumulated points should be enough to pay transaction fees.
* Points need to be collected through this command before can be used to pay transaction fees.
* Although funds in balance can be used to pay transaction fees as well in the back end, current implementation of `yoyow_client` will only try to pay fees with points (except raw-transaction signing). If you don't have enough points in the account, most commands will fail. So it's important to keep a certain amount of points in the account.

### How to Close

Press Ctrl+D if the client is running in Ubuntu.

## Accessing the Client via HTTP-RPC

When HTTP-RPC is enabled, we can access the client via HTTP-RPC call. All commands are usable. For example:

```
curl -d '{"jsonrpc": "2.0", "method": "info", "params": [], "id": 1}' http://127.0.0.1:8091/rpc
curl -d '{"jsonrpc": "2.0", "method": "transfer", "params": [123456789,123456789,"1","YOYO",null,true], "id": 1}' http://127.0.0.1:8091/rpc
curl -d '{"jsonrpc": "2.0", "method": "get_relative_account_history", "params": [123456789,0,1,10,10], "id": 1}' http://127.0.0.1:8091/rpc
```

Note:

* Use `http` as protocol

* Request `/rpc` but not `/`

* Not like the interactive CLI, the results of HTTP-RPC calls are formatted in json

* In the request, amounts have 5 decimal digits; in the response, amounts have no decimal point. Instead, amounts are multiplied by `10^5`.

* When entering the amount, it is recommended to use a string for the amount. It is recommended to use the normal decimal format, such as "12345.6789". Problems may occur if using the scientific notation format such as 1.23456789E4.


## Processing Deposits

### Checking Node Status

Get the `last_irreversible_block_num` data with the``info` command (via HTTP-RPC). Only blocks earlier than this block is reliable.

### Checking Account History

1. Firstly, Use `get_relative_account_history` command/API to get the latest sequence number:

```
curl -d '{"jsonrpc": "2.0", "method": "get_relative_account_history", "params": [123456789,0,0,1,0], "id": 1}' http://127.0.0.1:8091/rpc
```

If the result, `response["result"]` is empty, it means we have no deposit at all. If it's not empty, we get `response["result"][0]["sequence"]` as `maximum_sequence`.

If `maximum_sequence` is bigger than the last sequence we've saved, it means there are new records to be processed.


2. Use `get_relative_account_history` command/API to check for new records. For example, if last recorded sequence is 100, and `maximum_sequence` is 200, we can check from `101`, for at most 100 records, to `200`. Refer to the following commands:

```
curl -d '{"jsonrpc": "2.0", "method": "get_relative_account_history", "params": [123456789,0,101,100,200], "id": 1}' http://127.0.0.1:8091/rpc
```

In the returned result, `result=response["result"]`, so result is an array. If the array is empty, it means no new deposit. If it's not empty, the N'th record `result[N]` should be like:

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

* Get `result[N]["op"]["block_num"]`, if it's smaller than `last_irreversible_block_num`, then it's reliable, and need to be processed.

* Get `result[N]["op"]["op"][0]`, if it's `0`, then it's a transfer. (Although it should always be 0 if we request transfers only)

* Get `result[N]["op"]["op"][1]["to"]`, if it's the same as our account ID, then this transfer is a deposit.

* Check if `result[N]["op"]["op"][1]["amount"]["asset_id"]` is `0`. If yes, it means it's `YOYO` asset.

* Get `result[N]["op"]["op"][1]["amount"]["amount"]`, it's the amount. Remember to add the decimal point (5 digits).

* Get `result[N]["memo"]`, it is the memo info of the transfer records. It should have been decrypted already, and it can be an identifier to the customer.

* Save `result[N]["sequence"]` as the latest processed sequence number for use in the next loop. 

* Save `result[N]["op"]["trx_in_block"]` for later use.

* Use `get_block` command/API to get this transfer's transaction ID/Hash (change the parameter to the `block_num`):

```
curl -d '{"jsonrpc": "2.0", "method": "get_block", "params": [160000], "id": 1}' http://127.0.0.1:8091/rpc
```

  From the response `new_response`, save `new_response["result"]["transaction_ids"][trx_in_block]` as the transaction ID/hash of this deposit for future use.

Note:

* To be able to decrypt memo, the client need to be `unlocked`, and the memo private key need to be in the wallet.

## Process Withdrawal Requests

### Check Node Status

To be safe, we can only process withdrawal requests when node status is normal. 
Check with the `info` command/API.

* `head_block_time` should be no more than 15 seconds old

* `participation` should be more than `80`, which means 80% of block producers are online and at normal statuses.

### Checking Account Balance and Points

Check with the `get_full_account` command/API.

If points are not enough for paying transaction fee, collect more points with `collect_csaf` command/API.

### Sending Funds

1. Use the `transfer` command/API to send out funds.

Note: pay attention to the decimal digits.

Save the returned json for future use.

2. Get the transaction ID/hash with `get_transaction_id` command/API, save it for future use.

### Re-check / Follow-up

Similar to the deposit processing steps, when found an outgoing transfer, save the transaction ID/hash, block number and etc. for future use.

### Trouble Shooting

Every transaction has an `expiration` field. If the transaction hasn't been included in any block for some reason, and the timestamp of block whose number is `last_irreversible_block_num` is later than the `expiration` field of the transaction, the transaction won't be included in current chain, so it's safe to try to transfer again.

## Sample Code

* Ruby: https://github.com/yoyow-org/yoyow-core/blob/master/scripts/exchange.rb
