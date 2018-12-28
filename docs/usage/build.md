# Compiling and Running yoyow-core

## Recommended Hardware Environment

System configuration: two cores, 4G memory

Recommended System: Ubuntu 16.04 LTS (64-bit)

## Installation Dependence:
```
sudo apt-get update
sudo apt-get install autoconf cmake make automake libtool git libboost-all-dev libssl-dev g++ libcurl4-openssl-dev
```

## Compile Script:
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

## Running:

After the compilation is completed, two binary files are generated, yoyow_node and yoyow_client.

Yoyow_node is generally called a node program and is mainly used to connect to a blockchain network. The functions include synchronizing data on the chain, broadcasting transactions, and providing basic [API](../api/node_api.html), etc.

Yoyow_client is generally called a client program. It further encapsulates the API provided by the node program to provide users with a more friendly API. It also provides private key storage management and cryptographic signature function, so it is also called a wallet program.

### Node Program
```
./programs/yoyow_node/yoyow_node
```

When the node program runs, it automatically creates a directory ```witness_node_data_dir``` that stores data and configuration files. It takes several hours to synchronize all the data. After the synchronization is completed, you can stop the operation with Ctrl+C.

It should be noted that after inputting Ctrl+C, the program will continue to run for a while. Do not type Ctrl+C again. Otherwise, the database will be inconsistent. It may take a long time to replay the next time you start the node.

After receiving Ctrl+C, the program will print out in the log

```
1553960ms asio       main.cpp:238                  operator()           ] Caught SIGINT attempting to exit cleanly
1553960ms th_a       main.cpp:251                  main                 ] Exiting from signal 2
```
At this point, wait patiently for the program to exit.

When the node program runs, the websocket interface is not opened by default, and you need to add parameters when executing.
```
./programs/yoyow_node/yoyow_node --rpc-endpoint 127.0.0.1:8090
```
You can also directly modify the configuration of rpc-endpoint in the configuration file ```witness_node_data_dir/config.ini```.

### Client Program (Wallet)

The client program needs to connect to the websocket interface of the node program at runtime. By default, it will try to connect to 127.0.0.1:8090. You can also use the `-s` parameter to specify other addresses. 
```
./programs/yoyow_client/yoyow_client -s ws://47.52.155.181:10011
```

The client program provides a command line that can be interactive. You can perform some operations by typing the relevant commands through the command line.

As mentioned above, the client program saves the private key. For security reasons, in the operation of using the private key, you need to use the password to unlock the client. The password settings and unlock command are as follows:

```
>>> set_password <PASSWORD>
>>> unlock <PASSWORD>
```

Then you can import the private key
```
unlocked >>>  import_key <account> <private_key>
```

More supported commands can be viewed with the help command or by looking at the [wallet api](../api/wallet_api.html) documentation.
