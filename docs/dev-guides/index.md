# <b>Smart Contracts</b>
---
On Kusari you have two out-of-the-box solutions to kickstart your Smart Contract development.

- The EVM (Ethereum Virtual Machine) pallet, which presents an Ethereum compatibility layer
- The Contracts pallet, which is a FRAME library for Wasm based smart contracts

## **EVM Contracts by Frontier**
---
[Frontier](https://paritytech.github.io/frontier/) is the Ethereum compatibility layer for Substrate chains. It enables Kusari to run Ethereum contacts (EVM) natively, that means for instance that all existing Ethereum RPC methods work, so none of your dapps will break. Furthermore, it allows you to run your EVM smart contract natively in Substrate, tightly integrated with the rest of the Substrate ecosystem.

Read more in our EVM Pallet Deep Dive!

## **Contracts Pallet**
---
The experience of deploying to an EVM-based chain may be more familiar to developers that have written smart contracts before. However, the Contracts pallet makes some notable improvements to the design of the EVM:

- **Wasm.** The Contracts pallet uses WebAssembly as its compilation target. Any language that compiles to Wasm can potentially be used to write smart contracts. Nevertheless, it is better to have a dedicated domain-specific language, and for that reason Parity offers the ink! language.

- **Rent.** Contracts must pay rent or else hold a deposit suitably large enough in order to justify its existence on-chain. When a contract does not uphold this, it may create what's called a tombstone which is a reference to the contract. In some conditions, the contract will be deleted outright along with its storage if it does not maintain these requirements.

- **Caching.** Contracts are cached by default and therefore means they only need to be deployed once and afterward be instantiated as many times as you want. This helps to keep the storage load on the chain down to the minimum. On top of this, when a contract is no longer being used and the existential deposit is drained, the code will be erased from storage (known as reaping).


## **Ink**
---
[ink!](https://github.com/paritytech/ink) is a domain specific language for writing smart contracts in Rust and compiles to Wasm code. As it states in its README, it is still in an experimental phase so brave developers should be aware that they might have a bumpy - but workable - development experience. There are some projects that have built projects in ink! with a decent level of complexity such as Plasm's [Plasma contracts](https://github.com/staketechnologies/Plasm), so it is mature enough to start building interesting things.

For interested developers, they can get started writing smart contracts using ink! by studying the [examples](https://github.com/paritytech/ink/tree/master/examples) that were already written. These can be used as guideposts to writing more complex logic that will be deployable on smart contract parachains.

ink! has laid much of the groundwork for a new smart contract stack that is based on a Wasm virtual machine and compatible with Substrate chains.

**Parity Tech**

- [ink!](https://github.com/paritytech/ink)
- [Contracts pallet](https://github.com/paritytech/substrate/tree/master/frame/contracts)


