# <b>Waffle Tool</b>
---

 [Waffle](https://www.getwaffle.io/) is a popular development framework for testing Solidity smart contracts. Since SwapDEX is Ethereum compatible, with a few lines of extra configuration, you can use SwapDEX as you usually would with Ethereum to develop on SwapDEX.

 **Configure Waffle to Connect to SwapDEX**

 Assuming you already have a JavaScript project, install Waffle:

 ```
 npm install ethereum-waffle
 ```

 To configure Waffle to run tests against a SwapDEX development node or the SwapDEX Testnet, within your tests create a custom provider and add network configurations:

 **Javascript**

 ```javascript
 describe ('Test Contract', () => { 
     // Use custom provider to connect to SwapDEX or Edgeware development node const 
     SwapDexProvider = new ethers.providers.JsonRpcProvider(('https://rpc.swapdex.network') 
     const devProvider = new ethers.providers.JsonRpcProvider('http://localhost:9933/'); })
 ```

 <br></br>

<p align=right> Written by Masterdubs & Petar </p>