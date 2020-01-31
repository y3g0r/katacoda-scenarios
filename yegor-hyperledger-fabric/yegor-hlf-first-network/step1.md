Go to https://hyperledger-fabric.readthedocs.io/en/release-1.4/prereqs.html and make sure prerequisites are met in current lab.

Check curl
`curl --version`{{execute}}

Check docker
`docker --version`{{execute}}

Check docker-compose
`docker-compose --version`{{execute}}

Check go version
`go version`{{execute}}

Make sure `GOPATH` is set
`echo $GOPATH`{{execute}}

Create `GOPATH` directory
`mkdir -pv $GOPATH`{{execute}}

Make sure `GOPATH/bin` is included in `PATH` environment variable
`echo $PATH`{{execute}}

Ignore `node` and `python2.7` prerequisites for now.