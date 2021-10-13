# <b>Ethereum Libraries</b>
---

!!! warning
    Proceed with caution! This page is work in progress!

For a web app to interact with the Ethereum blockchain (i.e., read blockchain data and send transactions to the network), it must connect to an Ethereum node.

For this purpose, every Ethereum client implements the JSON-RPC specification so that applications can rely on a uniform set of endpoints.

If you want to use JavaScript to connect with an Ethereum node, it's possible to use vanilla JavaScript. Still, several convenience libraries exist within the ecosystem that makes this much more accessible. 
With these libraries, developers can write intuitive, one-line methods to initialize JSON RPC requests (under the hood) that interact with Ethereum.

**Why use a library?**
---
These libraries take away much of the complexity of interacting directly with an Ethereum node. They also provide utility functions (e.g., converting ETH to Gwei). As a developer, you can spend less time dealing with the intricacies of Ethereum clients and more time focused on the unique functionality of your application.

## **Web3.js Library**
---

![web3](assets/Web3.JPG#center)

Web3.js is a collection of libraries that allows programmers to interact with these on-chain components by facilitating a connection to Ethereum nodes.‌

In Ethereum, nodes provide low-level interfaces for users to submit transactions. A node can receive transactions through a JSON RPC interface. JSON RPC is a textual encoding format allowing running processes to receive data. Nodes participating in the Ethereum network may expose this interface in different ways, depending on its configuration and the underlying software implementation. Common options include HTTP connections, IPC or WebSockets.‌

For the full user documentation, and API reference for web3.js, click [here](https://web3js.readthedocs.io/en/v1.5.2/).

## **Ethers.js Library**
---

The ethers.js library provides tools to interact with Ethereum Nodes with JavaScript, similar to web3.js. Kusari has an Ethereum-like API available that is fully compatible with Ethereum-style JSON RPC invocations. Therefore, developers can leverage this compatibility and use the ethers.js library to interact with a Kusari node as if they were doing so on Ethereum.

### **Setup Ethers.js with Kusari**

To get started with the ethers.js library, install it using the following command:

```
npm install ethers
```

Once done, the simplest setup to start using the library and its methods is the following:

```javascript
const ethers = require('ethers');

// Variables definition
const privKey = '0xPRIVKEY';

// Define Providerconst 
provider = new ethers.providers.StaticJsonRpcProvider('RPC_URL', {
        chainId: ChainId,
        name: 'NETWORK_NAME'
});

// Create Walletlet 
wallet = new ethers.Wallet(privKey, provider);
```

Different methods are available inside provider and wallet. Depending on which network you want to connect to, you can set the RPC_URL, ChainID, NETWORK_NAME to the following values:

**Kusari Development Node**

- RRC URL: `http://localhost:9933`
- ChainID: `142`
- Network Name: `dev`

**Kusari Testnet**
- RPC URL: `https://rpc-testnet.swapdex.network/rpc`
- ChainID: `142`
- Network Name: `Kusari`

## **Web3.py Library**
---

Web3.py is a set of libraries that allow developers to interact with Ethereum nodes using HTTP, IPC, or WebSocket protocols with Python. 
Kusari has an Ethereum-like API available that is fully compatible with Ethereum-style JSON RPC invocations. Therefore, developers can leverage this compatibility and use the web3.py library to interact with a Kusari node as if they were doing so on Ethereum.

### **Setup Web3.py with Kusari**

To get started with the web3.py library, install it using the following command:

```
pip3 install web3
```

Once done, the simplest setup to start using the library and its methods is the following:

```python
from web3 import Web3
RPC_URL = 'https://rpc-testnet.swapdex.network/rpc'

web3 = Web3(Web3.HTTPProvider('RPC_URL'))
```