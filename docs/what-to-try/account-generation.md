# <b>Account Generation</b>

An address is the public part of a SwapDex account. The private part is the key used to access this address. The public and private parts together make up a SwapDex-Substrate account.

There are several ways to generate a SwapDex-Substrate accounts but we recommend the following procedure:

!!! tip
    Polkadot{.js} Browser Extenstion RECOMMENDED FOR MOST USERS

## Disclaimer: Key Security

The only ways to access your account are via your secret seed or your account's JSON file in combination with a password. It would be best if you kept them both secure and private. If you share them with anyone, they will have full access to your account, including all of your funds. This information is a target for hackers and others with bad intentions. 

On this page, we recommend various account generation methods that have different convenience and security tradeoffs. Please review this page carefully before making your account to understand the risks of the account generation method you choose and how to mitigate them to keep your funds safe properly.

## Storing your key safely

The seed is your key to the account. Knowing the seed allows you to re-generate and control this account or anyone else who knows the seed.

It is imperative to store the seed somewhere safe, secret, and secure. If you lose access to your account (i.e., forget the password for your account's JSON file), you can re-create it by entering the seed. This also means that somebody else can control your account if they have access to your seed.

For maximum security, the seed should be written down on paper or another non-digital device and stored in a safe place. You may also want to protect your seed from physical damage (e.g., keeping it in a sealed plastic bag to prevent water damage, storing it in a fireproof safe, etching it in metal, etc.) We recommend that you keep multiple copies of the seed in geographically separate locations (e.g., one in your home safe and one in a safety deposit box at your bank).

You should **not store your seed on any kind of computer that has or may have access to the internet in the future.**

## Storing your account's JSON file

The JSON file is encrypted with a password, which means you can import it into any wallet which supports JSON imports, but to then use it, you need the password. You don't have to be as careful with your JSON file's storage as you would with your seed (i.e. it can be on a USB drive near you), but remember that in this case, your account is only as secure as the password you used to encrypt it. Do not use easy to guess or hard to remember passwords. It is good practice to use a mnemonic password of four to five words. These are nearly impossible for computers to guess due to the number of combinations possible but much more manageable for humans to remember.

## Polkadot{.js} Browser Extension
Since Polkadot and SwapDex share the same foundation, namely Substrate, the Polkadot{.js} browser extension is a recommended way to create SwapDex-Substrate accounts.

- [Polkadot{.js} Browser Extension for Chrome & Brave](https://chrome.google.com/webstore/detail/polkadot%7Bjs%7D-extension/mopnmbcafieddcagagdcbnhejhlodfdd)
- [Polkadot{.js} Browser Extension for Firefox](https://addons.mozilla.org/en-US/firefox/addon/polkadot-js-extension/)

The Polkadot{.js} browser extension provides a reasonable balance of security and usability. It provides a separate local mechanism to generate your address and interact with Polkadot.
This method involves installing the Polkadot{.js} plugin and using it as a "virtual vault," separate from your browser, to store your private keys. It also allows the signing of transactions and similar functionality.
It is still running on the same computer you use to connect to the internet and thus is less secure than using Parity Signer or other air-gapped approaches.

## Create an Account

Open the Polkadot{.js} browser extension by clicking the logo on the top bar of your browser. You will see a browser popup, not unlike the one below.

![browser_extension](assets/polkadot_plugin_js.png#center)

Click the big plus button or select "Create new account" from the small plus icon in the top right. The Polkadot{.js} plugin will then use system randomness to make a new seed for you and display it to you in the form of twelve words.

![browser_extenstion_01](assets/polkadot_plugin_js_new.png#center)

You should back up these words as explained above. It is imperative to store the seed somewhere safe, secret, and secure. If you cannot access your account via Polkadot{.js} for some reason, you can re-enter your seed through the "Add account menu" by selecting "Import account from pre-existing seed".

![browser_extenstion_02](assets/polkadot_plugin_js_new_03.png#center)

## Name Account

The account name is arbitrary and for your use only. It is not stored on the blockchain and will not be visible to other users who look at your address via a block explorer. If you're juggling multiple accounts, it helps to make this as descriptive and detailed as needed.

## Enter Password

You will use the password to encrypt this account's information. You will need to re-enter it when using the account for all outgoing transactions or sign a cryptographic message.

!!! warning
    Note that this password does NOT protect your seed phrase. If someone knows the twelve words in your mnemonic seed, they still control your account even if they do not know the password.

!!! success
    Congrats! You managed to create a SwapDex-Substrate Account with the Polkadot{.js} browser extention.