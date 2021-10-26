# **STAKING**
---

This guide will introduce you to the concept of staking on the SwapDex and Kusari network.
SDX and KSI are utilizing a {=Proof-of-Stake=} consensus mechansim to agree on processes like block authoring and finality. 
As the name implies, **staking** is playing a essential role on both networks.  

In short, you have two options to stake. Either you stake in the form of nomination, or you stake in the form of running a validation. 
But staking, in general, means that you bind your SDX or KSI holdings for a specific purpose and time to receive rewards in return. 

This guide will mainly focus on helping you decide to either become a nominator or a validator and gives you some examples of the rewards you could expect.

!!! hint "Validator"
    The staking system on SwapDex and Kusari is designed to pay out rewards equally to all validators, regardless of their stake. That means the amount of staked coins on a validator does not influence its ability to author or validate more blocks. However, there is a probabilistic component to reward calculation (discussed below), so rewards may not be exactly equal for all validators in a given era.

!!! hint "Nominator"
    The rewards for nominators are payed out pro-rata after the validator reward is deducted. That means your share of the total nominator rewards per validator will increase with the amount of coins you staked on that specific validator. This system should motivate nominators to stake on lower-staked validators and thus should create a balanced-staked validator set.

## **How does Staking work in Smart DEX Chain and Kusari**
---
### **Identify which role you are**

In staking, you can be either a nominator or a validator.

As a nominator, you can nominate validator candidates that you trust to help you earn rewards in the chain's native coin. You can look at the [nominator guide](../what-to-try/nominator.md) to understand your responsibilities as a nominator and at the [validator guide](../what-to-try/validator.md) to understand what you need to do as a validator.

### ** Nomination Period**

Anyone who wants to become a validator can decide to become a validator candidate at any time.
The candidacy is made public to all nominators, who can vote (nominate) for their favorite validator candidates.

At the beginning of a new era, the network selects the highest nominated validator candidates, becoming the active set for the forthcoming era.

!!! warning "Caution as a Nominator"
    **Note** that there are no prerequisites to candidate as an validator. Therefore, we strongly advise to take a close look at the performance and reputation of the validators you want to stake on. **Nomination is not a set-and-forget approach**

### ** Staking Rewards Distribution**

To explain how rewards are paid to validators and nominators, we need to consider **validator and nominator pools**. A validator pool consists of the stake of an elected validator together with the nominators backing it. The nominator pool consists of the sum of nominated coins for that validator.

As you can see in the illustration below, every validator pool receives essentially the same amount of coins for equal work. The network distributes the coins at the end of each era. However, there is a probabilistic component to staking rewards in the form of era points and tips but these should average out over time.

Every validator can set a customized commission and the rest is payed **pro-rata** (proportional to stake) to the nominators. 

!!! note "Validator"
    If a validator stakes on his validator node, his stake counts as self-nomination and is getting pro-rata rewards from the nominator pool.

Notice in particular that the validator is rewarded twice: once in commission fees for validating (if their commission rate is above 0%), and once for nominating itself with stake. If a validator's commission is set to 100%, no coins will be paid out to any nominations in the nominator pool.

