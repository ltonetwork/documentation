# secp256r1

The most commonly used and well-supported Elliptic Curve is NIST P-256. This is an ECDSA method using the secp256r1 curve.

### Seeding

LTO libraries use BIP39 with the English word list to create a seed phrase for the secp256k1 key type. BIP32 is used to generate the binary seed from the seed phrase.

{% hint style="warning" %}
BIP39 + BIP32 with `secp256r1` is non-standard and not supported by mainstream wallets. It is safe when using hardened paths, but specific to LTO Network.
{% endhint %}

### Signing

* Created private key using the account seed `93d...S1v`.
* Create the corresponding coordinates for the public key and compress the public key.

### Encryption

Encryption is done using [ECIES Hybrid Encryption Scheme](https://cryptobook.nakov.com/asymmetric-key-ciphers/ecies-public-key-encryption), which combines ECDSA with AES. It uses the same keys for encryption and signing.
