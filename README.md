# Deprecated 
**Note: The project's repository has been change to: https://github.com/lightstreams-network**

THIS HAS BEEN DEPRECATED PLEASE SEE OUR NEW REPO: https://github.com/lightstreams-network

# Lightstreams Network

[Lightstreams](http://lightstreams.network) is a blockchain network that provides decentralised applications (DApps) that need high performance and data privacy with the required support. We currently use Tendermint for transaction consensus and a Distributed Secure Storage Network based on IPFS that allows for digital contact to be only distributed to authorised nodes.

## Running local client connecting to our test network

For now, a docker image has been created as the easiest way to run the lightstream client, with all the environment settings setup for you. If you don't have Docker installed on your machine follow these instructions [here](https://docs.docker.com/engine/installation/). 

Then run the following command:
```
$ docker run -d --name device-1 -p 3001:3001 -e LOCALHOST="0.0.0.0:3001" lightstreams/lightstreams:v12
$ docker exec -it device-1 lightstreams-client run
```

This will host the lightstreams-client on port 3001, and IPFS on ports 4001, 5001.
Make sure that you do not have a firewall blocking these ports and they are open.

To test device-1, open your browser at:
http://0.0.0.0:3001/


If you want to create another docker container running on the same machine to test selling content to another device, then:
```
$ docker run -d --name device-2 -p 3002:3002 -e LOCALHOST="0.0.0.0:3002" lightstreams/lightstreams:v12
$ docker exec -it device-2 lightstreams-client run
```

To test device-2, open your browser at:
http://0.0.0.0:3002/

To clean up:
```
$ docker stop device-1
$ docker stop device-2
$ docker rm device-1
$ docker rm device-2
```

## Testing our blockchain

[OPTIONAL] Not necessary for running local client only in case you would like to connect to our blockchain and verify it.

Lightstreams uses the following clients for managing the Lightstreams blockchain
- [Ethermint](https://github.com/tendermint/ethermint) - Is based on a fork of the Go-Ethereum (Geth) client
- [Tendermint](https://github.com/tendermint/tendermint) - A consensus engine that used PoA instead of PoW

Build from source the following versions
- Ethermint (0.5.3)
- Tendermint (0.12.0)

Ensure you are using go 1.8.3
``` 
$ go get -u -d github.com/tendermint/ethermint
$ cd $GOPATH/src/github.com/tendermint/ethermint
$ git reset --hard b72a1eef6edca2e8dfe03296fc665de025a5ae78
$ make install

$ go get -u github.com/tendermint/tendermint/cmd/tendermint
$ cd $GOPATH/src/github.com/tendermint/tendermint
$ git reset --hard e236302256b8b0b75441e8e44c6d0d3f5b5152c6 
$ make install
```

Clone lightstreams 
```
$ git clone https://github.com/lightstreams/lightstreams.git
$ cd lightstreams
```

### Initialise 

Make a directory for the Lightstreams and copy genesis
```
$ mkdir -p ~/.lightstreams/tendermint
$ cp config/tendermint/genesis.json ~/.lightstreams/tendermint
```

Initialise Ethermint 
```
$ ethermint --datadir ~/.lightstreams init config/ethereum/genesis.json
```

### Run 

Run Tendermint
```
$ tendermint --home ~/.lightstreams/tendermint node --p2p.seeds '35.193.87.136:46656,35.192.228.28:46656,35.192.64.32:46656'
```

In another console window, run ethermint
```
$ ethermint --datadir ~/.lightstreams
```

### Conneting Geth

```
$ geth attach http://localhost:8545
```
