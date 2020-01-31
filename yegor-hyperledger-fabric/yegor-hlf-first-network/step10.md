If we use `peer chaincode query` for chaincode operations which write to the ledger (use PutState API), we will get the response as if the data was altered, but it won't be actually committed.
`peer chaincode query -C mychannel -n mycc -c '{"Args":["invoke", "b", "a", "10"]}'`{{execute}}

`peer chaincode query -C mychannel -n mycc -c '{"Args":["query", "a"]}'`{{execute}}

## Peer chaincode invoke

In order to commit transaction we use `peer chaincode invoke` command.

The difference between `peer chaincode query` and `peer chaincode invoke` is as follows:

- Proposal is sent from client (cli in this case) to peer
- Peer deligates proposal to it's chaincode
- Chaincode perform read only interaction with the ledger to prepare read-write set
- Peer signs read-write set returned from chaincode, thus endorsing it
- Peer returns transaction simulation or endorsed proposal

`peer chaincode query` stops here.
`peer chaincode invoke` continues:

- The same operations as explained above are repeated for all provided peers
- Endorsment from all peers are collected by client and sent to orderer
- Orderer decides on order of transaction in current block and broadcasts new block once it's finished
- Peer receives new block, validate and commit transaction, adjusting ledger along the way

Chaincodes have [endorsment policies](https://hyperledger-fabric.readthedocs.io/en/release-1.4/glossary.html#endorsement-policy). 
Those are set at the time of chaincode instantiation. Those endorsment policies tell how many peers from which organizations must endorse proposal in order for it to be valid.

Let's try using `peer chaincode invoke` command.

```
peer chaincode invoke \
    --tls true \
    --peerAddresses peer0.org1.example.com:7051 \
    --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt \
    --peerAddresses peer0.org2.example.com:9051 \
    --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt \
    --orderer orderer.example.com:7050 \
    --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem \
    -C mychannel -n mycc -c '{"Args":["invoke","b","a","10"]}'
```{{execute}}

peer chaincode invoke \
    --tls true
    --peerAddresses peer0.org1.example.com:7051 \
    --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt \
    --peerAddresses peer0.org2.example.com:9051 \
    --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt \
    --orderer orderer.example.com:7050 \
    --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem \
    -C mychannel -n mycc -c '{"Args":["invoke","b","a","10"]}'

peer chaincode invoke \
    -C mychannel -n mycc -c '{"Args":["invoke","b","a","10"]}'

peer chaincode invoke \
    --tls true \
    -C mychannel -n mycc -c '{"Args":["invoke","b","a","10"]}'

peer chaincode invoke \
    --tls true \
    --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem \
    -C mychannel -n mycc -c '{"Args":["invoke","b","a","10"]}'

Seems like now everything is fine, right? Let's check:

`peer chaincode query -C mychannel -n mycc -c '{"Args":["query", "a"]}'`{{execute}}

peer chaincode invoke \
    --tls true \
    --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem \
    --waitForEvent \
    -C mychannel -n mycc -c '{"Args":["invoke","b","a","10"]}'

Now we can see that there is an endorsment failure

Let's add a peer from another organization. 

peer chaincode invoke \
    --tls true \
    --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem \
    --peerAddresses peer0.org2.example.com:9051 \
    --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt \
    --waitForEvent \
    -C mychannel -n mycc -c '{"Args":["invoke","b","a","10"]}'

Turns out if we explicitly specify peer in command line flags original peer from environment variables is not used and some of the environment variables conflict with flags specified as command line options, so lets include both peer0.org1 and peer0.org1 in command line options:

peer chaincode invoke \
    --tls true \
    --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem \
    --peerAddresses peer0.org1.example.com:7051 \
    --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt \
    --peerAddresses peer0.org2.example.com:9051 \
    --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt \
    --waitForEvent \
    -C mychannel -n mycc -c '{"Args":["invoke","b","a","10"]}'

Finally it looks ok, lets check using query:

`peer chaincode query -C mychannel -n mycc -c '{"Args":["query", "a"]}'`{{execute}}
In this particular scenario invoke command looks really cumbersome as we have to pass a ton of parameters with long paths.

Parameters explanation:
- orderer - location of orderer, 
