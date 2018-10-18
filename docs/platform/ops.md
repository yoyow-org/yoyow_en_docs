# 平台相关操作

以下相关操作命令可以在yoyow_client中交互命令行中执行

## 获取平台数量
```
get_platform_count
```
详见：[get_platform_count](../api/wallet_api.html#get-platform-count)

## 获取平台信息
```
# get_platform <owner_account>
get_platform 250926091
```
详见：[get_platform](../api/wallet_api.html#get-platform)

## 查询平台清单
```
# list_platforms <lowerbound> <limit> <order_by>
list_platforms 0 5 1
```
详见：[list_committee_members](../api/wallet_api.html#list-platforms)

## 创建平台
```
# create_platform <owner_account> <name> <pledge_amount> <pledge_asset_symbol> <url> <extra_data> <broadcast>
create_platform 223331844 yoyow.club 10000 YOYO "http://yoyow.club" null true
```
详见：[create_platform](../api/wallet_api.html#create-platform)

## 更新平台信息
```
# update_platform <platform_account> <name> <pledge_amount> <pledge_asset_symbol> <url> <extra_data> <broadcast>
update_platform 223331844 NUUUU null null "http://www.example.com" "http://www.example.com" true
```
详见：[update_platform](../api/wallet_api.html#update-platform)

## 为平台投票
```
# update_platform_votes <voting_account> <platforms_to_add> <platforms_to_remove> <broadcast>
update_platform_votes 250926091 ["223331844"] [] true
```
详见：[update_platform_votes](../api/wallet_api.html#update-platform-votes)

## 对平台授权
```
# account_auth_platform <account> <platform_owner> <broadcast>
account_auth_platform 250926091 223331844 true
```
详见：[account_auth_platform](../api/wallet_api.html#account-auth-platform)

## 取消平台授权
```
# account_cancel_auth_platform <account> <platform_owner> <broadcast>
account_cancel_auth_platform 250926091 223331844 true
```
详见：[account_cancel_auth_platform](../api/wallet_api.html#account-cancel-auth-platform)
