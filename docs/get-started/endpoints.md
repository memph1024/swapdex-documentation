# <b>SWAPDEX ENDPOINTS</b>
---

When interacting with the SwapDEX network via our <a href="https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fws.swapdex.network#/explorer" target="_blank"> substrate explorer app </a> or other UIs and programmatic methods, you'd ideally be running your own node (<a href="https://docs.swapdex.network/validator-guides/validator/" target="_blank"> text guide </a>). Granted, that's not something everyone wants to do, so convenience trumps ideals in most cases. To facilitate this convenience, SwapDEX has several public endpoints you can use for your DApps.

## <b>Starkley Tech Archive Node</b>
---
Starkley Tech, the company that develops the SwapDEX Rust client, maintains an archive node at endpoint `https://rpc.swapdex.network/`

To connect to the Starkley Tech node, use the endpoint in your JavaScript DApps like so:
``` javascript
const{ ApiPromise, WsProvider } = require('@polkadot/api')

(async () => {
    const provider = new WsProvider('wss://ws.swapdex.network')
    const api = await ApiPromise.create({ provider })
    // ...
```
or in SwapDEX Substrate Explorer by clicking on the top-left corner of the screen and opening up the TEST NETWORK group and selecting SwapDEX and **via Starkley Tech**

![img](assets/phoenix-endpoint.png#center)

<br></br>

<p align=right> Written by Masterdubs & Petar </p>