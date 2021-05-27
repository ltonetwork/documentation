# Accounts

## Creating a private key from seed

A seed string is a representation of entropy, from which you can re-create deterministically all the private keys for one wallet. It should be long enough so that the probability of selection is an unrealistic negligible.

In fact, seed should be an array of bytes but for ease of memorization, the LTO wallet uses a [mnemonic seed phrase](https://en.bitcoin.it/wiki/Brainwallet), to ensure that the seed is made up of words and easy to write down or remember. The application takes the UTF-8 bytes of the string and uses them to create keys and addresses.

For example, seed string 

`manage manual recall harvest series desert melt police rose hollow moral pledge kitten position add` 

after reading this string as UTF-8 bytes and encoding them to Base58, the string will be coded as 

`xrv7ffrv2A9g5pKSxt7gHGrPYJgRnsEMDyc4G7srbia6PhXYLDKVsDxnqsEqhAVbbko7N1tDyaSrWCZBoMyvdwaFNjWNPjKdcoZTKbKr2Vw9vu53Uf4dYpyWCyvfPbRskHfgt9q`

A seed string is involved with the creation of private keys. The nonce' field is an integer prepended to the seed bytes. Typically, this value is initially 0 and increases every time you create the new address. 

We use this array of bytes to calculate the hash `sha256(blake2b256(bytes))`. This resulting array of bytes called the _account seed_. From the account seed, you can deterministically generate a private and public key pair.

### Example

Brainwallet seed string

```text
manage manual recall harvest series desert melt police rose hollow moral pledge kitten position add
```

As Base58 encoded byte array

```text
xrv7ffrv2A9g5pKSxt7gHGrPYJgRnsEMDyc4G7srbia6PhXYLDKVsDxnqsEqhAVbbko7N1tDyaSrWCZBoMyvdwaFNjWNPjKdcoZTKbKr2Vw9vu53Uf4dYpyWCyvfPbRskHfgt9q
```

Account seed bytes with nonce 0 before apply hash function \(Base58 encoded\)

```text
1111xrv7ffrv2A9g5pKSxt7gHGrPYJgRnsEMDyc4G7srbia6PhXYLDKVsDxnqsEqhAVbbko7N1tDyaSrWCZBoMyvdwaFNjWNPjKdcoZTKbKr2Vw9vu53Uf4dYpyWCyvfPbRskHfgt9q
```

Account seed `sha256(blake2b256(account seed bytes))`  \(Base58 encoded\)

```text
93dvzDQ8KBe4y7Nw89xsguWe8ZTVYGAA5kjvJ7miQS1v
```

Account seed after `sha256` hashing \(optional, if your library does not do it yourself\)

```text
ETYQWXzC2h8VXahYdeUTXNPXEkan3vi9ikXbn912ijiw
```

### Alternative methods

Using the method based on the account seed, ensure that the seed phrase is compatible with the LTO wallet. However, it's not required to use this method.

The seed is not needed for signing, only the private key. The key can be generated through other means, for instance using [OpenSSL genkey](https://www.openssl.org/docs/man1.1.1/man1/openssl-genpkey.html).

## Key types

LTO network supports multiple cryptographic algorithms for signing.

| id | reference | type | curve |
| :--- | :--- | :--- | :--- |
| 1 | [ed25519](accounts.md#ed25519) | EdDSA | curve25519 |
| 2 | [secp256k1](accounts.md#secp-256-k1) | ECDSA | secp256k1 |
| 3 | [nist256p1](accounts.md#nist-p-256) | ECDSA | secp256p1 |
| 4 | RSA | RSA |  |

## ED25519

By default, accounts use ECDH with curve25519. ED25519 is used for signatures. X25519 is used for encryption in the project.

{% hint style="success" %}
ECDH allows generating the X25519 private key from the ED25519 private key and the X25519 public key from the ED25519 public key. Only the keys for signing are on the public chain, but this allows the keys for encryption to be calculated.
{% endhint %}

{% hint style="info" %}
Use [NaCl](https://nacl.cr.yp.to/) or [sodium](https://libsodium.gitbook.io/) to create an address from the _account seed_.
{% endhint %}

### Signing

Created private key

```text
4zsR9xoFpxfnNwLcY4hdRUarwf5xWtLj6FpKGDFBgscPxecPj2qgRNx4kJsFCpe9YDxBRNoeBWTh2SDAdwTySomS
```

Created public key

```text
GjSacB6a5DFNEHjDSmn724QsrRStKYzkahPH67wyrhAY
```

### Encryption

Created private key

```text
4q7HKMbwbLcG58iFV3pz4vkRnPTwbrY9Q5JrwnwLEZCC
```

Created public key

```text
6fDod1xcVj4Zezwyy3tdPGHkuDyMq8bDHQouyp5BjXsX
```

## secp256k1

Bitcoin, Ethereum, and many other blockchains use ECDSA with the secp256k1 curve for signing transactions. Outside of the realm of blockchain, this curve is barely used and not well supported.

### Signing

Created private key

```text
TODO
```

Created public key

```text
TODO
```

### Encryption

_Encryption is currently not supported for accounts with secp256k1 keys._

## NIST P-256

The most commonly used and well-supported Elliptic Curve is NIST P-256. This is an ECDSA method using the secp256p1 curve.

### Signing

Created private key

```text
TODO
```

Created public key

```text
TODO
```

### Encryption

_Encryption is currently not supported for accounts with P-256 keys._

## Creating the address

Our network address obtained from the **\(signature\) public key.** It uses on the chain id \('T' for testnet and 'L' for mainnet\), so different networks result in a different address for the same seed / public key.

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Version \(0x01\) | Byte | 1 |
| 2 | Address scheme \(0x54 for Testnet 0x57 for Mainnet\) | Byte | 1 |
| 3 | Public key hash | Bytes | 20 |
| 4 | Checksum | Bytes | 4 |

{% hint style="info" %}
* Public key hash the first 20 bytes of the _SecureHash_ of the public key. _SecureHash_ is the hash function `sha256(Blake2b256(public_key))`.
* Checksum is the first 4 bytes of _SecureHash_ of version, scheme, and hash bytes.
{% endhint %}

### Example

For public key

```text
GjSacB6a5DFNEHjDSmn724QsrRStKYzkahPH67wyrhAY
```

for the mainnet network \(chainId 'T'\) this key results in the following address

```text
3JmCa4jLVv7Yn2XkCnBUGsa7WNFVEMxAfWe
```

### Derived identities

The blockchain address of derived identities is calculated from a public key, plus a secret. To calculate the `public key hash`, hmac is used, instead of a normal sha256 hash.

```text
sha256_hmac(Blake2b256(public_key), secret)
```

