
# About SWAN Snapshots

If you want start a swan node, you can refer [node operator](https://docs.swanchain.io/bulders/swan-node)


## Hardware requirements
We recommend you have this configuration to run a node:

 - 8-Core CPU
 - at least 16 GB RAM
 - a locally attached NVMe SSD drive
 - adequate storage capacity to accommodate both the snapshot restoration process (if restoring from snapshot) and chain data, ensuring a minimum of (2 * current_chain_size) + snapshot_size + 20%_buffer


## Docker
This tutorial assumes you are familiar with [Docker](https://www.docker.com/) and have it running on your machine.


## Running a Swan Node
This tutorial will walk you through setting up your own Swan Node, you can see here:
https://docs.swanchain.io/bulders/swan-node


## Snapshots
If you're a prospective or current SWAN Node operator and would like to restore from a snapshot to save time on the initial sync, it's possible to always get the latest available snapshot of the Swan chain on mainnet by using the following CLI commands. The snapshots are updated every two weeks.

## Restoring from snapshot
In the home directory of your SWAN Node, create a folder(`$SWAN_NODE_DATA`). If you already have this folder, remove it to clear the existing state and recreate it. Next, run the following code and wait for the operation to complete.

### Init Swan Node configuration
```
docker run --rm -v $SWAN_NODE_DATA:/opt/swan swanchain254/swan-node-mainnet:latest init

```
### Download the snapshot and untar it

 - Swan Mainnet Snapshot: 
 	- [swanchain_snapshot_2024_11_29_height_2845579.tar.gz](https://6ba96e4b2881.acl.swanipfs.com/ipfs/QmUgXvTLggufGisAYgjTEBt3AoJANRFjWdSfCQrpC9gsti)
	 - [swanchain_snapshot_2024_12_27_height_3325082.tar.gz](https://6ba96e4b2881.acl.swanipfs.com/ipfs/QmUJPgaJwH8A3jrfwJFfY9tt6RivSEnHzeAi55xTChEwGy)
```
wget https://6ba96e4b2881.acl.swanipfs.com/ipfs/QmUJPgaJwH8A3jrfwJFfY9tt6RivSEnHzeAi55xTChEwGy -O swanchain_snapshot_2024_12_27_height_3325082.tar.gz
```
You'll then need to untar the downloaded snapshot and place the `$SWAN_NODE_DATA/data/geth/` subfolder

```
tar -zxvf swanchain_snapshot_2024_12_27_height_3325082.tar.gz -C $SWAN_NODE_DATA/data/geth/
```


### Start Swan node

 - `L1RPC` URL and `L1BEACON`

You'll need your own `L1RPC` URL and `L1BEACON` URL. This can be one that you run yourself.

 - Run the docker command
```
docker run -dit --name swan-node --restart=always -v $SWAN_NODE_DATA:/opt/swan -p 8545:8545 -e L1RPC=$L1RPC -e  L1BEACON=$L1BEACON swanchain254/swan-node-mainnet:latest

```
 
### Confirm you get a response from:

```
curl -d '{"id":0,"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["latest",false]}' \
  -H "Content-Type: application/json" http://localhost:8545
```


### Check the node sync status
```
tail -f $SWAN_NODE_DATA/log/geth.log
```








