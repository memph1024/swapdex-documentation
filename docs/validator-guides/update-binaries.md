# <b>HOW TO UPDATE VALIDATOR BINARIES</b>

---

The following guide will teach you how to conduct a binary update for your validator.
Binary updates are rare SwapDEX but essential from time to time to ensure that your validator can participate in the consensus mechanism and is able to function properly.

Learn more about the reasons behind this update in the following section.

## Why is this update needed? (Announcement)

!!! quote
    The anticipated launch of Kazama Swap on our Network has made a significant step forward.

    Today we like to inform you that the dev teams have made fantastic progress in the background, testing, and innovating on the Kusari Network.

    One outcome of the rigorous testing on Kusari was a significant improvement in our blockchain's underlying technology, namely the p2p network communication layer. The dev teams managed to enhance the standard substrate module to unleash the full potential of Kazama Swap on our EVM.

    So what does this mean for you? Well, this improvement renders us even more unique than other projects that simply adapt the substrate framework, and for our validator community, there is now some action to do.

    Since the P2P network layer is not part of the runtime, it cannot be updated automatically, meaning you need to download and install a new binary. This is not a fork because the old binaries will still work. However, the moment we update the runtime forklessly, your validators will have trouble interfacing with the new p2p network technology and won't be able to author blocks. To address any potential concerns, they won't suffer any slash penalties if they are not updated, but they will need to be updated to continue to author blocks.

    We shall release step-by-step instructions on how to update your validators accordingly, but for now, we would like to take the time to appreciate the outstanding efforts of our partners and development team.

## The Process

![img](assets/update_process.png#center)

!!! warning
Make sure to update your OS to Ubuntu 21.10 since the new binaries are compiled for that Ubuntu version. You can follow our <a href="https://youtu.be/n_KXf_fEOgE" target="_blank" >online guide</a> to conduct the OS update. Other OS versions might work as well but are under your own risk.

## Step 1 - Pause / Chill your Validator

---

Visit the <a href="https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fws.swapdex.network#/staking/actions" target="_blank">substrate portal</a> and navigate to the staking tab and select accounts.

Click the Stop Button and sign the transaction accordingly.
![img](assets/update_step_1_1.png#center)

After the transaction was signed you should see the following validation screen.
![img](assets/update_step_1_2.png#center)

!!! success
    You successfully paused / chilled your validator and can proceed to step 2

## Step 2 - Purge current Session Keys

---

Navigate to the <a href="https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fws.swapdex.network#/extrinsics" target="_blank">Developer Tab</a> and select Extrinsic then select your controller account and the session pallet in the dropdown menu.

The session pallet offers two extrinsics:

1. PurgeKeys()
2. SetKeys()

Select PurgeKeys() and click hit Submit Transaction.

![img](assets/update_step_2_1.png#center)

!!! success
    You successfully purged your current session keys and can proceed to step 3

## Step 3 - Execute Update Command on your Server

---

Login to your server as root.

!!! warning
    The following command will clean and delete the current binary and download and install a new but specific binary. If you are not sure about the version of your binary pls reach out to our validator community on <a href="https://discord.gg/swapdex" target="_blank" >Discord</a>.

```
 service swapdex stop && wget https://download2.starkleytech.com/swapdex/swapdex_ubuntu_21_10-swapdex-polkadot-v0.9.18-bdfbeb9e-980 && chmod 777 swapdex_ubuntu_21_10-swapdex-polkadot-v0.9.18-bdfbeb9e-980 && mv swapdex_ubuntu_21_10-swapdex-polkadot-v0.9.18-bdfbeb9e-980 /usr/bin/swapdex -f && runuser -l  swapdex -c 'swapdex purge-chain --chain swapdex -y'
```

!!! success
    You successfully deleted and cleared your old binary and installed the new binary

## Step 4 - Start Validator Service on your Server

---

Execute the following command to start your validator and the sync process.

```
service swapdex start && journalctl -f -u swapdex
```

!!! success
    You successfully started the new binary

Since we cleared / purged the old binaries' data your validator need to download the full database / blocks again. This sync process should not take long thanks to the improvements in the P2P Network Layer.

However, make sure to fully sync your validator before you restart your validator on the substrate portal. If your validator is not in sync, and you get elected to the active set you will miss opportunities to author blocks and might get chilled as a consequence.

You can observe the progress either in the logs or on our telemetry UI : <a href="https://telemetry.polkadot.io/#stats/0x15bac4f0a9aad3f46c5fc067fdb59b3ff29738dcd491fe5e37b4b76121163471" target="_blank"> Telemetry UI </a>

!!! success
    You successfully completed step 4

## Step 5 - Generate new Session Keys

---

Login to your VPS server.

Session keys are needed to associate your node with your controller account. To generate the session keys you can run the following command in your terminal:

!!! hint
    Hit ++ctrl+c++ To exit the logs showing up on your server

```
curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}' http://localhost:9933
```

The output will have a hex-encoded "result" field. The result is the concatenation of the four public keys. Save this result for a later step.
Copy the session key. It will look like this:

```
0x13660593581b2e728ee32122636f8996c6fd9c22f33beaa05e2797899c5458b0c888149bf3c0b5ca7fb7296e69fefd85e4e3d5b76848db890207575e49031f37d846e78babf8051c123b498ffe6f12e712f97f6b2f3b54345ffe51145a16bb22187d415c2101b9883668ce93c46f7ba556b394c59781854737b6c941747c0964
```

!!! success
    You successfully generated your new session keys

## Step 6 - Restart Validator on Substrate Portal

---

Visit the <a href="https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fws.swapdex.network#/staking/actions" target="_blank">substrate portal</a> and navigate to the staking tab and select accounts.

Click the Session Key button and enter your key from the previous step.

![img](assets/update_step_6_1.png#center)

Finally, set Session Keys and sign the transaction and your validator should join the active set without any issues.

![img](assets/update_step_6_2.png#center)

Finally, click validate and set your validator preferences. Keep in mind that some users may nominate you because of your commission settings, so it's recommended to keep the former commission settings or inform your nominators that you plan to change the percentage.

![img](assets/update_step_6_3.png#center)

![img](assets/update_step_6_4.png#center)

!!! success
    You successfully finished the update process. Congrats mate!

<br></br>

<p align=right> Written by Masterdubs & Petar </p>
