# Account IDs and self managed keys

Most popular messaging applications require the user to register with an email or mobile phone number in order to use the service. This requirement represents a major privacy and security compromise for users due to the centralized management of phone numbers (i.e. telecommunications service providers), which have the capacity to assume direct control of specific users’ phone numbers. Additionally, methods such as SIM swapping attacks, service provider hacking, and phone number recycling may be exploited by lower-level actors to compromise user security.

Using phone numbers as the basis for account registration also greatly weakens privacy, with many countries requiring users to provide personally identifying information such as a passport, driver’s license or identity card to obtain a phone number—permanently mapping users’ identities to their phone numbers.

To counter this, Session messenger does not use phone numbers or email addresses as the basis for its account system. User identity is established through the generation of an Ed25519 public-private key pair. This keypair is not required to be linked with any other identifier, and new key pairs can be generated on-device in seconds. This means that each key pair (and thus, each account) is pseudonymous, unless intentionally linked with an individual identity by the user through out-of-band activity.

{% hint style="info" %}
Example of an Account ID on Session messenger: _056c3d9682f167135d4c86b0af24e7aca98949380fa825e01455e788fe3df1d05c_
{% endhint %}

Additionally, because messages are encrypted using the recipient address, Session is able to remove the [trust on first use](https://getsession.org/blog/trust-on-first-use-the-achilles-heel-of-centralised-messengers) problem.
