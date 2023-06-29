# secp256k1

Bitcoin, Ethereum, and many other blockchains use ECDSA with the secp256k1 curve for signing transactions.

## Creating a private key from seed

LTO libraries use BIP39 with the English word list to create a seed phrase for the secp256k1 key type. BIP32 is used to generate the binary seed from the seed phrase.

## Signing

* Created private key using the account seed `93d...S1v`.
* Create the corresponding coordinates for the public key and compress the public key.

## Encryption

Encryption is done using [ECIES Hybrid Encryption Scheme](https://cryptobook.nakov.com/asymmetric-key-ciphers/ecies-public-key-encryption), which combines ECDSA with AES. It uses the same keys for encryption and signing.
