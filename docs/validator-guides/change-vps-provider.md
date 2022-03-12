# <b>HOW TO CHANGE YOUR VPS PROVIDER</b>
---

The need to change a VPS provider can occur out of many reasons. 
Regardless why you decided to change your provider, this guide will show you how to perform all necessary steps in the correct order to not risk the staked SDX.

We will perform the transition in 6 steps:

1. Prepare the new VPS
2. Generate new Session Keys on the new VPS
3. Pair Validator Stash with new the VPS
4. Waiting Period
5. Shut down old VPS

!!! warning
    1. Do not copy the Keystore folder of the old VPS to the new VPS!
    2. Do not use the same session key!
    3. Do not wipe your current VPS before you completed all steps in this guide.

---
## Step 1 - Prepare new VPS

To enable a smooth transition bring your new VPS server to a fully synced state. 
You can follow the <a href="https://docs.swapdex.network/validator-guides/validator/" target="_blank">**Become a Validator**</a> guide to prepare your new VPS.  

Once your new VPS is fully synced you can continue with the next steps.

---

## Step 2 - Generate New Session Keys

Login to your new and fully synced VPS.

Execute the following command to fetch a new session key:

```
curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}' http://localhost:9933
```

Copy the session key. It will look like this:

```
0x13660593581b2e728ee32122636f8996c6fd9c22f33beaa05e2797899c5458b0c888149bf3c0b5ca7fb7296e69fefd85e4e3d5b76848db890207575e49031f37d846e78babf8051c123b498ffe6f12e712f97f6b2f3b54345ffe51145a16bb22187d415c2101b9883668ce93c46f7ba556b394c59781854737b6c941747c0964
``` 

---

## Step 3 - Pairing (Set Keys)

Head to the <a href="https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fws.swapdex.network#/extrinsics" target="_blank">**Substrate Portal (Chain UI)**</a> account actions tab `Network -> Staking -> Account Actions` or simply follow the aforementioned link. 

Here we want to pair our new VPS with the current validator stash.
The session key from step two will become the link between our validator stash and our new VPS. Therefore, we must change the session keys for our validator stash.
We do this by clicking on the menu (three dots) button displayed on the right end of our stash and then clicking the **change session keys** item.

![img](assets/change-vps-step-03_2.png#center)

Paste the session key generated in **Step 2** into the respective field of the pop-up menu and click the **Set Session Key** button.

![img](assets/change-vps-step-03_3.png#center)

!!! hint
    **Take a note of the era you are currently in** 


---

## Step 4 - Waiting Period

The `set_key` command does not have an immediate effect and requires **two full eras** to elapse before it does. If you do switch off your old VPS too early you may risk being chilled for the upcoming era. 

Example: If we clicked on the **Set Session Key** button halfway through era number 10 we must wait until era 10 elapses and two more eras passed by. Which means, we would be good to go in era 13.

Read about the consequences of getting chilled <a href="https://docs.swapdex.network/deep-dives/staking/#what-are-some-root-causes-for-getting-slashed" target="_blank">**here**</a>.

Usually being chilled means your validator will be inactive and won't perform any <a href='https://docs.swapdex.network/validator-guides/validator-payout-overview/' target='_blank'>payable actions</a> during the upcoming era. But in case more than 10% of the network is getting chilled at the same time you might face penalties.


---

## Step 5 - Shutting down your old VPS

When two full eras passed we can be sure that our new VPS has taken over, and we can safely shut down our old VPS.


!!! success
    Alright mate! You are all set :D

<br></br>

<p align=right> Written by Masterdubs & Petar </p>