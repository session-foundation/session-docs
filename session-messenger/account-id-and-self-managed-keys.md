# Account ID and Self Managed Keys

As opposed to standard sign-up processes used by messaging apps such as WhatsApp or Signal, Session does not require a phone number or any other identifying information to create an account. When creating an account, clients simply generate a keypair on-device while users nominate basic account information such as a non-unique nickname. The user’s private key is used for encryption and account recovery, while the public key (‘Account ID’) is used for addressing.
