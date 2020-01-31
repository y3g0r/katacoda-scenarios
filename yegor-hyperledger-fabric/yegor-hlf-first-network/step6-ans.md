`./byfn.sh up -o etcdraft` command starts fabric network using docker compose stack, and when all containers are ready it starts end-to-end test right away.

Actual command which starts docker compose stack looks like this:
`docker-compose -f docker-compose-cli.yaml -f docker-compose-etcdraft2.yaml up -d`

Network consists of the following components:
- 4 peers, based on hyperledger/fabric-peer image
- 5 orderers, based on hyperledger/fabric-orderer image
- 1 cli, based on hyperledger/fabric-tools image

You can use `docker-compose -f docker-compose-cli.yaml -f docker-compose-etcdraft2.yaml ps` to see all the services.

Files/services structure involved:
- base/peer-base.yaml
  - defines 2 services: peer-base and orderer-base
- base/docker-compose-base.yaml
  - defines orderer.example.com by extending orderer-base from base/peer-base.yaml
  - defines peer{0,1}.org{0,1}.example.com (4 services) by extending peer-base from base/peer-base.yaml
- docker-compose-cli.yaml
  - defines cli service, used to drive end-to-end tests
  - puts cli, peer{0,1}.org{0,1}.example.com and orderer.example.com from base/docker-compose-base.yaml in the same network named 'byfn'
  - defines named volumes for orderer and peer services
- docker-compose-etcdraft2.yaml
  - defines orderer{2..5}.example.com (4 services) by extending orderer-base from base/peer-base.yaml (used only with etcdraft consensus type)

If you invoke `docker ps` you will also see containers based on dev-peer0.org{1,2}.example.com-mycc-1.0 images. Those are chaincode containers created by peers. Those are side effects of end-to-end test, so we will discuss them in the next step.