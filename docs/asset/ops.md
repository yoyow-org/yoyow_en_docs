# 资产相关操作

以下相关操作命令可以在yoyow_client中交互命令行中执行

## 普通转账

```
# transfer <from> <to> <amount> <asset_symbol> <memo> <broadcast>
transfer 250926091 209414065 100 YOYO "memo" true
```
详见：[transfer](../api/wallet_api.html#transfer)

## 查询

### 查询账户YOYO余额
该函数只返回账户拥有的YOYO数
```
# list_account_balances <account> 
list_account_balances 250926091
```
详见：[list_account_balances](../api/wallet_api.html#list-account-balances)


### 查询账户所有资产余额
该函数暂时没有钱包的接口，只能调用节点的api，比如使用wscat链接websocket端口
```
{"id":1, "method":"call", "params":[0,"get_account_balances",["250926091", []]]}
```
详见：[get_account_balances](../api/node_api.html#get-account-balances)

## 积分相关

### 查看积分
get_full_account 可以查看积分，在返回结果中的"statistics"中，返回有csaf，core_balance，prepaid，分别对应积分，余额，零钱
```
# get_full_account <account_name_or_id>
get_full_account 250926091
```

### 领取到当前时间的积分
```
# collect_csaf <from> <to> <amount> <asset_symbol> <broadcast>
collect_csaf 250926091 209414065 100 YOYO true
```
详见：[collect_csaf](../api/wallet_api.html#collect-csaf-with-time)

### 领取积累到指定时间的积分
```
# collect_csaf_with_time <from> <to> <amount> <asset_symbol> <time> <broadcast>
collect_csaf_with_time 250926091 209414065 100 YOYO "2018-04-16T02:44:00" true
```
详见：[collect_csaf_with_time](../api/wallet_api.html#collect-csaf-with-time)


## 用户发行资产
用户发行资产有两个核心的操作，创建资产（create_asset）和发行资产（issue_asset），刚创建完成的资产并不能使用（没有流通），需要issue给指定的账户才可以使用。

### 获取资产信息
```
# get_asset <asset_name_or_id>
get_asset 3
```
详见：[get_asset](../api/wallet_api.html#get-asset)

### 查询资产清单
```
# list_assets <lower_bound_symbol> <limit> 
list_assets YOYO 4
```
详见：[list_assets](../api/wallet_api.html#list-assets)

### 创建资产
```
# create_asset <issuer> <symbol> <precision> <common> <initial_supply> <broadcast>
create_asset 250926091 TOTOTO 4 {"max_supply":300000,"market_fee_percent":0,"max_market_fee":0,"issuer_permissions":4} 200000 true
```
详见：[create_asset](../api/wallet_api.html#create-asset)

### 发行资产
```
# issue_asset <to_account> <amount> <symbol> <memo> <broadcast>
issue_asset 250926091 10000 WOWO memo true
```
详见：[issue_asset](../api/wallet_api.html#issue-asset)

### 更新资产
```
# update_asset <symbol> <new_issuer> <new_options> <broadcast>
update_asset WOWO null {"max_supply":"2000000000"} true
```
详见：[update_asset](../api/wallet_api.html#update-asset)

### 销毁资产
```
# reserve_asset <from> <amount> <symbol> <broadcast>
reserve_asset 250926091 1000 WOWO true
```
详见：[reserve_asset](../api/wallet_api.html#reserve-asset)