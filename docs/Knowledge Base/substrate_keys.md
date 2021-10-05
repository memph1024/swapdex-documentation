# Substrate Keys

Public and private keys are an essential aspect of most crypto-systems and an essential component that enables blockchains like Polkadot to exist.

## Account Keys

Account keys are keys that are meant to control funds. They can be either:

- The vanilla ```ed25519``` implementation using Schnorr signatures.
- The Schnorrkel/Ristretto ```sr25519``` variant using Schnorr signatures.
- ECDSA signatures on ```secp256k1```

There are no differences in security between ```ed25519``` and ```sr25519``` for simple signatures.

We expect ```ed25519``` to be much better supported by commercial HSMs for the foreseeable future.

At the same time, ```r25519``` makes implementing more complex protocols safer. In particular, ```sr25519``` comes with safer version of many protocols like HDKD common in the Bitcoin and Ethereum ecosystem.

## "Controller" and "Stash" Keys

When we talk about "controller" and "stash" keys, we usually talk about them in the context of running a validator or nominating TSDX. Still, they are valuable concepts for all users to know. Both keys are types of account keys. They are distinguished by their intended use, not by an underlying cryptographic difference. When creating new controller or stash keys, all cryptography supported by account keys is an available option.

The **controller key** is a semi-online key that will be in the user's direct control and used to submit manual extrinsics. This means that validators and nominators will use the controller key to start or stop validating or nominating. 

!!! warning
    Controller keys should hold some TSDX to pay for fees, but You should not use them to hodl massive amounts or life savings since the blockchain will expose them to the internet with relative frequency. You should treat them carefully and occasionally replaced them with new ones.

The **stash key** is a key that will, in most cases, be a cold wallet, existing on a piece of paper in a safe or protected by layers of hardware security. It should rarely if ever, be exposed to the internet or used to submit extrinsics. The stash key is intended to hold a large amount of funds. You can compare it to a savings account at a bank, which ideally is only ever touched in urgent conditions. Or, perhaps a more apt metaphor is to think of it as buried treasure, hidden on some random island and only known by the pirate who initially hid it.