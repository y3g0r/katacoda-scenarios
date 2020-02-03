Lets now install another chaincode ourselve.

## Check available chaincodes

`docker exec -ti cli bash`{{execute}}

`ls $GOPATH/src/github.com/chaincode`{{execute}}

These examples each demonstate specific aspects of chaincode API and specific fabric features. 

Feel free to navigate source code in IDE and read about specific examples in docs. 

We will use sacc as it's a simple chaincode which doesn't require any new knowledge to use it.

## Install

Chaincode is installed on peer.

This step doesn't care about channel or endorsment policies, or orderer tls CA certificates.

Just provide name, version and path (in case of chaincode in golang, path is releative to $GOPATH/src).

`peer chaincode install --name sacc --version 1 --path github.com/chaincode/sacc`{{execute}}

Make sure chaincode was in fact installed

`peer chaincode list --installed`{{execute}}

## Instantiate

When chaincode is instantiated it becomes available on particular channel.

Also, we have to provide endorsment policy at this stage, because default one might be not what we are looking for.

Another point is that chaincode instantiation on a channel creates config transaction which must be committed. 
For this, orderer is involved, so we have to explicitly enable tls and provide path to root TLS CA certificate.

And the last point is that when chaincode gets instantiated it may require some initialization parameters. 
Provide them with --ctor/-c parameter as we do in `peer chaincode invoke`.

`export CRYPTO=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto`{{execute}}

```
peer chaincode instantiate \
    --name sacc \
    --version 1 \
    --channelID mychannel \
    --policy 'OR("Org1MSP.member")' \
    --ctor '{"Args":["mybalance", "10000000"]}' \
    --tls true \
    --cafile $CRYPTO/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem
```{{execute}}

Make sure chaincode was in fact instantiated

`peer chaincode list --instantiated -C mychannel`{{execute}}

## Query and Invoke

`peer chaincode query -C mychannel -n sacc -c '{"Args":["get", "mybalance"]}'`{{execute}}

```
peer chaincode invoke \
    --waitForEvent \
    --tls true \
    --cafile $CRYPTO/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem \
    -C mychannel \
    -n sacc \
    -c '{"Args":["set", "mybalance", "9999999999"]}'
```{{execute}}

Check

`peer chaincode query -C mychannel -n sacc -c '{"Args":["get", "mybalance"]}'`{{execute}}

## Challenge

Why we don't need to provide additional peers when invoking sacc chaincode?
