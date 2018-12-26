# Asset Related Operations

The following related operation commands can be executed in the interactive command line in yoyow_client

## Common Transfer
```
# transfer <from> <to> <amount> <asset_symbol> <memo> <broadcast>
transfer 250926091 209414065 100 YOYO "memo" true
```
For details, please refer to: [transfer](../api/wallet_api.html#transfer)

## Query

### Checking YOYO Balance in the Account

This function only returns the number of YOYO owned by the account.
```
# list_account_balances <account> 
list_account_balances 250926091
```
For details, please refer to:
[list_account_balances](../api/wallet_api.html#list-account-balances)

### Checking All Assest Balances in the Account

This function does not have an interface for the wallet at the moment, and it can only call the node's api, such as using wscat to link the websocket port.

```
{"id":1, "method":"call", "params":[0,"get_account_balances",["250926091", []]]}
```
For details, please refer to: [get_account_balances](../api/node_api.html#get-account-balances)

## About Bonus Points

### Checking Bonus Points

Bonus Points can be viewed by using the command "get_full_account" and the "statistics" in the returned results include "csaf""core balance""prepaid", corresponding to "Bonus Points""Balance""Tipping".
```
# get_full_account <account_name_or_id>
get_full_account 250926091
```

### Collecting the Bonus Points for the Time Being

```
# collect_csaf <from> <to> <amount> <asset_symbol> <broadcast>
collect_csaf 250926091 209414065 100 YOYO true
```
For details, please refer to: [collect_csaf](../api/wallet_api.html#collect-csaf-with-time)

### Collecting the Bonus Points Accumulated before a Specific Time
```
# collect_csaf_with_time <from> <to> <amount> <asset_symbol> <time> <broadcast>
collect_csaf_with_time 250926091 209414065 100 YOYO "2018-04-16T02:44:00" true
```
For details, please refer to: [collect_csaf_with_time](../api/wallet_api.html#collect-csaf-with-time)

## User-Issued Assets

There are two core operations for the user-issued assets, including creating assets (create_asset) and issuing assets (issue_asset). The newly created assets are not available (no circulation) and need to be issued to the specified account before they can be used.

### Getting Assets Info

```
# get_asset <asset_name_or_id>
get_asset 3
```
For details, please refer to: [get_asset](../api/wallet_api.html#get-asset)

### Checking Assets List
```
# list_assets <lower_bound_symbol> <limit> 
list_assets YOYO 4
```
For details, please refer to: [list_assets](../api/wallet_api.html#list-assets)

### Creating Assets
```
# create_asset <issuer> <symbol> <precision> <common> <initial_supply> <broadcast>
create_asset 250926091 TOTOTO 4 {"max_supply":300000,"market_fee_percent":0,"max_market_fee":0,"issuer_permissions":4} 200000 true
```
For details, please refer to: [create_asset](../api/wallet_api.html#create-asset)

### Issuing Assets
```
# issue_asset <to_account> <amount> <symbol> <memo> <broadcast>
issue_asset 250926091 10000 WOWO memo true
```
For details, please refer to: [issue_asset](../api/wallet_api.html#issue-asset)

### Updating Assets

```
# update_asset <symbol> <new_issuer> <new_options> <broadcast>
update_asset WOWO null {"max_supply":"2000000000"} true
```
For details, please refer to: [update_asset](../api/wallet_api.html#update-asset)

### Destroying Assets

```
# reserve_asset <from> <amount> <symbol> <broadcast>
reserve_asset 250926091 1000 WOWO true
```
For details, please refer to: [reserve_asset](../api/wallet_api.html#reserve-asset)