![img](assets/nominator-rewards.svg#center)

### ** Rewards Mechanism**

By now, two concepts should be clear:

- Every validator pool is rewarded equally regardless of the total stake captured by that validator pool
- Smaller validator pools tend to reward nominators more per coin then pools with more stake. 

Notice that the reward mechanism is giving nominators an economic incentives to favor smaller validator pools.
The reason behind that is to prevent a concentration of power among a small set of validators. 

Furthermore, it is in alignment with the principle of risk and reward. The smaller the validator pool, the higher the risk that the validator may have a bad reputation or inferior performance and the higher the reward per staked coin.

Notice as well, that there is no limitation of nominators that can stake on a validator, but there is a limitation of nominators a validator can pay rewards to. This condition is called **oversubscribed**. That said, once a payout on an oversubscribed validator takes place only the top number of nominators are considered. So pay attention for signs of oversubscription.

!!! warning "Warning for Validators and Nominators"
    We also remark that when the network slashes a validator slot for a misbehavior (e.g. validator offline, equivocation, etc.) the slashed amount is a fixed percentage (and NOT a fixed amount), which means that validator pools with more stake get slashed more coins. Again, this is done to provide nominators with an economic incentive to shift their preferences and back less popular validators whom they consider to be trustworthy.

Another point to note is that each validator candidate is free to set their desired commission fee (as a percentage of rewards) to cover operational costs. Since the network pays all validator pools the same, validator pools with lower commission fees pay more to nominators than pools with higher fees. Thus, each validator can choose between increasing their fees to earn more or decreasing their fees to attract more nominators and increase their chances of being elected. In the long term, we expect that all validators will need to be cost-efficient to remain competitive. Validators with a higher reputation will charge slightly higher commission fees (which is fair).

## **Accounts**
---

To add an extra layer of protection we advise to create two types of accounts:

- Controller Account
- Stash Account

The **Stash** account is your cold wallet which holds your funds and delegates some functions to the controller account. A cold wallet can be completely offline and is usually used to store crypto. With this system of two wallets, we want to minimize the touchpoints with your stash account to minimize potential risks. 

The **Controller** account is your hot wallet and acts on behalf of your stash account. The controller is entitled to signal decisions about nominating and validating. For instance, it sets preferences for payout account and commission if you are a validator, the controller also sets your session key if you have enough funds to pay for the transaction fees of cause.
<br>
</br>
![img](assets/accounts.png#center)

## **Validator and Nominators**
---

As we learned, validator slots are limited and they do most of the heavy lifting: they produce new block candidates in BABE (block production mechanism), vote, and reach a consensus in GRANDPA (block finality mechanism). In essence, validators validate the state transition function of the blockchain.

Nominators, on the other hand, have far fewer responsibilities. Those include monitoring their validators' performance (uptime), keeping an eye on changing commission rates (a validator can change commission at any time), and general health monitoring of their and their validators' account. 

<br></br>

![img](assets/nominator-validator.png#center)

!!! tip "Expertise needed"
    While not set-it-and-forget-it, the experience needed to become a nominator is less than the expertise required to be a reliable validator.

## **Slashing**
---

Slashing is an important concept to protect the network.

The network has two fundamental requirements that it expects from every validator:

- Be online
- Stay honest

Slashing will happen if the systems detects that a validator misbehaves (e.g., goes offline, attacks the network, or runs modified software) in the network. The validator and its nominators will get slashed in realation to the offense by losing a percentage of their bonded/staked coins.

The network will add any slashed coin to the Treasury. The rationale for this (rather than burning or distributing them as rewards) is that the Council may revert slashes by simply paying out from the Treasury.

The option to revert slashes would be helpful in situations such as a faulty runtime causing slashing or forcing validators offline through no fault of their own. 

In the case of legitimate slashing, it moves tokens away from malicious validators to those building the ecosystem through the normal Treasury process.


!!! warning "Slashing"
    It is essential to realize that slashing only occurs for active validations. Each validator is considered its entity for purposes of slashing, just as they are for staking rewards.

### **What are some root causes for getting slashed?**

We group the root causes into three distinct clusters:

1. **Liveliness**
2. **Equivocation**
3. **Misconduct**

**Liveliness**

Liveliness is concerned about the ability of the validaor to be online and responsive. 
If a validator misses to produce a block or send a heatbeat signal during an era the validator will be considered as offline and will be but on chill.
In that case, no slashing of coins takes place but since the validator did not contribute any work there are also no block rewards payed out to the validator pool. 

**Equivocation**

Multiple signing in the consensus mechanism protocols. To elaborate a bit further:
The network expects that only one validator validates a block by pointing to the block and signing it.
If multiple validators claim to have signed the most recent block we have a case of equivocation, where other validators essentially dublicates the effort of an honest validator.

In the case of equivocation the penality is calculated in a manner that takes into account the likelyhood of beeing an coordinated attack or not. 

The penalties are calculated by the following formular


```
let x = offenders, n = total number of validators in the active set

penality = MIN((3*x/n)^2 , 1)
```

=== "Example 01"

    ``` 
    Assume that there are 100 validators in the active set, and one of them equivocates in a slot. 
    This is unlikely to be an attack on the network, but much more likely to be a misconfiguration of a validator

    x = 1, n = 100

    penality = MIN((3*1/100)^2 , 1) = MIN(0,0009, 1) = 0,0009 =  0,09%

    The slash will apply on this validator and the the validator will be chilled.
    ```

=== "Example 02"

    ``` 
    Now assume that there is a group of 5 validators, and all of them have an issue in the same slot.

    x = 5, n = 100

    penality = MIN((3*5/100)^2 , 1) = MIN(0,0225, 1) = 0,0225 =  2,25%

    The slash will be applied to the 5 validators pools and their nominators. All slashed validators will also be chilled.
    
    ```
=== "Example 03"
    ```
    Now assume that there is a group of 20 validators, and all of them have an issue in the same slot.

    x = 20, n = 100

    penality = MIN((3*20/100)^2 , 1) = MIN(0,36, 1) = 0,36 =  36,0%

    The slash will be applied to the 20 validators pools and their nominators. All slashed validators will also be chilled.
    ```

**Misconduct**

Misconducts are behavious that poses a security or monetary risk to the network or can cause mass collusions.
Therefore, misconducts are the highest punished behaviours in the Kusari and Smart DEX Chain networks. 
Reported and confirmed slashes in that category can lead to the transfer of the funds inside the validator pool to the treasury and could ulitmately be burned.  

!!! warning
    If a validator is reported for any one of the offences they will be removed from the validator set (chilled) and they will not be paid while they are out. They will be considered inactive immediately and will lose their nominators. They need to re-issue intent to validate and again gather support from nominators.


### **Slashing Consequences**

|              | Isolated Event                                                 | >10% of validators offending                                                                               | >33% of validators offending                                |
|:------------:|----------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------|
| Liveliness   | Chill, no slash                                                | Chill, plus penalties that increase linearly with the number of corresponding validators that are offline  | Slash increases, requires 44% to attain maximum slash of 7% |
| Equivocation | Chill, plus slash of 0,0004% (assuming 1500 active validators) | Slash, increases in a linear manner                                                                        | Maximum slash of 100%                                       |
| Misconduct   | 100% Slash                                                     | 100% Slash                                                                                                 | 100% Slash                                                  |

!!! warning
    All slashes include loss of all nominaors!

### **Chilling**

Chilling is the act of stepping back from any nominating or validating. It can be done by a validator or nominator at any time themselves, taking effect in the next era. It can also specifally mean removing a validator from the active validator set by another validator, disqualifying them from the set of electable candidates in the next NPoS cycle.

Chilling may be voluntary and validator-initiated, e.g. if there is a planned outage in the validator's surroundings or hosting provider, and the validator wants to exit to protect themselves against slashing. When voluntary, chilling will keep the validator active in the current session, but will move them to the inactive set in the next. The validator will not lose their nominators.

When used as part of a punishment (initiated externally), being chilled carries an implied penalty of being un-nominated. It also disables the validator for the remainder of the current era and removes the offending validator from the next election.

---
### **Potential causes for liveliness slashes**

Let's have a look on events that can cause a slash under liveliness.
To break this down, we differentiate between two scenarios:

- You have an existing server
- You install a new server or migrate to a new server

**Existing Server**
 
 Potential causes are:
 - Server is offline
 - Firewall or network connectivity prevents communicaiton to the network
 - SDD is full and prevents further syncing

**New Server / Mirgration**

Potential causes are:
- The validator instance may not be started with the `-- validator` flag
- If you switch server you may not have applied the correct session key
- Your node might not be fully syncronized

### **How to mitigate the risk of being slashed on liveliness?**

Let's have look into what operators can do to reduce the risk of getting slashed under liveliness.

- Monitoring
    - Using monitoring tools like grafana and prometheus, together with PagerDuty for alerts
    - Trigger alerts when the service is unreachable or when blocks are stalled (e.g, 100 blocks)
- Checklists
    - Ensure that session-keys are verified on the destination server
    - Ensure that the `--validator` flag exists
    - Understand server migration wait times. It takes at least 2 eras before new keys are applied

---

### **Potential causes for equivocation slashes**

Equivocation slashes are caused if multiple validators claim to have signed the same block.
This can be cause by:

- If you cloned a validator to be used as a backup or during the migration of your validator to a new server.
- Using a failover system to provide "zero-downtime"
- Copying the keystore folder when trying to copy the database 

### **How to mitigate the risk of being slashed on equivocation?**

- Never copy your **Keystore** folder to another server
- If your server went offline spontaneously, do not panic and attempt to restore a cloned server to minimize downtime
- Use a backup server, if it is not fully synced use a snapshot to do so quickly using 7zz for extraction.
- Apply a freshly generated set of keys from this server to the stash
- It is better to be chilled with no loss than attempt a zero downtime manoeuvre and get slashed  

---

### **Potential causes for unintentional misconduct**

In this paragraph we assume that misconduct is conducted unintentionally and want to provide some reasons for a slash under unintentional misconduct.

- Using a binary from a thirs-party location that might be altered 
- Compromised server in which an unwanted third party alters code in an undesired manner

### **How to mitigate the risk of being slashed on unintentional misconduct?**

- Always download the source files and binaries from the official SwapDex server
- Verfiy the hash of the downloaded binaries
- Use the "secure validator" server setup on the w3f rep or adhere to its principles
- Basic security advise applies, use a firewall, manage user access, use SSH certificate access etc.
- Avoid using your server as a general purpose machine. Greater interaction = greater risk.


## **Reward Distribution**
---
### **Reward Distribution Example**

## **Inflation**
---
### **Why Stake?**
### **Why not Stake?**

## **How many Validators does Kusari has?**
---