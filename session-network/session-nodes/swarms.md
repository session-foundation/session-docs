# Swarms

Session requires a secondary logical data layer built on top of the Session Node Network to ensure reliable message storage and retrieval.&#x20;

Swarms protect users from message loss if Session Nodes go offline, drop messages, or become otherwise unavailable. Messages are replicated across small groupings of Session Nodes (swarms). Swarm composition is determined by the network design, not individual nodes, keeping network swarms robust and operational even as Session Nodes join and leave the network.

The following set of simple rules ensure that Session Nodes within swarms remain synchronized as

the composition of swarms changes:&#x20;

* &#x20;When a node joins a new swarm, existing swarm members recognize the new member and push the swarm’s data records to the new member.
* &#x20;When a node leaves a swarm, its existing records can be safely erased, with the exception of when the node is migrating from a dissolving swarm. In this case, the migrating node determines the swarms responsible for its records and distributes them accordingly.
