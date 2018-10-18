# 理事会相关操作

以下相关操作命令可以在yoyow_client中交互命令行中执行

## 创建理事会账户
```
# create_committee_member <owner_account> <pledge_amount> <pledge_asset_symbol> <url> <broadcast>
create_committee_member 223331844 1000 YOYO "http://www.yoyow.org" true
```
详见：[create_committee_member](../api/wallet_api.html#create-committee-member)


## 更新理事会账户

```
# update_committee_member <committee_member_account> <pledge_amount> <pledge_asset_symbol> <url> <broadcast>
update_committee_member 223331844 1000 YOYO "http://demo.yoyow.org" true
```
详见：[update_committee_member](../api/wallet_api.html#update-committee-member)

## 投票给理事会

```
# update_committee_member_votes <voting_account> <committee_members_to_add> <committee_members_to_remove> <broadcast>
update_committee_member_votes 223331844 ["init1"], [],  true
```
详见：[update_committee_member_votes](../api/wallet_api.html#update-committee-member-votes)


## 获取理事会成员信息

```
# get_committee_member <owner_account> 
get_committee_member 25997
```
详见：[get_committee_member](../api/wallet_api.html#get-committee-member)


## 查看当前候选历史
```
# list_committee_members <lower_bound> <limit> <order_by>
list_committee_members 0 5 1
```
详见：[list_committee_members](../api/wallet_api.html#list-committee-members)

## 查看当前还未执行理事会提案

```
list_committee_proposals 
```
详见：[list_committee_members](../api/wallet_api.html#list-committee-proposals)

## 发提案
【待补充】