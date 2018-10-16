# 测试网

假如测试网的方式很简单，官方测试网p2p node的链接地址为“”，运行witness_node 时指定seed node 为该链接即可。同时需要指定genesis文件。
```
./witness_node  --seed-nodes='["127.0.0.1:1999"]'  --rpc-endpoint 0.0.0.0:8091  -d test_net --genesis-json genesis.json
```

如果不想自己搭建测试网节点，可以直接使用公共的api
```
websocket 接口地址： ws://47.52.155.181:10011
jsonrpc 接口地址： http://47.52.155.181:10011/rpc
```