# <b>Phoenix Endpoints</b>

When interacting with the Phoenix network via our [substrate explorer app](https://substrate-explorer-testnet.swapdex.network/?rpc=wss%3A%2F%2Fswapdex.starkleytech.com%2Fws#/settings) or other UIs and programmatic methods, you'd ideally be running your own node (text guide, video guide). Granted, that's not something everyone wants to do, so convenience trumps ideals in most cases. To facilitate this convenience, Kusama has several public endpoints you can use for your apps provided by infrastructure and API services providers in the ecosystem.

## <b>Starkley Tech Archive Node</b>

Starkley Tech, the company that develops the SwapDex Rust client, maintains an archive node at endpoint `https://rpc-testnet.swapdex.network/rpc`

To connect to the Starkley Tech node, use the endpoint in your JavaScript apps like so:
``` javascript
const{ ApiPromise, WsProvider } = require('@polkadot/api')

(async () => {
    const provider = new WsProvider('wss://kusama-rpc.polkadot.io/')
    const api = await ApiPromise.create({ provider })
    // ...
```
or in Phoenix Substrate Explorer by clicking on the top-left corner of the screen and openig up the TEST NETWORK group and selecting SwapDex Phoenix and **via Starkley Tech**

![endpoint](assets/phoenix-endpoint.png)