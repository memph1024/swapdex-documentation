# <b> STOP VALIDATING</b>
---

If you wish to remain a validator or nominator (e.g. you're only stopping for planned downtime or server maintenance), submitting the `chill` extrinsic in the `staking` pallet should suffice. If you wish to unbond funds or reap an account, you should continue with the following.    

To ensure a smooth stop to validation, make sure you should do the following actions:

- Chill your validator
- Purge validator session keys
- Unbond your tokens

These can all be done with [Substrate Explorer App](https://substrate-explorer-testnet.swapdex.network/?rpc=wss%3A%2F%2Fswapdex.starkleytech.com%2Fws#/explorer) interface or with extrinsics.

## <b> Kill Validator </b>
---
To chill your validator or nominator, call the `staking.chill()` extrinsic. See the How to Chill page for more information. You can also claim your rewards at this time.

## <b> Purge validator session keys </b>
---
Purging the validator's session keys removes the key reference to your stash. This can be done through the `session.purgeKeys()` extrinsic with the controller account.

!!! warning
    **If you skip this step, you will not be able to reap your stash account**, and you will need to rebond, purge the session keys, unbond, and wait the unbonding period again before being able to transfer your tokens. See Unbonding and Rebonding for more details.


## <b> Unbond your Coins
---
Unbonding your tokens can be done through the Network > Staking > Account actions page in PolkadotJS Apps by clicking the corrosponding stash account dropdown and selecting "Unbond funds". This can also be done through the `staking.unbond()` extrinsic with the controller account.

<br></br>

<p align=right> Written by Masterdubs & Petar </p>