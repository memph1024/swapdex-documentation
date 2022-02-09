# <b>INTERNAL TRANSFER OF BALANCES</b>
---
Cheers Friends, 

This guide will teach you how to transfer your funds from our "substrate" side to our Ethereum-Virtual-Machine (EVM).
Remember, the EVM side of our chain allows you to interact with many eth-based smart contracts and projects that eventually will move to SwapDex.
Moreover, our cross-chain bridges will be attached to our EVM module, so for you to utilize cross-chain trades, you must use the EVM.

That said, let me tell you how we tackle this.
First, I will walk you through the process to get started right away.
Second, I will elaborate a bit more on the concept so that the interested lads can read up.

## <b> PART 1 - WALKTHROUGH </b>
!!! Hint
    You can access the TRANSFER function here: <a href="https://app.swapdex.network/#/transfers" target="_blank"> Visit Dashboard </a> <br>
    You can watch me walk you through the process in this video: <a href="https://youtu.be/oZM_EoZgCAo" target="_blank"> Visit YouTube </a> 

The transfer is done in four (4) steps:

- Setup 
- Origin and Destination Address
- Amount    
- Confirmation

### <b> STEP 01 - Setup </b>

!!! Hint
    Make sure you have selected the "Transfers" function of our DApp (see picture below)

![img](assets/Internal-transfer-step-01.png#center)

!!! hint 
    You can change the transfer direction with the center button.
    Confirm the direction by clicking on next

![img](assets/Internal-transfer-step-011.png#center)

!!! Hint 
    Click on "Connect SwapDex Account" to connect your substrate wallet to the DApp. 
    This substrate address needs to hold the funds you wish to transfer.
    If you use the polkadot.js browser extension to manage your addresses, the DApp will offer you a dropdown menu.

![img](assets/Internal-transfer-step-012.png#center)

!!! hint
    Click next once you have selected your address.
    If you have trouble seeing the dropdown menu, please reload the page or close your browser and restart the process.

### <b> STEP 02 - Origin and Destination Address </b>

In this step, you can choose to either connect to your MetaMask wallet and automatically fetch the correct address, or you can paste any eth-based address.

![img](assets/Internal-transfer-step-02.png#center)

!!! hint
    Once you have selected a target address, you can confirm by clicking next.
    You need to confirm the selected accounts and click on "Ok".

![img](assets/Internal-transfer-step-021.png#center)

### <b> STEP 03 - Amount </b>

In this step, you need to select the amount you want to transfer and confirm by clicking on the "Transfer Amount" button.

!!! Warning
    It is generally advised to test a transaction with a small amount, especially if you are not familiar with the transfer function so far.
    The correct transfer of funds is your sole responsibility! No refunds are possible.

![img](assets/Internal-transfer-step-03.png#center)

!!! hint
    Open the substrate or EVM explorer to be prepared to check that the transfer went through successfully. <br>
    <a href="https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fws.swapdex.network#/explorer" target="_blank"> Substrate Explorer </a> <br>
    <a href="https://evm.swapdex.network/blocks" target="_blank"> EVM Explorer </a>

### <b> STEP 04 - Confirmation </b>

Confirm the transfer by hitting the "Sign and Send" button.

![img](assets/Internal-transfer-step-04.png#center)

!!! Hint
    Sign with your substrate account.

![img](assets/Internal-transfer-step-041.png#center)

!!! hint 
    If everything went fine, you are going to see a success message at the button of the page

![img](assets/Internal-transfer-step-042.png#center)

!!! hint 
    Now head over to the substrate explorer to check if the DApp successfully wrote the transfer into a block

![img](assets/Internal-transfer-step-043.png#center)


!!! Success
    Congrats, you successfully swapped SDX from the substrate to the EVM side.
    Feel free to reverse the swap and use the transfer function to your needs. 


## <b> PART 2 - WHAT HAPPENS IN THE BACKGROUND </b>
---

Many may ask themselves whether SwapDex is a single chain when it has both an ethereum and substrate side at the same time?
Well, to make a long story short... SwapDex is one single chain BUT it runs an ethereum simulation in parallel.

How does this work?

To answer this question, we need to look at the node architecture. Nodes are the pillars of our distributed network, and they run all the necessary code.

![img](assets/node-architecture.png#center)

I want to direct your focus to the SUBSTRATE RUNTIME module of the Substrate Node. 
The Runtime hosts all the code that makes SwapDex unique, and it's composed of code pallets. 
As you can see, the democracy function of our chain is also a code pallet and allows our community to govern the chain. Likewise, the staking pallet enables our community to run validators and stake as nominators. Like those pallets, the EVM is another pallet that allows our community to interact with eth-based smart contracts and cross-chain bridges.

By performing the transfer described in step 01, we are transferring coins from the SUBSTRATE RUNTIME ENVIRONMENT into the EVM pallet and vice versa, that's it :D.
As briefly touched on earlier, the EVM side opens up many new opportunities for you to utilize your SDX coins.

![img](assets/node-architecture-01.png#center)


<br></br>

<p align=right> Written by Masterdubs & Petar </p>

