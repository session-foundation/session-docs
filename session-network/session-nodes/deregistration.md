# Deregistration

The Session Network uses deregistration rules to enforce standards of node performance in a decentralized way. This ensure all operational Session Nodes are up to date and performing adequately to maintain the network.&#x20;

### Session Node states

Session Nodes have four different states: awaiting, active, decommissioned or deregistered.

| State          | Description                                                                                                                                                                |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Awaiting       | Session Node is awaiting the required [stake](staking-and-collateralization.md) of Session Tokens.                                                                         |
| Active         | Session Node is staked, performing tasks as required, and receiving rewards.                                                                                               |
| Decommissioned | Session Node is not performing tasks as required or is not meeting uptime standards, and has been placed into an inactive state. Decommissioned nodes do not earn rewards. |
| Deregistered   | Session Node has been inactive (decommissioned) for too long, and has been deregistered from the network.                                                                  |

### **Decommissioned nodes and credits**

Session Nodes have "credits" which determine how long a node may remain inactive (decommissioned) before being deregistered from the network. These credits are used during any time period where the Session Node is offline (or otherwise stops meeting network requirements).&#x20;

A new node starts out with `INITIAL_CREDIT`, and then builds up `CREDIT_PER_DAY` for each day the it remains active, up to a maximum of `DECOMMISSION_MAX_CREDIT`.

|                           |                                                |
| ------------------------- | ---------------------------------------------- |
| `INITIAL_CREDIT`          | 60 blocks (\~2 hours) of decommission time.    |
| `CREDIT_PER_DAY`          | 24 blocks (\~0.8 hours) of decommission time.  |
| `DECOMMISSION_MAX_CREDIT` | 1440 blocks (\~48 hours) of decommission time. |
| `MINIMUM`                 | 60 blocks(\~2 hours) of decommission time.     |

**Example**:

If an Session Node stops sending uptime proofs, a quorum of service nodes will analyze whether the node has built up enough credits (at least `MINIMUM`). If so, instead of submitting a deregistration, the quorum instead submits a decommission, removing the node from the list of active Session Nodes. While decommissioned, the node does not receive rewards or participate in active network duties.

If the Session Node node comes back online (i.e. starts sending the required performance proofs again) before its credits run out, a quorum will reinstate the node using a recommission transaction. This transaction adds the Session Node back to the reward list and resets the node's accumulated credits to 0.&#x20;

If the Session Node does not come back online within the required number of blocks (i.e. if the node does not come back online and begin performing as required before its credits are depleted), then a quorum will send a permanent deregistration transaction to the network, locking that node's stake for 30 days.

### Quorums tests <a href="#testing-quorums" id="testing-quorums"></a>

|                                              |                                                                                                                                                                                                                                                                                                                    |
| -------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <p><strong>Uptime Proofs</strong></p><p></p> | Checks if a node has provided uptime proofs within the last 2 hours. If the node has not provided uptime proofs, but it has at least `MINIMUM` credits, it will be decommissioned. If it does not have at least `MINIMUM` credits, it will be directly deregistered.                                               |
| **IP Changes**                               | Checks if a Session Node has broadcast more than one IP to the network in the last 24 hours. If a service node's IP has changed, it will be forced to the bottom of the reward list.                                                                                                                               |
| **Checkpointing**                            | Checks if each Session Node within the quorum has provided a hash of a block for a specific block height. If a node within the quorum does not provide a hash, but it has at least `MINIMUM` credits, it will be decommissioned. If it does not have at least `MINIMUM` credits, it will be directly deregistered. |

### **State change transactions**

State change transaction change the state of a Session Node. Typically, state change transactions are created when a quorum comes to a consensus about a given node's activities (or lack thereof).

The only state change transaction that can be created by any indivdual node is the `Register` transaction, which changes the state of a service node from `awaiting` to `active`.

| State\_Change\_Transaction | Description                                                                                                                                                                                                    |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Register TX`              | <p>A transaction created from the staking portal registering the Session Node.</p><p>State Change: <code>Awaiting -> Active</code></p><p><em>Note: This is <strong>not</strong> a quorum transaction.</em></p> |
| `Decommission TX`          | <p>Node is made inactive; it remains in the service node list, but is removed from the rewards list and from any network duties. </p><p>State Change: <code>Active -> Decommissioned</code></p>                |
| `Recommission TX`          | <p>Node is added back to the Session Node list and placed at the bottom of the rewards list. </p><p>Stage Change: <code>Decommissioned -> Active</code></p>                                                    |
| `IP Change TX`             | <p>Node is pushed to the bottom of the rewards list as punishment for changing its IP. </p><p>Stage Change: N/A (no change)</p>                                                                                |
| `Deregister TX`            | <p>Node is deregistered from the network.</p><p>State Change: <code>Active -> Deregistered</code> / <code>Decommissioned -> Deregistered</code></p>                                                            |
