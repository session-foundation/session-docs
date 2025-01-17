# Onion Requests

Session utilises a lightweight onion-routing protocol called Onion Requests for all message routing.&#x20;

This protects identifying user information such as IP addresses and helps prevent metadata analysis which would deanonymise users and/or their contacts.&#x20;

While functional, Onion Requests are a very limited message-based protocol built on top of TCP. Onion Requests are effective for passing messages across the network, but do not lend themselves well to expanding the network capabilities to large transfers or responsive voice/video communication.

In the future, it is intended that the Session client will be able to interface with the network’s more advanced onion-routing overlay network, making key features such as onion-routed voice and video calls possible.
