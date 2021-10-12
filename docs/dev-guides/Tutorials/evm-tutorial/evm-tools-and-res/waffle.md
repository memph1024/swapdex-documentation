# <b>Waffle Tool</b>
---

 [Waffle](https://www.getwaffle.io/) is a popular development framework for testing Solidity smart contracts. Since Edgeware is Ethereum compatible, with a few lines of extra configuration, you can use Phoenix as you usually would with Ethereum to develop on Phoenix.

 **Configure Waffle to Connect to Phoenix**

 Assuming you already have a JavaScript project, install Waffle:

 ```
 npm install ethereum-waffle
 ```

 To configure Waffle to run tests against a Phoenix development node or the Phoenix Testnet, within your tests create a custom provider and add network configurations:

 **Javascript**

 ```javascript
 describe ('Test Contract', () => { 
     // Use custom provider to connect to Phoenix or Edgeware development node const 
     PhoenixProvider = new ethers.providers.JsonRpcProvider(('https://rpc-testnet.swapdex.network/rpc') 
     const devProvider = new ethers.providers.JsonRpcProvider('http://localhost:9933/'); })
 ```