# 测试网

YoYoW发布的可执行文件主要包括两个yoyow_node, yoyow_wallet.
yoyow_node 一般也被叫做node程序，主要负责同步链上数据，

测试网使用的可执行文件在
[yoyow-core-testnet](https://github.com/yoyow-org/yoyow-core-testnet/releases)下载。

测试网使用的网页钱包地址：http://demo.yoyow.org:8000/#/

加入测试网，直接运行下载的测试网的yoyow_node即可，以Ubuntu环境为例
```
./yoyow_node  --rpc-endpoint 0.0.0.0:8091  -d test_net
```

如果不想自己搭建测试网节点，可以直接使用公共的api
```
websocket 接口地址： ws://47.52.155.181:10011
jsonrpc 接口地址： http://47.52.155.181:10011/rpc
```