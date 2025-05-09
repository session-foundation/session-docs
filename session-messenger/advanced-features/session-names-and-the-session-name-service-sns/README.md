# Session Names and the Session Name Service (SNS)

{% hint style="info" %}
**Note:** SNS has not yet been deployed in the Session Token ecosystem.
{% endhint %}

The Session Name Service (SNS) is a distributed, open, and extensible naming system used in the Session Ecosystem. SNS will be used to translate Session Account IDs (long, alphanumeric codes) into human-readable labels, called Session Names.

Session Names are one-of-a-kind and easy to share, making it simpler to connect with other people on Session.

### How the Session Name Service (SNS) works

The Session Name Service allows users to register records mapping information, such as an Account ID, to a human-readable label. These mappings are stored and maintained on-chain, operated using a series of smart contracts, making SNS a secure and decentralized naming solution for Session.

### Session Name Records

When a Session Name is registered, a record containing a name, an Account ID (Session public key) and a wallet address is submitted. For example, a record may contain the following information:

| **Name**               | `SessionFan`                                                         |
| ---------------------- | -------------------------------------------------------------------- |
| **Session Account ID** | `053b6b764388cd6c4d38ae0b3e7492a8ecf0076e270c013bb5693d973045f45254` |
| **Wallet address**     | `0x31c319A6621d7e08557391A117B7d0a26C63Fc84`                         |

In the future, these records could also contain other information.

### Session Name Resolvers

Because Session Nodes track on-chain registration of Session Names, they are able to act as Resolvers for the Session Name Service (SNS).

Session clients are able to query a random set of Session Nodes, which then resolve the queried name to a Session Account ID. In this case, multiple nodes are queried (ensuring they return the same ID) to prevent a single dishonest node compromising name resolution.&#x20;

For example, when sending a new message on Session, entering `SessionFan` prompts the client to query Session Nodes, which then retrieve the corresponding **Account ID**. This allows you to add the recipient using their readable name (`SessionFan`), rather than manually entering their Account ID.
