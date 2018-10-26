# 见证人相关操作

## 见证人节点搭建
搭建见证人的完整教程可以参考[YOYOW见证人教程(Ubuntu)](https://mp.weixin.qq.com/s/l4KfKtUUfaCEp9ykIbIByA)

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

## 见证人离线

```
# 先设置离线
update_witness 25638 YYW1111111111111111111111111111111114T1Anm null null null true
# 然后将押金改为0，押金过7天时间会退回（如果没有因为作恶被扣除的话）
update_witness 25638 null 0 YOYO null true
```

## 领取见证人收益
```
# collect_witness_pay <witness_account> <pay_amount> <pay_asset_symbol> <broadcast>
collect_witness_pay 25638 100 YOYO true
``` 