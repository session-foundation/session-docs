# Staking and collateralization

Session Nodes lock Session Tokens using a staking smart contract in order to register on the network. The staking transaction contains both the Session Tokens and the information required to stake a node, including the key belonging to the node and the operator fee.

For a node to be fully staked, a fixed total amount of Session Tokens must be deposited to the staking contract. Operators must stake a minimum of 25% of the total stake requirement. If an operator does not contribute the full stake themselves, other contributors must stake the remaining amount (creating a multi-contributor node). In multi-contributor nodes, the operator can set an operator fee to subsidize the time and cost required to successfully run the node. This operator fee is expressed as a % of the node’s reward which will be awarded to the operator. The remaining reward is then awarded among the contributors proportional to their stakes, as per usual.

### Contributors and small staking

Up to 10 contributors (including the operator) can combine stakes to meet the requirements for a full Session Node. While the operator must contribute a minimum of 25% of the staking requirement, minimum contributions are calculated dynamically based on the amount of open staking slots for a particular node.&#x20;

To calculate the minimum staking amount for a given contributor:

$$
\frac{Total\:staking\:requirement -Current\:stake}{Open\:staking\:slots}
$$

If a contributor stakes less than 25% of the requirement for a full node, they are classified as a small staker. To prevent small stakers holding disproportionate power over the node, small stakers are time-locked for 30 days after their initial staking deposit transaction.

### **Staking Requirements**

The current staking requirement for a full node is 25,000 SESH.&#x20;

This means that the minimum staking requirement for a Session Node operator is 6,250 SESH.&#x20;

In the future, if a  case  arises wherein large portions of the total token supply are locked via Session Nodes and the Staking Reward Pool, it is possible (with the consensus of the Session Network) to reduce the staking requirement (via network consensus) to ensure the network is able to continue growing.
