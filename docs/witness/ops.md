# 见证人相关操作

## 查询
### 获取见证人信息。
```
# get_witness <owner_account>
get_witness 132826789
```
详见：[get_witness](../api/wallet_api.html#get-witness)

### 查询见证人清单
```
# list_witnesses <lower_bound> <limit> <order_by>
list_witnesses 0 5 1
```
详见：[list_witnesses](../api/wallet_api.html#list-witnesses)

## 创建见证人
```
# create_witness <owner_account> <block_signing_key> <pledge_amount> <pledge_asset_symbol> <url> <broadcast>
create_witness 223331844 YYW1111111111111111111111111111111114T1Anm 1000000 YOYO "http://www.yoyow.org" true
```
详见：[create_witness](../api/wallet_api.html#create-witness)

## 更新见证人信息
```
# update_witness <lower_bound> <limit> <ops>
update_witness 0 5 1
```
详见：[update_witness](../api/wallet_api.html#update-witness)

## 见证人投票
```
# update_witness_votes <voting_account> <witnesses_to_add> <witnesses_to_remove> <broadcast>
update_witness_votes 250926091 [abit] [] true
```
详见：[update_witness_votes](../api/wallet_api.html#update-witness-votes)
