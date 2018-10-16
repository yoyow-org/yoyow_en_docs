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
### 节点程序
```
./programs/yoyow_node/yoyow_node
```
节点程序执行时，会在当前目录下自动创建存放链数据的文件夹（witness_node_data_dir），其中也包含配置文件。链数据同步会花费几个小时，同步完成后，可以通过 Ctrl+C 停止运行，也可以
The node will automatically create a data directory including a config file. It may take several hours to fully synchronize the blockchain. After syncing, you can exit the node using Ctrl+C and setup the command-line wallet by editing ```witness_node_data_dir/config.ini``` as follows:
```
rpc-endpoint = 127.0.0.1:9000
```
### 客户端程序（钱包）
After starting the witness node again, in a separate terminal you can run:
```
./programs/yoyow_client/yoyow_client
```
Set your inital password:
```
>>> set_password <PASSWORD>
>>> unlock <PASSWORD>
```
