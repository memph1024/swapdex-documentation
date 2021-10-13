# **Setting up an Kusari node for local development**
---

## **Setting up a local Kusari node for WASM/EVM development**

This guide walks you through how to quickly fire up a personal Kusari blockchain which you can use to run tests, execute commands, and inspect state while controlling how the chain operates.

!!! note
    **Note** This is a fast-track way to run a node. You can always compile from source as well. We recommend using your own compiled binaries for production mainnet.

!!! note
    **Note** Note If you don't have Docker installed, you can quickly [install it from here](https://docs.docker.com/get-docker/)

You can clone our repo with docker-compose to get started right away:

```
git clone https://github.com/edgeware-network/edgeware-node
cd edgeware-node
docker run --rm -it decentration/edgeware:v3.3.3
cd docker
docker-compose -f docker-compose-local.yml up
```

!!! note
    **Note** If you want to reset or purge the local chain, delete the docker container by running `docker-compose rm`

Now you should see a series of status updates in your terminal.

Afterwards you can head to [Substrate Explorer App](https://substrate-explorer-testnet.swapdex.network/?rpc=wss%3A%2F%2Fswapdex.starkleytech.com%2Fws#/treasury) and connect to `127.0.0.1:9944`, and you should see blocks being produced.

Now you can continue to connect Metamask, Remix, and Web3.js to have great experience.

## **Reach out to us for more engagement**

Glad you've made it through! 🥰 We are eager to guide your more on your exploration through Kusari Ethereum compability feature. We are keen to hear your experience and suggestions you may have for us.. You can feel free to chat with us on our Discord Server. We can assist you with issues you may have or a project you may want to get funded through our treasury. Don't hesitate to share your feedback on our channels, there is always space to improve! 🙌
