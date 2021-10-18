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

## **Validator and Nominators**
---

## **Slashing**
---

### **Unresponsiveness**
### **GRANDPA Equivocation**
### **BABE Equivocation**
### **Chilling**
### **Slashing Across Eras**

## **Reward Distribution**
---
### **Reward Distribution Example**

## **Inflation**
---
### **Why Stake?**
### **Why not Stake?**

## **How many Validators does Kusari has?**
---