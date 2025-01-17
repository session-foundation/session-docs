# Session Name Service

The Session Name Service (SNS) allows [Session ](https://getsession.org)users to register human-readable names with their Account ID on Session.

These names which can be mapped to an Account ID and used for simple contact discovery in Session.

### Name resolution

Each SNS record can contain a name, an Account ID (Session public key), and a wallet address.

For example, a record may be made with:&#x20;

{% code overflow="wrap" lineNumbers="true" %}
```
Name: SessionFan
Session Account ID: 053b6b764388cd6c4d38ae0b3e7492a8ecf0076e270c013bb5693d973045f45254 
Wallet address: 0x31c319A6621d7e08557391A117B7d0a26C63Fc84
```
{% endcode %}

By querying the relevant SNS record, users can discover the associated Account ID or wallet address and use this information for in-app actions such as sending a message request.&#x20;

Session uses this registry to route messages addressed to registered names, allowing users to make themselves contactable via their real name, online pseudonym, or any other name they choose.&#x20;

Once the record has been purchased, the Account ID it is mapped to may be changed at any time.&#x20;

### Name rules

There are certain restrictions on which characters are valid for names used in SNS records. Note that names are **not** case sensitive.

All names must:

* Start with an alphanumeric character or underscore
* Consist of alphanumeric, hyphens, or underscores
* End with an alphanumeric character of underscore

You may register names with special characters or emojis by using the equivalent Punycode representation.

The name must contain 1-64 characters.

### Record expiry

By default, SNS records do not have an expiry date. However, ownership of a record may be [transferred ](sns-marketplace.md)to a different wallet or user if desired. \


\
