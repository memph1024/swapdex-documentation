# <b>Balance Transfers</b>

Balance transfers are used to send TSDX balances from one account to another account. To start transferring balances, we will begin by using our [Substrate Explorer App](https://substrate-explorer-testnet.swapdex.network/?rpc=wss%3A%2F%2Fswapdex.starkleytech.com%2Fws#/explorer). 

!!! hint
    This guide assumes that you've already created an account and have some funds that are ready to be transferred.

## <b>Kusari Subtrate Explorer App</b>

Let's begin by opening [Substrate Explorer App](https://substrate-explorer-testnet.swapdex.network/?rpc=wss%3A%2F%2Fswapdex.starkleytech.com%2Fws#/explorer). There are two ways to make a balance transfer:

1. By using the "Transfer" tab in the "Accounts" dropdown
2. Clicking the "send" button while in the "Accounts" page

### <b>Using the Transfer Tab</b>

Click on the "Transfer" tab in the "Accounts" dropdown.

![Balance Transfer](assets/balance-transfer-01.png#center)

Now a pop-up window will appear on the page. The pop-up asks you to enter 3 inputs:

1. "send from account": Your account with funds that you will send from.
2. "send to address": The address of the account that will receive the funds.
3. "amount": The amount of tokens you will transfer.

![Balance Transfer](assets/balance-transfer-02.png#center)

After setting your inputs correctly, click the "Make Transfer" button and confirm. Once the transfer is included in a block you will see a green notification in the top-right corner of your screen.


### <b>Keep-Alive Checks</b>

At an extrinsic level, there are two main ways to transfer funds from one account to another. These are `transfer` and `transfer_keep_alive`. 
`transfer` will allow you to send TSDX regardless of the consequence; `transfer_keep_alive` will not allow you to send an amount that would allow the sending account to be removed due to it going below the existential deposit.

By default, [Substrate Explorer App](https://substrate-explorer-testnet.swapdex.network/?rpc=wss%3A%2F%2Fswapdex.starkleytech.com%2Fws#/explorer) will use `transfer_keep_alive`, ensuring that the account you send from cannot drop below the existential deposit (0.001666 TSDX). However, it may be that you do not want to keep this account alive (for example, because you are moving all of your funds to a different address). In this case, click on the "keep-alive" toggle at the bottom of the pop-up window.
The label should switch from "Transfer with account keep-alive checks"(`transfer_keep_alive` will be used) to "Normal transfer without keep-alive checks" (`transfer` extrinsic will be used). As a common use case for using normal transfers is to entirely clear out the account, a second toggle will appear if you have the keep-alive check turned off that will send all the tokens in the account, minus a transaction fee, to the destination address.

Attempting to send less than the existential deposit to an account with 0 TSDX will always fail, no matter if the keep-alive check is on or not. For instance, attempting to transfer 0.001 TSDX to an account you just generated (and thus has no TSDX) will fail, since 0.001 TSDX is less than the existential deposit of 0.001666 TSDX and the account cannot be initialized with such a low balance.

!!! warning
    Even if the transfer fails due to a keep-alive check, the transaction fee will be deducted from the sending account if you attempt to transfer.

### <b>Existing Reference Error</b>

If you are trying to reap an account and you recieve an error similar to "There is an existing reference count on the sender account. As such the account cannot be reaped from the state", then you have existing references to this account that must first be removed before it can be reaped. References may still exist
from:

- Bonded coins (most likely)
- Unpurged session keys (if you were previously a validator)
- Coin locks
- Existing recovery info
- Existing assets

### <b>Bonded Coins</b>

If you have coins that are bonded, you will need to unbond them before you can reap your account. Follow the instructions at Unbonding and Rebonding to check if you have bonded coins, stop nominating (if necessary) and unbond your coins.

### <b>Purging Session Keys</b>

If you used this account to set up a validator and you did not purge your keys before unbonding your coins, you need to purge your keys. You can do this by seeing the How to Stop Validating page. This can also be checked by checking `session.nextKeys` in the chain state for an existing key.

### <b>Checking for Locks</b>

You can check for locks by querying `system.account(AccountId)` under `Developer > Chain state`. Select your account, then click the "+" button next to the dropdowns, and check the relative `data`JSON object. If you see a non-zero value for anything other than `free`, you have locks on your account that need to get resolved.

You can also check for locks by navigating to `Accounts > Accounts` in [Substrate Explorer App](https://substrate-explorer-testnet.swapdex.network/?rpc=wss%3A%2F%2Fswapdex.starkleytech.com%2Fws#/explorer). Then, click the dropdown arrow of the relevant account under the 'balances' column. If it shows that some tokens are in a 'locked' state, you can see why by hovering over the information icon next to it.

### <b>Existing Recovery Info</b>

Currently, Kusari does not use the [Recovery Pallet](https://substrate.dev/docs/en/knowledgebase/runtime/frame#recovery), so this is probably not the reason for your coins having existing references.

On Kusari, you can check if recovery has been set up by checking the `recovery.recoverable(AccountId)` chain state. This can be found under `Developer > Chain state` in [Substrate Explorer App](https://substrate-explorer-testnet.swapdex.network/?rpc=wss%3A%2F%2Fswapdex.starkleytech.com%2Fws#/explorer).

### <b>Existing Non-TSDX Assets</b>

Currently, Kusari does not use the [Assets Pallet](https://substrate.dev/docs/en/knowledgebase/runtime/frame#assets), so this is probably not the reason for your tokens having existing references.

## <b>From the Accounts Page</b>

Navigate to the "Accounts" page by selecting the "Accounts" tab from the "Accounts" dropdown located on the top navigational menu of [Substrate Explorer App](https://substrate-explorer-testnet.swapdex.network/?rpc=wss%3A%2F%2Fswapdex.starkleytech.com%2Fws#/explorer).

You will see a list of accounts you have loaded. Click the "Send" button in the row for the account you will like to send funds from.

![Balance Transfer](assets/balance-transfer-03.png#center)

Now you will see the same pop-up window as if using the "Transfer" tab. Fill in the inputs correctly and hit "Make Transfer" then confirm the balance transfer. You will see a green notification in the top-right corner of the screen when the transfer is included in a block.