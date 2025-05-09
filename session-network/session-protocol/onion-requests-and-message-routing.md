# Onion requests and message routing

Session implements a simple onion-routing protocol, called Onion Requests, to protect user IP addresses.

{% hint style="info" %}
Onion routing is a method of sending internet traffic (like messages) through multiple encrypted layers, like the layers of an onion. Each layer is ‘peeled’ by a different server in the network, hiding the sender's identity.
{% endhint %}

For the messaging context, this is critical for preventing a node or other party from using metadata to de-anonymize users or deduce conversation participants for a given chat.

Onion requests are a relatively simple system, built using TCP. Although this simplicity has its advantages, it also means that onion requests inherit some of TCP's limitations—such as slower connection speeds compared to other protocols.&#x20;

To route a message, the sender first needs to find the Session Nodes in their recipient’s swarm. To do this, the client fetches the swarm mapping for their recipient’s Account ID. Given the swarm mapping, the sender creates a message and prepares the message to be sent to the Session Node network. The sender includes the necessary information for the message to be processed, including:

* &#x20;The Account ID of the recipient
* &#x20;The message timestamp
* &#x20;The message expiry time
* &#x20;The namespace where the message will be stored

The sender then sends this message—using an onion request—to a random Session Node within the recipient’s swarm. This Session Node will then propagate the message to the remaining nodes in the swarm, with each node storing the message until the specified expiry time.

{% hint style="info" %}
By default, chat messages expire after two weeks. However, this can be configured using the Disappearing Messages feature in Session messenger
{% endhint %}

While functional, Onion Requests are a very limited message-based protocol built on top of TCP. Onion Requests are effective for passing messages across the network, but do not lend themselves well to expanding the network capabilities to large transfers or responsive voice/video communication.

In the future, it is intended that the Session client will be able to interface with the network’s more advanced onion-routing overlay network, making key features such as onion-routed voice and video calls possible.
