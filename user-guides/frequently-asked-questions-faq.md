---
description: >-
  Find answers to common questions about running and staking to testnet nodes
  here.
---

# ❓ Frequently Asked Questions (FAQ)

## **What is the minimum and maximum I can stake to a Session node as an operator?**

For single contributor nodes, you must provide the full stake of 20,000 Session Tokens yourself. For multicontributor nodes, as the operator, you must provide a minimum stake of 5,000 Session Tokens, and the remaining stake can be contributed by others. This allows for flexibility in multicontributor nodes, enabling collaboration while still requiring a minimum commitment from the operator.

## If I am running a multicontributor node, do I also have to stake Session Tokens?

Yes, if you run a multicontributor node, you are required to stake at least 5000 Session Tokens as the node operator. This ensures you have a vested interest in the node's performance and helps secure the network by maintaining a minimum stake commitment.

## What is the multicontributor operator fee?

Operators can set an operator fee on their multicontributor nodes, which is collected in Session Tokens. This fee helps operators to cover the costs of running a node. The fee is taken as a percentage of the node rewards allocated to the operator. Once the fee is deducted, the remaining rewards are distributed among all contributors (including the operator), proportional to stake amount.\
\
For example: If a node earns 1000 Session Tokens and the operator sets a 10% fee, the operator keeps 100 tokens as their fee, and the remaining 900 tokens are split based on stake amounts—so, if the operator staked 50%, they’d get 450 tokens (550 in total), and two contributors with 25% stakes each would get 225 tokens each.

## How do staking rewards and operator fees get sent to my wallet address?

Session Tokens earned through staking rewards or operator fees on multicontributor nodes do not get automatically sent to your wallet. Instead, to save on gas fees, they are held in a smart contract until you manually claim them. You can claim rewards anytime on your [My Stakes](https://stake.getsession.org/mystakes) page. Simply hit the Claim button. Your Session Tokens will then be sent to the rewards addressed you specified. This is either the wallet address you registered with, or a different address that you specified when configuring your node registration or stake to a multicontributor node.

## Can I specify a separate rewards address from the wallet I am registering my node with or staking from?

Yes, you can specify a separate address to receive your rewards. The rewards address does not need to be the same as the wallet you are using to register your node or stake from. This allows you to direct rewards to a different wallet for added flexibility. Note that rewards are not automatically sent to your wallet. Instead, you can manually claim them anytime via your [My Stakes](https://stake.getsession.org/mystakes) page.

## How do I reserve a stake for specific contributors to my multicontributor node?

To reserve a stake for specific contributors to your node, collect the wallet addresses of the contributors you wish to reserve stakes for. When registering your multicontributor node using the Guided setup mode, you will be prompted to reserve stakes. You can then allocate a reserved stake amount for each of your contributors. If using the Express setup mode, you can also edit reserved stakes when finalising your node before registering and staking. Reserving stakes is particularly useful if you are collaborating with trusted contributors or if certain stakeholders need to secure their participation in your node.

## How many contributors can I reserve stakes for on my multicontributor node?

Each node can have 10 contributors staking to it, including the operator. This means you can reserve between 1 and 9 stakes for other contributors on your multicontributor node. If the full stake amount is not fully reserved, public contributors can contribute stakes to reach the full stake amount, and/or contributors with reserved stakes can increase the stake that they provide to reach the full stake amount.

## Can contributors stake more than their reserved amount on a multicontributor node?

Yes, contributors can stake more than their reserved amount on a multicontributor node in the case where a node still has remaining stake available that has not been reserved. The reserved stake amount serves as a minimum threshold, ensuring contributors have a guaranteed portion they can contribute. If the full stake amount is not fully reserved, public contributors can contribute stakes to reach the full stake amount, and/or contributors with reserved stakes can increase the stake that they provide to reach the full stake amount.

## How do I activate my node once it is fully staked?

If you have enabled automatic activation during registration, your node will automatically join the network once it is fully staked. If you have disabled automatic activation, you can manually activate your node to join the network when you are ready. The option to activate your node will appear on the [My Stakes](https://stake.getsession.org/mystakes) page once your node is fully staked. This can be useful if you want to ensure all configurations are correct before activating, or prevent the node from activating at a time that is inconvenient for you.

\


