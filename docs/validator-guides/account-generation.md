# <b>ACCOUNT GENERATION</b>
---

To start interacting with SwapDEX, you need a wallet or account that enables you to send transactions, participate in the chain governance, run a validator or nominate to earn interest. 

The following guide will show you how easy setting up a SwapDEX account is and what exact steps you need to take. 

If you encounter any issues or problems, please feel free to reach out to us on <a href="https://discord.gg/swapdex" target="_blank">Discord</a>.

---

Before we start, let's get some terms right, and let's spend a couple of seconds talking about the concept of keys.

A standard wallet or account is composed out of two keys:

- The {==PUBLIC KEY==} (Wallet address)
- The {==PRIVATE KEY==} (The key that allows you to access and manage your funds) 

Your SwapDEX account follows the same rules. Therefore, we will continue with a section on key security to make you aware that You must keep your PRIVATE KEY, SEED PHRASE, or JSON KEYSTORE file secret at all times. 

## **Storing your key safely**
---

The only ways to access your account are via your secret seed or your account's JSON file in combination with a password. It would be best if you kept them both secure and private. If you share them with anyone, they will have full access to your account, including all of your funds. This information is a target for hackers and others with bad intentions. 

In this guide, we recommend the **Polkadot{.js}** browser plug-in as the best method to create your SwapDEX account.

!!! tip
    <a href="https://polkadot.js.org/extension/" target="_blank"> Polkadot{.js} </a> Browser Extension RECOMMENDED FOR MOST USERS 

The seed is your key to the account. Knowing the seed allows you to re-generate and control your account or anyone else who knows the seed.

It is imperative to store the seed somewhere safe, secret, and secure. If you lose access to your account (i.e., forget the password for your account's JSON file), you can re-create it by entering the seed. This also means that somebody else can control your account if they have access to your seed.

For maximum security, the seed should be written down on paper or another non-digital device and stored in a safe place. You may also want to protect your seed from physical damage (e.g., keeping it in a sealed plastic bag to prevent water damage, storing it in a fireproof safe, etching it in metal, etc.) We recommend that you keep multiple copies of the seed in geographically separate locations (e.g., one in your home safe and one in a safety deposit box at your bank).

You should **not store your seed on any kind of computer that has or may have access to the internet in the future.**

## **Storing your account's JSON file**
---

The JSON file is encrypted with a password, which means you can import it into any wallet which supports JSON imports, but to then use it, you need the password. You don't have to be as careful with your JSON file's storage as you would with your seed (i.e. it can be on a USB drive near you), but remember that in this case, your account is only as secure as the password you used to encrypt it. Do not use easy to guess or hard to remember passwords. It is good practice to use a mnemonic password of four to five words. These are nearly impossible for computers to guess due to the number of combinations possible but much more manageable for humans to remember.

## **Polkadot{.js} Browser Extension**
---

Since Polkadot and SwapDEX share the same foundation, namely Substrate, the Polkadot{.js} browser extension is the recommended way to create your SwapDEX account.

- <a href="https://chrome.google.com/webstore/detail/polkadot%7Bjs%7D-extension/mopnmbcafieddcagagdcbnhejhlodfdd" target="_blank">Polkadot{.js} Browser Extension for Chrome & Brave </a>
- <a href="https://addons.mozilla.org/en-US/firefox/addon/polkadot-js-extension/" target="_blank">Polkadot{.js} Browser Extension for Firefox </a>

The Polkadot{.js} browser extension provides a reasonable balance of security and usability. It's comparable to MetaMask for Ethereum. It provides a separate local mechanism to generate your address and interact with SwapDEX.

So let's start installing the Polkadot{.js} plugin to enable you to interact with SwapDEX.


## **Create an Account**
---

Open the Polkadot{.js} browser extension by clicking the logo on the top bar of your browser. You will see a browser popup, not unlike the one below.

![browser_extension](assets/polkadot_plugin_js.png#center)

Click the big plus button or select "Create new account" from the small plus icon in the top right. The Polkadot{.js} plugin will then use system randomness to make a new seed for you and display it to you in the form of twelve words.

![browser_extenstion_01](assets/polkadot_plugin_js_new.png#center)

You should back up these words as explained above. It is imperative to store the seed somewhere safe, secret, and secure. If you cannot access your account via Polkadot{.js} for some reason, you can re-enter your seed through the "Add account menu" by selecting "Import account from pre-existing seed".

![browser_extenstion_02](assets/polkadot_plugin_js_new_03.png#center)

## **Name your Account**
---

The account name is arbitrary and for your use only. It is not stored on the blockchain and will not be visible to other users who look at your address via a block explorer. If you're juggling multiple accounts, it helps to make this as descriptive and detailed as needed.

## **Enter Password**
---

You will use the password to encrypt this account's information. You will need to re-enter it when using the account for all outgoing transactions or sign a cryptographic message.

!!! warning
    Note that this password does NOT protect your seed phrase. If someone knows the twelve words in your mnemonic seed, they still control your account even if they do not know the password.

!!! success
    Congrats! You managed to create a SwapDEX-Substrate Account with the Polkadot{.js} browser extention.

<br></br>

<p align=right> Written by Masterdubs & Petar </p>