Finally, prerequisites are met, we can proceed to actual lab.


`cd first-network`{{execute}}

The next step will populate `channel-artifacts` and `crypto-config`, meanwhile make sure that those folders are empty:

`ls channel-artifacts/ crypto-config/`{{execute}}

`./byfn.sh generate`{{execute}}

Check that artifacts were generated:
`ls channel-artifacts/ crypto-config/`{{execute}}

## Challenge
Using IDE tab to the right of terminal tab explore byfn.sh script and try to figure out what generate command is doing.

[Detailed instructions](https://hyperledger-fabric.readthedocs.io/en/release-1.4/build_network.html)