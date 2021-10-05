# Become a Nominator

If you are looking for a "set-it-and-forget-it" approach to leverage your TSDX coins, then becoming a SwapDex nominator is the way to go. 
As a nominator, you are participating in the staking system of SwapDex. Nominators use a specified amount of their funds to "vote" for SwapDex validators.
Validators are network nodes that participate in the consensus and block authoring process. Your job as a nominator is to appoint (stake) your TSDX to elect the active set of validators. The active validators list consists of validators that received the most TSDX as "votes". If your chosen validator makes it to the active set, he will earn rewards and ideally share them with all of his nominators.

The main difference between a **Validator** and a **Nominator** is the active participation in the network. Validators engage in the block production and finality mechanisms, whereas **nominators** take a more passive role with the above mentioned "set-it-and-forget-it" approach. Being a nominator does not require running a node of your own or worrying about online uptime. However, a good nominator performs due diligence on the validators that they elect. When looking for validators to nominate, a nominator should pay attention to their reward percentage for nominating a specific validator - as well as the risk that they bear of being slashed if the validator gets slashed.

## Setting up Controller and Stash Accounts

!!! info
    In this guide, we use the terms "account" and "wallet" interchangeably.

Nominators are recommended to set up two separate stash and controller accounts. Explanation and reasoning for generating distinct accounts for this purpose is elaborated in the keys section of the Wiki.

You can generate your stash and controller account via any of the recommended methods that are detailed on the [account generation page](./account-generation.md).

!!! hint
    Payouts can go to any custom address. If you'd like to redirect payments to an account that is neither the controller nor the stash account, set one up. Note that it is extremely unsafe to set an exchange address as the recipient of the staking rewards.

## Using Polkadot-JS UI

### Step 1: Bond your coins
On the Polkadot-JS UI navigate to the "Staking" tab (within the "Network" menu).

The "Staking Overview" subsection will show you all the active validators and their information - their identities, the amount of KSM that are staking for them, amount that is their own provided stake, how much they charge in commission, the era points they've earned in the current era, and the last block number that they produced. If you click on the chart button it will take you to the "Validator Stats" page for that validator that shows you more detailed and historical information about the validator's stake, rewards and slashes.

The "Account actions" subsection (link) allows you to stake and nominate.

The "Payouts" subsection (link) allows you to claim rewards from staking.

The "Targets" subsection (link) will help you estimate your earnings and this is where it's good to start picking favorites.

The "Waiting" subsection (link) lists all pending validators that are awaiting more nominations to enter the active validator set. Validators will stay in the waiting queue until they have enough KSM backing them (as allocated through the Phragmén election mechanism). It is possible validator can remain in the queue for a very long time if they never get enough backing.

The "Validator Stats" subsection (link) allows you to query a validator's stash address and see historical charts on era points, elected stake, rewards, and slashes.

Pick "Account actions" underneath "Network" > "Staking", then click the "+ Nominator" button.

You will see a modal window that looks like the below:

![Become a Nominator](assets/kusama_nominator_popup.png)

Select a "value bonded" that is less than the total amount of KSM you have, so you have some left over to pay transaction fees. Transaction fees are currently at least 0.01 KSM, but they are dynamic based on a variety of factors including the load of recent blocks.

Also be mindful of the reaping threshold - the amount that must remain in an account lest it be burned. That amount is 0.01 in Kusama, so it's recommended to keep at least 0.1 KSM in your account to be on the safe side.

Choose whatever payment destination that makes sense to you. If you're unsure, you can choose "Stash account (increase amount at stake)" to simply accrue the rewards into the amount you're staking and earn compound interest.

![Become a Nominator](assets/kusama_nominator_dropdown.png)

### Step 2: Nominate a Validator

You are now bonded. Being bonded means your tokens are locked and could be slashed if the validators you nominate misbehave. All bonded funds can now be distributed to up to 16 validators. Be careful about the validators you choose since you will be slashed if your validator commits an offence.

Click on "Nominate" on an account you've bonded and you will be presented with another popup asking you to select some validators.

![Become a Nominator](assets/kusama_nominator_selection.png)

Select them, confirm the transaction, and you're done - you are now nominating. Your nominations will become active in the next era. Eras last six hours on Kusama - depending on when you do this, your nominations may become active almost immediately, or you may have to wait almost the entire six hours before your nominations are active. You can check how far along Kusama is in the current era on the Staking page.

Assuming at least one of your nominations ends up in the active validator set, you will start to get rewards allocated to you. In order to claim them (i.e., add them to your account), you must manually claim them. To initiate a claim, you can do it yourself or have the validator that you staked for initiate a claim. This is to help optimize the effectiveness and storage of payouts on Kusama. See the Claiming Rewards section of the Staking wiki page for more details.

### Step 3: Stop Nominating

At some point, you might decide to stop nominating one or more validators. You can always change who you're nominating, but you cannot withdraw your tokens unless you unbond them. Detailed instructions are available here.