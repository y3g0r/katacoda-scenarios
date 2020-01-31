We could [use bootstrap script](https://hyperledger-fabric.readthedocs.io/en/release-1.4/install.html) to download fabric binaries, samples and pull docker images: 

```
mkdir fabric
cd fabric
curl -sSL http://bit.ly/2ysbOFE | bash -s -- 1.4.4 1.4.4 0.4.18
```{{execute}}

but it will download things we don't need for this lab, so let's do everything manually.

P.S. Katacoda environment is rather slow, so feel free to use bootstrap script if you have ~10 mins to wait, or use it on your local system, it will take faster.
