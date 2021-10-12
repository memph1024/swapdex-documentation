# <b>Become a Nominator</b>

If you are looking for a "set-it-and-forget-it" approach to leverage your TSDX coins, then becoming a SwapDex nominator is the way to go. 
As a nominator, you are participating in the staking system of SwapDex. Nominators use a specified amount of their funds to "vote" for SwapDex validators.
Validators are network nodes that participate in the consensus and block authoring process. Your job as a nominator is to appoint (stake) your TSDX to elect the active set of validators. The active validators list consists of validators that received the most TSDX as "votes". If your chosen validator makes it to the active set, he will earn rewards and ideally share them with all of his nominators.

The main difference between a **Validator** and a **Nominator** is the active participation in the network. Validators engage in the block production and finality mechanisms, whereas **nominators** take a more passive role with the above mentioned "set-it-and-forget-it" approach. Being a nominator does not require running a node of your own or worrying about online uptime. However, a good nominator performs due diligence on the validators that they elect. When looking for validators to nominate, a nominator should pay attention to their reward percentage for nominating a specific validator - as well as the risk that they bear of being slashed if the validator gets slashed.

## Setting up Controller and Stash Accounts

!!! info
    In this guide, we use the terms "account" and "wallet" interchangeably.

Nominators are recommended to set up two separate stash and controller accounts. Explanation and reasoning for generating distinct accounts for this purpose is elaborated in the [keys section](../04-Deep%20Dives/substrate_keys.md) of the Wiki.

You can generate your stash and controller account via any of the recommended methods that are detailed on the [account generation page](./account-generation.md).

!!! hint
    Payouts can go to any custom address. If you'd like to redirect payments to an account that is neither the controller nor the stash account, set one up. Note that it is extremely unsafe to set an exchange address as the recipient of the staking rewards.

## Using SwapDex Substrate Explorer

### Step 1: Bond your coins
On the [SwapDex Substrate Explorer](https://substrate-explorer-testnet.swapdex.network/?rpc=wss%3A%2F%2Fswapdex.starkleytech.com%2Fws#/explorer) UI navigate to the "Staking" tab (within the "Network" menu).

![browser_extension](assets/nominator_01.png#center)

The "Staking Overview" subsection will show you all the active validators and their information:

- (1) their identities
- (2) the amount of TSDX that are staking for them
- (3) amount that is their own provided stake 
- (4) how much they charge in commission
- (5) the era points they've earned in the current era
- (6) and the last block number that they produced. 

![browser_extension](assets/nominator_02.png#center)

If you click on the (7) chart button, it will take you to the "Validator Stats" page for that validator that shows you more detailed and historical information about the validator's stake, rewards, and slashes.

The "Account actions" subsection (link) allows you to stake and nominate.
Pick "Account actions" underneath "Network" > "Staking", then click the "+ Nominator" button.

![browser_extension](assets/nominator_03.png#center)

You will see a modal window that looks like the below:

![Become a Nominator](assets/Polkadot_Substrate_Portal.gif#center)

Select a "value bonded" that is less than the total amount of TSDX you have, so you have some leftover to pay transaction fees. Transaction fees are currently at least 0.01 TSDX, but they are dynamic based on various factors, including a load of recent blocks.

Also, be mindful of the reaping threshold - the amount that must remain in an account lest it be burned. That amount is 0.01 at SwapDex, so it's recommended to keep at least 0.1 TSDX in your account to be on the safe side.

Choose whatever payment destination makes sense to you. If you're unsure, you can choose "Stash account (increase amount at stake)" to simply accrue the rewards into the amount you're staking and earn compound interest.

![browser_extension](assets/nominator_04.png#center)

### Step 2: Nominate a Validator

You are now bonded. Being bonded means your tokens are locked and could be slashed if the validators you nominate misbehave. All bonded funds can now be distributed to up to 16 validators. Be careful about the validators you choose since you will be slashed if your validator commits an offense.

Click on "Nominate" on an account you've bonded, and you will be presented with another popup asking you to select some validators.

![Become a Nominator](assets/kusama_nominator_selection.png)

Select them, confirm the transaction, and you're done - you are now nominating. Your nominations will become active in the next era. Eras last 24 hours on SwaoDex - depending on when you do this, your nominations may become active almost immediately, or you may have to wait nearly the entire 24 hours before your nominations are active. You can check how far along SwapDex is in the current era on the Staking page.

Assuming at least one of your nominations ends up in the active validator set, you will start to get rewards allocated to you. To claim them (i.e., add them to your account), you must manually claim them. You can do it yourself or have the validator you staked to initiate a claim. This is to help optimize the effectiveness and storage of payouts on SwapDex. See the Claiming Rewards section of the Staking wiki page for more details.

!!! success
    Congrats! You successfully staked TSDX on the substrate side of the SwapDex Chain 

### Step 3: Stop Nominating

At some point, you might decide to stop nominating one or more validators. You can always change who you're nominating, but you cannot withdraw your coins unless you unbond them. Detailed instructions are available here.