# Staking and collateralisation

Session Nodes lock Session Tokens using a staking smart contract in order to register on the network. The staking transaction contains both the Session Tokens and the information required to stake a node, including the key belonging to the node and the operator fee.

The amount of Session Tokens required to be locked for a node to be fully staked will be announced closer to TGE. Operators must stake a minimum of 25% of the total stake requirement. If an operator does not contribute the full stake themselves, other contributors must stake the remaining amount. In this instance, the operator sets an operator fee to subsidise the time and cost required to successfully run a node. This operator fee is expressed as a percentage of node’s reward which will be awarded to the operator. The remaining reward is then awarded among the contributors proportional to their stakes.

### Contributors and small staking

Up to 10 contributors (including the operator) can combine stakes to meet the requirements for a full Session Node. While the operator must contribute a minimum of 25% of the staking requirement, minimum contributions are calculated dynamically based on the amount of open staking slots for a particular node.&#x20;

To calculate the minimum staking amount for a given contributor:

$$
\frac{Total\:staking\:requirement -Current\:stake}{Open\:staking\:slots}
$$

If a contributor stakes less than 25% of the full stake, they are classified as a small staker. To prevent mass unlocks, small stakers are time-locked for 30 days after their initial staking transaction.&#x20;

### Descaling staking requirement

In the case where too large a portion of the total token supply is locked through staking to Session Nodes and the Staking Reward Pool, the full stake requirement _may_ be reduced to:&#x20;

* Ensure the number of nodes in the network is still able to increase
* Enough Session Tokens remain in circulation for utility purposes (i.e. Session Pro purchases)

No reduction is expected in the near-future.
