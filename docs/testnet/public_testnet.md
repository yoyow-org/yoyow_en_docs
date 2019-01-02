# Testnet

## Executable Files for Testnet

The executable files for testnet can be downloaded on [yoyow-core-testnet](https://github.com/yoyow-org/yoyow-core-testnet/releases)

## Testnet Web Wallet

The website address of the web wallet used in testnet: <http://demo.yoyow.org:8000/#/>

You can register your testnet account in the web wallet. After registration, you will receive 12000 TEST YOYO.

## Running the Testnet Node

Set up a testnet node, run the downloaded yoyow_node of the testnet directly, taking the Ubuntu environment as an example.

```
./yoyow_node  --rpc-endpoint 127.0.0.1:8090 
```

## Testnet Explorer

Testnet Explorer: <https://testnet.explorer.yoyow.org/>

## Testnet Public Node

If you don't want to build a testnet full node by yourself, you can use the public full node directly:

```
websocket interface address: ws://47.52.155.181:10011
jsonrpc interface address: http://47.52.155.181:10011/rpc
```

## Running Testnet Wallet

If using public node，yoyow_client needs to be added `-s` parameter to specify the node address of the link：

```
./yoyow_client -s ws://47.52.155.181:10011 -r 0.0.0.0:8091 -H 127.0.0.1:8093
```

## Others

If you encounter any problems in testing, such as insufficient test tokens, please add the WeChat of the assistant: yoyow99; apply for the membership of the YOYOW developer group.

![yoyow99](../images/testnet/yoyow99.png)
