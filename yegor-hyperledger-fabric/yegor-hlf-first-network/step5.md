Finally, prerequisites are met, we can proceed to the actual lab.


`cd first-network`{{execute}}

The next step will populate `channel-artifacts` and `crypto-config`, meanwhile make sure that those folders are empty:

`ls channel-artifacts/ crypto-config/`{{execute}}

`./byfn.sh generate -o etcdraft`{{execute}}

Check that artifacts were generated:
`ls channel-artifacts/ crypto-config/`{{execute}}

## Challenge
Using IDE tab to the right of the terminal tab explore byfn.sh script and try to figure out what generate command is doing.