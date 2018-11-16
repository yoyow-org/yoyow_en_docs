# 平台相关操作
# Platform Related Operations

以下相关操作命令可以在yoyow_client中交互命令行中执行

The following related operation commands can be executed in the interactive command line in yoyow_client

## 获取平台数量
## Getting Platform Number
```
get_platform_count
```
For details, please refer to: [get_platform_count](../api/wallet_api.html#get-platform-count)

## 获取平台信息
## Getting Platform Info
```
# get_platform <owner_account>
get_platform 250926091
```
For details, please refer to: [get_platform](../api/wallet_api.html#get-platform)

## 查询平台清单
## Checking Platform List
```
# list_platforms <lowerbound> <limit> <order_by>
list_platforms 0 5 1
```
For details, please refer to: [list_committee_members](../api/wallet_api.html#list-platforms)

## 创建平台
## Creating Platforms
```
# create_platform <owner_account> <name> <pledge_amount> <pledge_asset_symbol> <url> <extra_data> <broadcast>
create_platform 223331844 yoyow.club 10000 YOYO "http://yoyow.club" null true
```
For details, please refer to: [create_platform](../api/wallet_api.html#create-platform)

## 更新平台信息
## Undating Platform Info
```
# update_platform <platform_account> <name> <pledge_amount> <pledge_asset_symbol> <url> <extra_data> <broadcast>
update_platform 223331844 NUUUU null null "http://www.example.com" "http://www.example.com" true
```
For details, please refer to: [update_platform](../api/wallet_api.html#update-platform)

## 为平台投票
## Voting for Platforms
```
# update_platform_votes <voting_account> <platforms_to_add> <platforms_to_remove> <broadcast>
update_platform_votes 250926091 ["223331844"] [] true
```
For details, please refer to: [update_platform_votes](../api/wallet_api.html#update-platform-votes)

## 对平台授权
## Authorizing Platforms
```
# account_auth_platform <account> <platform_owner> <broadcast>
account_auth_platform 250926091 223331844 true
```
For details, please refer to: [account_auth_platform](../api/wallet_api.html#account-auth-platform)

## 取消平台授权
## Canceling the Platform Authorization
```
# account_cancel_auth_platform <account> <platform_owner> <broadcast>
account_cancel_auth_platform 250926091 223331844 true
```
For details, please refer to: [account_cancel_auth_platform](../api/wallet_api.html#account-cancel-auth-platform)
