# Witness Operations

## Witness Node Construction

Complete tutorials for building witnesses can be found in [YOYOW Witness Tutorials (Ubuntu)](https://mp.weixin.qq.com/s/l4KfKtUUfaCEp9ykIbIByA)

## Query
### Getting Info of Witnesses
```
# get_witness <owner_account>
get_witness 132826789
```
For details, please refer to: [get_witness](../api/wallet_api.html#get-witness)

### Query for Witness List
```
# list_witnesses <lower_bound> <limit> <order_by>
list_witnesses 0 5 1
```
For details, please refer to: [list_witnesses](../api/wallet_api.html#list-witnesses)

## Creating Witnesses
```
# create_witness <owner_account> <block_signing_key> <pledge_amount> <pledge_asset_symbol> <url> <broadcast>
create_witness 223331844 YYW1111111111111111111111111111111114T1Anm 1000000 YOYO "http://www.yoyow.org" true
```
For details, please refer to: [create_witness](../api/wallet_api.html#create-witness)

## Updating the Info of Witnesses
```
# update_witness <witness_account> <block_signing_key> <pledge_amount> <pledge_asset_symbol> <url> <broadcast>
update_witness 223331844 null 100345 YOYO null true
```
For details, please refer to: [update_witness](../api/wallet_api.html#update-witness)

## Witness Voting
```
# update_witness_votes <voting_account> <witnesses_to_add> <witnesses_to_remove> <broadcast>
update_witness_votes 250926091 [abit] [] true
```
For details, please refer to: [update_witness_votes](../api/wallet_api.html#update-witness-votes)

## Witness being Offline
```
# Set an offline status first
update_witness 25638 YYW1111111111111111111111111111111114T1Anm null null null true
# and then change the deposit to 0, and the deposit will be returned after 7 days (if no deduction due to misbehaviors)
update_witness 25638 null 0 YOYO null true
```

## Getting Witness Returns
```
# collect_witness_pay <witness_account> <pay_amount> <pay_asset_symbol> <broadcast>
collect_witness_pay 25638 100 YOYO true
``` 
