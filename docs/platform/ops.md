# Platform Related Operations

The following related operation commands can be executed in the interactive command line in yoyow_client

## Getting Platform Number
```
get_platform_count
```
For details, please refer to: [get_platform_count](../api/wallet_api.html#get-platform-count)

## Getting Platform Info
```
# get_platform <owner_account>
get_platform 250926091
```
For details, please refer to: [get_platform](../api/wallet_api.html#get-platform)

## Checking Platform List
```
# list_platforms <lowerbound> <limit> <order_by>
list_platforms 0 5 1
```
For details, please refer to: [list_committee_members](../api/wallet_api.html#list-platforms)

## Creating Platforms
```
# create_platform <owner_account> <name> <pledge_amount> <pledge_asset_symbol> <url> <extra_data> <broadcast>
create_platform 223331844 yoyow.club 10000 YOYO "http://yoyow.club" null true
```
For details, please refer to: [create_platform](../api/wallet_api.html#create-platform)

## Undating Platform Info
```
# update_platform <platform_account> <name> <pledge_amount> <pledge_asset_symbol> <url> <extra_data> <broadcast>
update_platform 223331844 NUUUU null null "http://www.example.com" "http://www.example.com" true
```
For details, please refer to: [update_platform](../api/wallet_api.html#update-platform)

## Voting for Platforms
```
# update_platform_votes <voting_account> <platforms_to_add> <platforms_to_remove> <broadcast>
update_platform_votes 250926091 ["223331844"] [] true
```
For details, please refer to: [update_platform_votes](../api/wallet_api.html#update-platform-votes)

## Authorizing Platforms
```
# account_auth_platform <account> <platform_owner> <broadcast>
account_auth_platform 250926091 223331844 true
```
For details, please refer to: [account_auth_platform](../api/wallet_api.html#account-auth-platform)

## Canceling the Platform Authorization
```
# account_cancel_auth_platform <account> <platform_owner> <broadcast>
account_cancel_auth_platform 250926091 223331844 true
```
For details, please refer to: [account_cancel_auth_platform](../api/wallet_api.html#account-cancel-auth-platform)
