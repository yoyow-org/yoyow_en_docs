# 编译运行yoyow-core

推荐系统： Ubuntu 16.04 LTS (64-bit)

## 安装依赖:
```
sudo apt-get update
sudo apt-get install autoconf cmake make automake libtool git libboost-all-dev libssl-dev g++ libcurl4-openssl-dev
```

## 编译脚本:
```
git clone https://github.com/yoyow-org/yoyow-core.git
cd yoyow-core
git checkout yy-mainnet # may substitute "yy-mainnet" with current release tag
git submodule update --init --recursive
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release ../
make yoyow_node
make yoyow_client
```

## 运行:
编译完成后主要生成两个二进制文件，yoyow_node和yoyow_client。
yoyow_node一般被称为节点程序，主要用来连接区块链网络，作用包括同步链上数据，广播交易，提供基础的[API](../api/node_api.html)等。
yoyow_client 一般被叫称为客户端程序，对节点程序提供的API进一步封装，为用户提供更友好的API，同时提供了私钥保存管理，加密签名的功能，因此也被称为钱包程序。

### 节点程序
```
./programs/yoyow_node/yoyow_node
```
节点程序运行时会自动创建存放数据和配置文件的目录```witness_node_data_dir```，会花费几个小时同步完所有的数据。同步完成后，可以通过 Ctrl+C 停止运行。

需要注意的是，输入Ctrl+C后，程序依然会继续运行一会，不要再次键入Ctrl+C，否则会导致数据库不一致，下次启动node可能需要长时间的replay。收到Ctrl+C后，程序会在日志中打印出
```
1553960ms asio       main.cpp:238                  operator()           ] Caught SIGINT attempting to exit cleanly
1553960ms th_a       main.cpp:251                  main                 ] Exiting from signal 2
```
此时耐心等待程序退出即可。

节点程序运行时，默认并不会开放websockt接口，需要在执行时添加参数 
```
./programs/yoyow_node/yoyow_node --rpc-endpoint 127.0.0.1:8090
```
也可以直接修改配置文件```witness_node_data_dir/config.ini```中rpc-endpoint的配置。

### 客户端程序（钱包）
客户端程序在运行时需要连接到节点程序的websocket接口，默认会尝试连接127.0.0.1:8090，也可以使用`-s`参数指定其他地址。
```
./programs/yoyow_client/yoyow_client -s ws://47.52.155.181:10011
```
客户端程序提供了一个可以交互的命令行，可以通过命令行键入相关命令执行一些操作。

如前文所言，客户端程序保存有私钥，从安全考虑在使用到私钥的操作中，需要先使用密码解锁客户端，设置密码以及unlock的命令如下：
```
>>> set_password <PASSWORD>
>>> unlock <PASSWORD>
```

然后可以导入私钥
```
unlocked >>>  import_key <account> <private_key>
```

更多支持的命令，可以通过help命令查看，或者查看[wallet api](../api/wallet_api.html) 文档