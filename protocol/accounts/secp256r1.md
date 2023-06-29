# secp256r1

The most commonly used and well-supported Elliptic Curve is NIST P-256. This is an ECDSA method using the secp256r1 curve.

### Seeding

Seeding is not supported for secp256r1. Instead, the private key must be stored and used to create an account.

### Signing

* Created private key using the account seed `93d...S1v`.
* Create the corresponding coordinates for the public key and compress the public key.

### Encryption

Encryption is done using [ECIES Hybrid Encryption Scheme](https://cryptobook.nakov.com/asymmetric-key-ciphers/ecies-public-key-encryption), which combines ECDSA with AES. It uses the same keys for encryption and signing.
