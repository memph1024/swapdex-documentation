# <b>KUSARI ENDPOINTS</b>
---

When interacting with the Kusari network via our [substrate explorer app](https://substrate-explorer-testnet.swapdex.network/?rpc=wss%3A%2F%2Fswapdex.starkleytech.com%2Fws#/settings) or other UIs and programmatic methods, you'd ideally be running your own node ([text guide](../what-to-try/validator.md)). Granted, that's not something everyone wants to do, so convenience trumps ideals in most cases. To facilitate this convenience, Kusari has several public endpoints you can use for your DApps.

## <b>Starkley Tech Archive Node</b>
---
Starkley Tech, the company that develops the SwapDex Rust client, maintains an archive node at endpoint `https://rpc-testnet.swapdex.network/rpc`

To connect to the Starkley Tech node, use the endpoint in your JavaScript DApps like so:
``` javascript
const{ ApiPromise, WsProvider } = require('@polkadot/api')

(async () => {
    const provider = new WsProvider('wss.swapdex.starkleytech.com')
    const api = await ApiPromise.create({ provider })
    // ...
```
or in Kusari Substrate Explorer by clicking on the top-left corner of the screen and openig up the TEST NETWORK group and selecting SwapDex Kusari and **via Starkley Tech**

![img](assets/phoenix-endpoint.png#center)

<br></br>

<p align=right> Written by Masterdubs & Petar </p>