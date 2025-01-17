# Consensus

Session Node state is primarily tracked by the Session appchain, a native L1 which includes rewards calculation logic and voting logic required to deregister or punish nodes on the network. New nodes are permissioned into the Session Node network through staking into the EVM native staking contract.

The consensus of Session Nodes can be snapshotted and signed efficiently using BLS signatures and posted back to the native EVM native chain for validation. This is used for node deregistration and is required for any Session Node to claim funds or withdraw their Session Node from the network.

{% hint style="info" %}
Appchains are specialised, app-specific blockchains built to increase scalability and throughput. The Session appchain logic is based on the original Oxen blockchain.&#x20;
{% endhint %}

In this way, the most expensive and frequent operations can occur in an appchain environment (where fees are lower). The appchain state is then snapshotted and returned to the EVM chain for validation and security purposes.
