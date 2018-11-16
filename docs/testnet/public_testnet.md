# 测试网
# Testnet

## 测试网可执行文件
## Executable Files for Testnet

测试网使用的可执行文件在
[yoyow-core-testnet](https://github.com/yoyow-org/yoyow-core-testnet/releases)下载。

The executable files for testnet can be downloaded on [yoyow-core-testnet](https://github.com/yoyow-org/yoyow-core-testnet/releases)

## 测试网网页钱包
## Testnet Web Wallet

测试网使用的网页钱包地址：<http://demo.yoyow.org:8000/#/>

The website address of the web wallet used in testnet: <http://demo.yoyow.org:8000/#/>

可以在网页钱包注册测试网账户，注册成功后会送12000个TEST YOYO。

You can register your testnet account in the web wallet. After registration, you will receive 12000 TEST YOYO.

## 运行测试网node节点
## Running the Testnet Node

搭建测试网全节点，直接运行下载的测试网的yoyow_node即可，以Ubuntu环境为例

Set up a testnet node, run the downloaded yoyow_node of the testnet directly, taking the Ubuntu environment as an example.

```
./yoyow_node  --rpc-endpoint 127.0.0.1:8090 
```

## 测试网公共node节点
## Testnet Public Node

如果不想自己搭建测试网node全节点，可以直接使用公共的node全节点：

If you don't want to build a testnet full node by yourself, you can use the public full node directly:

```
websocket 接口地址： ws://47.52.155.181:10011
jsonrpc 接口地址： http://47.52.155.181:10011/rpc
```
```
websocket interface address: ws://47.52.155.181:10011
jsonrpc interface address: http://47.52.155.181:10011/rpc
```
## 运行测试网钱包
## Running Testnet Wallet

如果使用公共的node节点，yoyow_client 需要添加`-s`参数来指定链接的node地址：

If using public node，yoyow_client needs to be added `-s` parameter to specify the node address of the link：

```
./yoyow_client -s ws://47.52.155.181:10011 -r 0.0.0.0:8091 -H 127.0.0.1:8093
```

## 其他
## Others
如果您在测试中遇到任何问题，比如测试币不够，请加小助手微信：yoyow99，申请进YOYOW开发者群。 

If you encounter any problems in testing, such as insufficient test tokens, please add the WeChat of the assistant: yoyow99; apply for the membership of the YOYOW developer group.

![yoyow99](../images/testnet/yoyow99.png)
