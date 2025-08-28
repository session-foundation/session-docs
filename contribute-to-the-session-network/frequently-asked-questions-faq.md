---
description: >-
  Find answers to common questions about running and staking to testnet nodes
  here.
---

# Frequently Asked Questions (FAQ)

## **What is the minimum and maximum I can stake to a Session node as an operator?**

For single contributor nodes, you must provide the full stake of 25,000 SESH yourself. For multicontributor nodes, as the operator, you must provide a minimum stake of 6250 Session Tokens, and the remaining stake can be contributed by others. This allows for flexibility in multicontributor nodes, enabling collaboration while still requiring a minimum commitment from the operator.

## If I am running a multicontributor node, do I also have to stake SESH?

Yes, if you run a multicontributor node, you are required to stake at least 6250 SESH as the node operator. This ensures you have a vested interest in the node's performance and helps secure the network by maintaining a minimum stake commitment.

## What is the multicontributor operator fee?

Operators can set an operator fee on their multicontributor nodes, which is collected in SESH. This fee helps operators to cover the costs of running a node. The fee is taken as a percentage of the node rewards allocated to the operator. Once the fee is deducted, the remaining rewards are distributed among all contributors (including the operator), proportional to stake amount.\
\
For example: If a node earns 1000 Session Tokens and the operator sets a 10% fee, the operator keeps 100 tokens as their fee, and the remaining 900 tokens are split based on stake amounts—so, if the operator staked 50%, they’d get 450 tokens (550 in total), and two contributors with 25% stakes each would get 225 tokens each.

## How do staking rewards and operator fees get sent to my wallet address?

SESH earned through staking rewards or operator fees on multicontributor nodes do not get automatically sent to your wallet. Instead, to save on gas fees, they are held in a smart contract until you manually claim them. You can claim rewards anytime on your [My Stakes](https://stake.getsession.org/mystakes) page. Simply hit the Claim button. Your SESH will then be sent to the rewards addressed you specified. This is either the wallet address you registered with, or a different address that you specified when configuring your node registration or stake to a multicontributor node.

## Can I specify a separate rewards address from the wallet I am registering my node with or staking from?

Yes, you can specify a separate address to receive your rewards. The rewards address does not need to be the same as the wallet you are using to register your node or stake from. This allows you to direct rewards to a different wallet for added flexibility. Note that rewards are not automatically sent to your wallet. Instead, you can manually claim them anytime via your [My Stakes](https://stake.getsession.org/mystakes) page.

## How do I reserve a stake for specific contributors to my multicontributor node?

To reserve a stake for specific contributors to your node, collect the wallet addresses of the contributors you wish to reserve stakes for. When registering your multicontributor node using the **Guided** setup mode, you will be prompted to reserve stakes. You can then allocate a reserved stake amount for each of your contributors. If using the **Express** setup mode, you can also edit reserved stakes when finalising your node before registering and staking. Reserving stakes is particularly useful if you are collaborating with trusted contributors or if certain stakeholders need to secure their participation in your node.

## How many contributors can I reserve stakes for on my multicontributor node?

Each node can have 10 contributors staking to it, including the operator. This means you can reserve between 1 and 9 stakes for other contributors on your multicontributor node. If the full stake amount is not fully reserved, public contributors can contribute stakes to reach the full stake amount, and/or contributors with reserved stakes can increase the stake that they provide to reach the full stake amount.

## Can contributors stake more than their reserved amount on a multicontributor node?

Yes, contributors can stake more than their reserved amount on a multicontributor node in the case where a node still has remaining stake available that has not been reserved. The reserved stake amount serves as a minimum threshold, ensuring contributors have a guaranteed portion they can contribute. If the full stake amount is not fully reserved, public contributors can contribute stakes to reach the full stake amount, and/or contributors with reserved stakes can increase the stake that they provide to reach the full stake amount.

## How do I activate my node once it is fully staked?

If you have enabled automatic activation during registration, your node will automatically join the network once it is fully staked. If you have disabled automatic activation, you can manually activate your node to join the network when you are ready. The option to activate your node will appear on the [My Stakes](https://stake.getsession.org/mystakes) page once your node is fully staked. This can be useful if you want to ensure all configurations are correct before activating, or prevent the node from activating at a time that is inconvenient for you.

## What happens when a node gets liquidated? <a href="#liquidation-penalty" id="liquidation-penalty"></a>

If a node is not exited within 7 days after becoming eligible (22 days after the initial exit request), the node becomes eligible for liquidation by other users. When a node gets liquidated, a 0.2% penalty is taken from the operator's stake: 0.03% of the operator’s stake is transferred to the liquidator, and 0.17% of the operator’s stake is returned to the Staking Reward Pool.

## What is the network-wide claims limit? <a href="#network-claims-limit" id="network-claims-limit"></a>

To keep the Session Network secure, there is a network-wide limit on how much SESH can be claimed as rewards from the Staking Portal within a 12 hour period. This provides additional security against potential exploits. Up to 1,000,000 SESH can be claimed with a 12 hour period. The 12 hour periods span 00:00 UTC to 12:00 UTC and 12:00 UTC to 00:00 UTC.

When you load the Staking Portal, it will fetch the total amount of SESH already claimed in the current claim cycle. If your claimable amount is greater than the tokens remaining in the claim limit, you'll see a "claim limit reached" screen with a countdown timer showing when the limit resets.

For example, if there are 1,000 SESH remaining in the current limit and you have 999 SESH claimable, you will be able to claim and the portal will show you the claim option. However, if you have 1,001 SESH claimable, you'll see the limit reached screen and will need to wait for the next 12-hour cycle to begin.

## Can I unlock my stake from a multicontributor node before it starts operating? <a href="#unlock-stake-before-registration" id="unlock-stake-before-registration"></a>

Yes, under certain conditions. It is not possible to unlock your stake from a multicontributor node for 24 hours after you contribute your stake. If the node has not been registered and begun to operate before this 24 hour period, you can unlock your stake. If the node has been registered and is currently operating, you can unlock your stake according to certain conditions, depending on how much you are staking (see below).&#x20;

## Can I unlock my stake from a multicontributor node while it is operating? <a href="#unlock-stake-while-operating" id="unlock-stake-while-operating"></a>

Yes, under certain conditions, depending on how much you are staking to the multicontributor node. \
\
Large contributors, who are staking more than ¼ of the full stake amount, can initiate an unlock at any time during the node's operation. The unlock period is 15 days, after which the stake can be reclaimed.  \
\
Small contributors, who are staking less than ¼ of the full stake amount, cannot initiate an unlock during the first 30 days of a node's operation, but can do so at any time after this initial 30-day period. As above, the unlock period is 15 days, after which the stake can be reclaimed.&#x20;
