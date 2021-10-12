# <b>Phoenix and EVM</b>
---
![eth-logo](assets/eth-logo.png#center)

Phoenix has a pallet that allows developers to write EVM smart-contracts. This means that you can use Phoenix as you would with Ethereum. Phoenix is fully compatible with Ethereum's Web3 API and EVM. Here, we'll walk through a few subtle differences between Phoenix and Ethereum. Namely, Phoenix has a Proof of Stake-based consensus mechanism. This shouldn't affect you if you're building a DeFi or NFT based application. See our related documentation on proof-of-stake. In the following sections we detail Phoenix<>EVM Compatibility.

---

## **Full-Ethereum API and Tooling Compatibility**

If you're moving some portion of your smart contracts, state, or considering porting your full set of contracts off Ethereum to Phoenix, it should 'just work'. That is the full set of your application, contracts, and tooling will remain the same. Phoenix will be able to support:

- Solidity and Serpent Based Smart Contracts
- Ecosystem Tools (e.g., block explorers, front-end development libraries, wallets--i.e Metamask)
- Development Tools (e.g., Truffle, Remix, MetaMask, ethers, web3js)
- Ethereum Tokens via Bridges (e.g., token movement, state visibility, message passing)

You can view our tutorials to get a better feel for building Ethereum smart contracts on Phoenix, and how to directly offload or migrate your Ethereum application onto Phoenix.

As previously mentioned, Phoenix is proof of stake, this does mean that smart contracts that rely on components of Ethereum's API that touch on Proof of Work--difficulty, uncles, hash-rate won't work as expected on Phoenix. For those values, we have constant values set at the runtime level. Existing Ethereum contracts that rely on Proof of Work internals (e.g., mining pool contracts) will almost certainly not work as expected on Phoenix.

---

## **How Phoenix achieves Ethereum Compatibility**

We achieve Ethereum compatibility with three integrated components. If you're a smart contract developer, this may just be of passing interest.

- Pallet Ethereum: which allows for full Ethereum Block Processing
- SputnikEVM: You can view the full documentation [here:](https://docs.rs/evm/Pallet) 
- EVM: which allows you to deploy