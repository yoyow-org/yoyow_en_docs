# 理事会相关操作
# Committee Related Operations
以下相关操作命令可以在yoyow_client中交互命令行中执行

The following related operation commands can be executed in the interactive command line in yoyow_client

## 创建理事会账户
## Creating Committee Account

```
# create_committee_member <owner_account> <pledge_amount> <pledge_asset_symbol> <url> <broadcast>
create_committee_member 223331844 1000 YOYO "http://www.yoyow.org" true
```
For details, please refer to: [create_committee_member](../api/wallet_api.html#create-committee-member)


## 更新理事会账户
## Updating Committee Account

```
# update_committee_member <committee_member_account> <pledge_amount> <pledge_asset_symbol> <url> <broadcast>
update_committee_member 223331844 1000 YOYO "http://demo.yoyow.org" true
```
For details, please refer to: [update_committee_member](../api/wallet_api.html#update-committee-member)

## 投票给理事会
## Voting for Committee

```
# update_committee_member_votes <voting_account> <committee_members_to_add> <committee_members_to_remove> <broadcast>
update_committee_member_votes 223331844 ["init1"], [],  true
```
For details, please refer to: [update_committee_member_votes](../api/wallet_api.html#update-committee-member-votes)


## 获取理事会成员信息
## Getting Info of Committee Members
```
# get_committee_member <owner_account> 
get_committee_member 25997
```
For details, please refer to: [get_committee_member](../api/wallet_api.html#get-committee-member)


## 查看当前候选历史
## Checking History of Current Candidates 
```
# list_committee_members <lower_bound> <limit> <order_by>
list_committee_members 0 5 1
```
For details, please refer to: [list_committee_members](../api/wallet_api.html#list-committee-members)

## 查看当前还未执行理事会提案
## Checking the Current Committee Proposals Not Implemented

```
list_committee_proposals 
```
For details, please refer to: [list_committee_members](../api/wallet_api.html#list-committee-proposals)
