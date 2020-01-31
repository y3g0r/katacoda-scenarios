In IDE tab edit .env file in fabric-samples/first-network so that
`IMAGE_TAG=1.4.4`

Then bring up the network
`./byfn.sh up -o etcdraft`{{execute}}

This command will bring up docker-compose services comprising fabric blockchain network, and start end-to-end test right-away.

We will discuss end-to-end test in the next step, feel free to proceed w/o waiting for command to finish.

## Challenge

Using IDE tab to the right of Terminal tab, explore byfn.sh script and figure out docker-compose files/services structure which comprises the network. You can also create new terminal and invoke docker commands.