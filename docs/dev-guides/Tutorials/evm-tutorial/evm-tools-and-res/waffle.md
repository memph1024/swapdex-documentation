# <b>Waffle Tool</b>
---

 [Waffle](https://www.getwaffle.io/) is a popular development framework for testing Solidity smart contracts. Since Edgeware is Ethereum compatible, with a few lines of extra configuration, you can use Kusari as you usually would with Ethereum to develop on Kusari.

 **Configure Waffle to Connect to Kusari**

 Assuming you already have a JavaScript project, install Waffle:

 ```
 npm install ethereum-waffle
 ```

 To configure Waffle to run tests against a Kusari development node or the Kusari Testnet, within your tests create a custom provider and add network configurations:

 **Javascript**

 ```javascript
 describe ('Test Contract', () => { 
     // Use custom provider to connect to Kusari or Edgeware development node const 
     KusariProvider = new ethers.providers.JsonRpcProvider(('https://rpc-testnet.swapdex.network/rpc') 
     const devProvider = new ethers.providers.JsonRpcProvider('http://localhost:9933/'); })
 ```