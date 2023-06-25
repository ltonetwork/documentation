# Accounts

## Key types

LTO network supports multiple cryptographic algorithms for signing.

| id | reference                            | type  | curve      |
| -- | ------------------------------------ | ----- | ---------- |
| 1  | [ed25519](accounts.md#ed25519)       | EdDSA | curve25519 |
| 2  | [secp256k1](accounts.md#secp-256-k1) | ECDSA | secp256k1  |
| 3  | secp256r1                            | ECDSA | secp256r1  |

{% hint style="info" %}
Use [NaCl](https://nacl.cr.yp.to/) or [sodium](https://libsodium.gitbook.io/) to create an address from the _account seed_.
{% endhint %}

{% hint style="success" %}
EdDSA allows generating the X25519 private key from the ED25519 private key and the X25519 public key from the ED25519 public key. Only the keys for signing are on the public chain, but this allows the keys for encryption to be calculated.
{% endhint %}

By default, accounts use EdDSA with curve25519. ED25519 is used for signatures. X25519 is used for encryption in the project.

## ED25519

### Creating a private key from seed

A seed string is a representation of entropy, from which you can re-create deterministically all the private keys for one wallet. It should be long enough so that the probability of selection is an unrealistic negligible.

In fact, seed should be an array of bytes but for ease of memorization, the LTO wallet uses a [mnemonic seed phrase](https://en.bitcoin.it/wiki/Brainwallet), to ensure that the seed is made up of words and easy to write down or remember. The application takes the UTF-8 bytes of the string and uses them to create keys and addresses.

For example, seed string&#x20;

`manage manual recall harvest series desert melt police rose hollow moral pledge kitten position add`&#x20;

after reading this string as UTF-8 bytes and encoding them to Base58, the string will be coded as&#x20;

`xrv7ffrv2A9g5pKSxt7gHGrPYJgRnsEMDyc4G7srbia6PhXYLDKVsDxnqsEqhAVbbko7N1tDyaSrWCZBoMyvdwaFNjWNPjKdcoZTKbKr2Vw9vu53Uf4dYpyWCyvfPbRskHfgt9q`

A seed string is involved with the creation of private keys. The nonce' field is an integer prepended to the seed bytes. Typically, this value is initially 0 and increases every time you create the new address.&#x20;

We use this array of bytes to calculate the hash `sha256(blake2b256(bytes))`. This resulting array of bytes called the _account seed_. From the account seed, you can deterministically generate a private and public key pair.

#### Example

Brainwallet seed string

```
manage manual recall harvest series desert melt police rose hollow moral pledge kitten position add
```

As Base58 encoded byte array

```
xrv7ffrv2A9g5pKSxt7gHGrPYJgRnsEMDyc4G7srbia6PhXYLDKVsDxnqsEqhAVbbko7N1tDyaSrWCZBoMyvdwaFNjWNPjKdcoZTKbKr2Vw9vu53Uf4dYpyWCyvfPbRskHfgt9q
```

Account seed bytes with nonce 0 before apply hash function (Base58 encoded)

```
1111xrv7ffrv2A9g5pKSxt7gHGrPYJgRnsEMDyc4G7srbia6PhXYLDKVsDxnqsEqhAVbbko7N1tDyaSrWCZBoMyvdwaFNjWNPjKdcoZTKbKr2Vw9vu53Uf4dYpyWCyvfPbRskHfgt9q
```

Account seed `sha256(blake2b256(account seed bytes))`  (Base58 encoded)

```
93dvzDQ8KBe4y7Nw89xsguWe8ZTVYGAA5kjvJ7miQS1v
```

Account seed after `sha256` hashing (optional, if your library does not do it yourself)

```
ETYQWXzC2h8VXahYdeUTXNPXEkan3vi9ikXbn912ijiw
```

#### Alternative methods

Using the method based on the account seed, ensure that the seed phrase is compatible with the LTO wallet. However, it's not required to use this method.

The seed is not needed for signing, only the private key. The key can be generated through other means, for instance using [OpenSSL genkey](https://www.openssl.org/docs/man1.1.1/man1/openssl-genpkey.html).

### Signing

Created private key using the account seed `93d...S1v`.

```
4zsR9xoFpxfnNwLcY4hdRUarwf5xWtLj6FpKGDFBgscPxecPj2qgRNx4kJsFCpe9YDxBRNoeBWTh2SDAdwTySomS
```

Created public key

```
GjSacB6a5DFNEHjDSmn724QsrRStKYzkahPH67wyrhAY
```

### Encryption

Created private key using the account seed `93d...S1v`.

```
4q7HKMbwbLcG58iFV3pz4vkRnPTwbrY9Q5JrwnwLEZCC
```

Created public key

```
6fDod1xcVj4Zezwyy3tdPGHkuDyMq8bDHQouyp5BjXsX
```

## secp256k1

Bitcoin, Ethereum, and many other blockchains use ECDSA with the secp256k1 curve for signing transactions.

### Seeding

LTO libraries use BIP39 with the English word list to create a seed phrase for the secp256k1 key type. BIP32 is used to generate the binary seed from the seed phrase.

### Signing

* Created private key using the account seed `93d...S1v`.
* Create the corresponding coordinates for the public key and compress the public key.

### Encryption

Encryption is done using [ECIES Hybrid Encryption Scheme](https://cryptobook.nakov.com/asymmetric-key-ciphers/ecies-public-key-encryption), which combines ECDSA with AES. It uses the same keys for encryption and signing.

## secp256r1

The most commonly used and well-supported Elliptic Curve is NIST P-256. This is an ECDSA method using the secp256r1 curve.

### Seeding

Seeding is not supported for secp256r1. Instead, the private key must be stored and used to create an account.

### Signing

* Created private key using the account seed `93d...S1v`.
* Create the corresponding coordinates for the public key and compress the public key.

### Encryption

Encryption is done using [ECIES Hybrid Encryption Scheme](https://cryptobook.nakov.com/asymmetric-key-ciphers/ecies-public-key-encryption), which combines ECDSA with AES. It uses the same keys for encryption and signing.

## Creating the address

The public network address is obtained from the **(signature) public key** and network id. The method is the same regardless of the key type.

| # |    Field Name   |  Type | Length |
| - | :-------------: | :---: | ------ |
| 1 |  Version (0x01) |  Byte | 1      |
| 2 |    Network id   |  Byte | 1      |
| 3 | Public key hash | Bytes | 20     |
| 4 |     Checksum    | Bytes | 4      |

{% hint style="info" %}
* _Public key hash_ is the first 20 bytes of the _SecureHash_ of the public key. _SecureHash_ is the hash function `sha256(Blake2b256(public_key))`.
* _Checksum_ is the first 4 bytes of _SecureHash_ of version, scheme, and hash bytes.
{% endhint %}

Because the address contains the **network id**, different networks result in a different address for the same seed / public key.

| Network | Char | Byte |
| ------- | ---- | ---- |
| Testnet | T    | 0x54 |
| Mainnet | L    | 0x4C |

### Example

For public key

```
GjSacB6a5DFNEHjDSmn724QsrRStKYzkahPH67wyrhAY
```

for the mainnet network (network id 'T'), this key results in the following address

```
3JmCa4jLVv7Yn2XkCnBUGsa7WNFVEMxAfWe
```

### Derived identities

The blockchain address of derived identities is calculated from a public key, plus a secret. To calculate the public key hash, hmac is used, instead of a regular sha256 hash.

```
sha256_hmac(Blake2b256(public_key), secret)
```

Derived identity addresses are only used for [decentralized identifiers (DIDs)](identities/decentralized-identifiers.md) and can't be used to sign transactions on the public blockchain.
