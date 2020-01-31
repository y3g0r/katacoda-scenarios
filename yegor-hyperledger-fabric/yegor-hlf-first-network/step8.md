In this step we will interact with the our network by [querying](https://hyperledger-fabric.readthedocs.io/en/release-1.4/glossary.html#query) (used to read data from the ledger) and [invoking](https://hyperledger-fabric.readthedocs.io/en/release-1.4/glossary.html#invoke) (used to write data to the ledger) chaincode using peer command in cli container.

List running containers:
`docker ps`{{execute}}

Execute bash in cli container:
`docker exec -ti cli bash`{{execute}}

## Chaincode list

Environment variable CORE_PEER_ADDRESS is used to instruct peer cli to talk to a particular peer server. Let's check if it's set:
`echo $CORE_PEER_ADDRESS`{{execute}}

All peer commands from cli container will control peer server running in peer0.org1.example.com container.

Lets check what chaincode is isnstalled on a peer:
`peer chaincode list --installed`{{execute}}

We can see that chaincode "mycc", version 1.0 is installed on peer0.org1.example.com.

Lets change to a different peer and check if any chaincode is installed there (look in base/docker-compose-base.yaml for peer addresses):
`export CORE_PEER_ADDRESS=peer1.org1.example.com:8051`{{execute}}

Lets check what chaincode is isnstalled on a peer:
`peer chaincode list --installed`{{execute}}

We can see that no chaincode is installed on peer1.org1.example.com. Let's switch back to original peer:
`export CORE_PEER_ADDRESS=peer0.org1.example.com:7051`{{execute}}

Chaincode on peer can be in 2 states: installed and instantiated. Installed, simply speaking, means that chaincode source code is present on peer's filesystem. But in order for chaincode to run it must be instantiated. Instantiated chaincode runs in a separate container.
Let's list instantiated chaincodes on peer0.org1.example.com. When chaincode is instantiated it is running in context of a particular channel, so we need to specify a channel

`peer chaincode list --instantiated --channelID mychannel`{{execute}}

We can also find instantiated chaincodes by inspecting running containers, but this approach is  dangerous, because there could be stale running chaincode containers.

```
exit
docker ps | grep peer0.org1.example.com
```{{execute}}