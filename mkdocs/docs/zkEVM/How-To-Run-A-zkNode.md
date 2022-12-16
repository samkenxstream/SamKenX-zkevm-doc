

This document is a guide to creating a local single-node zkEVM blockchain with no connections to external peers. It will only exist on your local machine.



## zkNode's Brief Overview


zkNode is the software needed to run a zkEVM node. It is a client that the network requires to implement the synchronization and govern the roles of the participants (Sequencers or Aggregators). 

Polygon zkEVM participants can choose how they participate:

- As a node to know the state of the network, or
- As a participant in the process of batch production in any of the two roles: **Sequencer** or **Aggregator**

The zkNode Architecture modular in nature. See the diagram below for a highlevel view of the zkNode.



![Figure 3: zkEVM zkNode Diagram](https://wiki.polygon.technology/assets/images/fig3-zkNode-arch-aa4d18996fba1849291ea18e3f11d955.png)



Note that for Testnet purposes, a centralised Sequencer is implemented. 

Further details on the zkNode and its components are covered [here](https://docs.hermez.io/zkEVM/Overview/Overview/#zknode-architecture). 





## Before Setting Up A Local zkNode

In order to participate in the Polygon zkEVM, whether it is for testnet or mainnet, one needs to set up a zkNode. This will allow users to test their smart contracts, experiment with new code, and play around with the network on their local machines.



```text
CAUTION!

Currently the zkProver does not run on ARM-powered Macs.
For Windows users, using WSL/WSL2 is not recommended.

Unfortunately, Apple M1 chips are not supported for now 
- since some optimizations on the zkProver require 
specific Intel instructions. 
This means some non-M1 computers won't work regardless of 
the OS, for example: AMD.

```





### Prerequisites

The current zkEVM version requires `go`, `docker` and `docker-compose` to have been installed on your machine. If you don’t have them installed, check out the links provided below.

- https://go.dev/doc/install
- https://www.docker.com/get-started
- https://docs.docker.com/compose/install/



#### System Requirements

- zkEVM Node: 16GB RAM with 4-core CPU
- zkProver: 1TB RAM with 128-core CPU



If you want to run a full-fledged zkProver on your own, you'll need at least 1TB of RAM. If you are unable to meet the Prover requirements, you can still run the zkNode.

After completing this tutorial, the following components will be running in your local machine;

- zkEVM Node Databases
- Explorer Databases
- L1 Network
- Prover
- zkEVM Node components
- Explorers





### Setting Up A Local zkNode

Before starting the zkEVM node setup, you need to clone the [official zkNode repository](https://github.com/0xPolygonHermez/zkevm-node) from Polygon zkEVM Github.

```text
git clone https://github.com/0xPolygonHermez/zkevm-node.git 
```



The `zkevm-node` docker image must be built at least once and whenever the code is changed. If you haven't already built the `zkevm-node` image, you must run:

```text
make build-docker
```



Certain commands on the `zkevm-node` can interact with smart contracts, run specific components, create encryption files, and print debug information.

To interact with the binary program, we provide `docker-compose` files and a `Makefile` to spin up/down the various services and components, ensuring smooth local deployment and a better command line interface for developers.



```text
DANGER!
All the data is stored inside of each docker container. 
This means once you remove the container, the data will be lost.
```



The `test/` directory contains scripts and files for developing and debugging. Change the working directory to `test/` on your local machine.

```text
cd test/
```



Now, run the zkNode environment:

```text
make run
```



To stop the zkNode:

```text
make stop
```



To restart the whole zkNode environment:

```text
make restart
```



### Sample Data

It's important to populate your local zkEVM node with some data before you start testing out the network. The `make run` command will execute the containers required to run the environment, but it will not execute anything else. **Your local L2 network will be essentially empty**.

The following scripts are available if you require sample data that has already been deployed to the network.



```text
# To add some examples of transactions and smart contracts:
make deploy-sc

# To deploy a full Uniswap environment:
make deploy-uniswap

# To grant the MATIC smart contract a set amount of tokens:
make run-approve-matic
```






### Connecting to Metamask


INFO
Metamask requires the network to be running while configuring it, so make sure your network is up.

To configure your Metamask to use your local zkEVM environment, follow these steps:

1. Log in to your Metamask wallet
2. Click on your account picture and then on **Settings**
3. On the left menu, click on **Networks**
4. Click on **Add Network** button
5. Fill up the L2 network information
    **Network Name**: Polygon zkEVM - Local
    **New RPC URL**: http://localhost:8123
    **ChainID**: 1001
    **Currency Symbol**: ETH
    **Block Explorer URL**: http://localhost:4000

6. Click on **Save**
7. Click on **Add Network** button
8. Fill up the L1 network information
    **Network Name**: Geth - Local
    **New RPC URL**: http://localhost:8545
    **ChainID**: 1337
    **Currency Symbol**: ETH
9. Click on **Save**



You can now interact with your local zkEVM network and sign transactions from your Metamask wallet.