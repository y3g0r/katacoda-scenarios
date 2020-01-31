Fabric binaries can be found on GitHub [fabric releases](https://github.com/hyperledger/fabric/releases) page.

We will use version 1.4.4 for now:

`wget https://github.com/hyperledger/fabric/releases/download/v1.4.4/hyperledger-fabric-linux-amd64-1.4.4.tar.gz`{{execute}}

Extract archive

`tar -xvf hyperledger-fabric-linux-amd64-1.4.4.tar.gz`{{execute}}

Add bin folder to path

`echo 'export PATH=$PWD/bin:$PATH' >> ~/.bashrc`{{execute}}

Apply changes
`exec $SHELL`{{execute}}

Make sure fabric binaries are in the path now:
`peer version`{{execute}}