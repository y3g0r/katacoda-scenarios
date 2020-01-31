Chaincode can be invoked either from SDK or from command line. We will use command line.

Execute bash in cli container:

`docker exec -ti cli bash`{{execute}}

`peer chaincode query` command is used to send proposal and retrieve transaction simulation as a result. This can be used for read operations, as simulation result is not written to the ledger.
`peer chaincode query -C mychannel -n mycc -c '{"Args":["query", "a"]}'`{{execute}}

`peer chaincode query -C mychannel -n mycc -c '{"Args":["query", "b"]}'`{{execute}}

`peer chaincode query -C mychannel -n mycc -c '{"Args":["query", "c"]}'`{{execute}}


Feel free to explore chaincode source code in IDE fabric-samples/chaincode/chaincode_example02/go. Chaincode query/invoke entrypoint function has the following signature
`func Invoke(stub shim.ChaincodeStubInterface) pb.Response`