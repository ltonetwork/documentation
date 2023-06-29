# Cryptography

## Encoding

### Base58

Arrays of bytes (like strings and hashes) in the project are encoded by [Base58 algorithm with Bitcoin alphabet](https://en.bitcoin.it/wiki/Base58Check\_encoding).

When encoding a hash, use the raw bytes and not the hexadecimal notation.

_Note that encoding is not encryption. You can decode a base58 encoded message without the need for any type of key._

**Examples**

* The string `hello world` is encoded as `StV1DL6CwTryKyV`.
*   SHA256 hash `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855` is encoded as

    `GKot5hBsd81kMupNCXHaqbhv3huEbxAFMLnpcX2hniwn`&#x20;

### Base64

When casting objects to JSON, data is sometimes encoded as base64 instead of base58. Base64 encoded strings are always prefixed with `base64:`.

## Hashing

### SHA256

In general Live Contracts uses the 256-bit version of SHA-2 for encoding. SHA-2 is one of the most common hashing algorithms which means it's available for almost all platforms and programming languages.

### BLAKE2b + SHA256

When secure hashes are required, a combination of BLAKE2b and SHA256 is used.

{% hint style="success" %}
BLAKE2b is supported by NaCl-compatible libraries like libsodium as ['Generic hashing'](https://download.libsodium.org/doc/hashing/generic\_hashing.html). It's widely supported across platforms and languages.
{% endhint %}

## Signing

### **ED25519**

The default method for signing is using ED25519. This is an EdDSA algorithm on curve25519. It is designed to be faster than existing digital signature schemes without sacrificing security.

ED25519 is well-supported across many platforms and languages.

{% hint style="success" %}
Functions for ED25519 are defined as _sign_ in [libsodium](https://download.libsodium.org/doc/public-key\_cryptography/public-key\_signatures.html) and [nacl](https://nacl.cr.yp.to/sign.html).
{% endhint %}

{% hint style="warning" %}
There are many valid (not unique!) signatures for one message when using ED25519. Also, you should not rely on any information before the hash and/or signature are checked.
{% endhint %}

### secp256k1

Bitcoin, Ethereum, and many other blockchains use ECDSA with the secp256k1 curve for signing transactions. Outside of the realm of blockchain, this curve is not commonly used.

### secp256r1

The most commonly used and well-supported Elliptic Curve is NIST P-256. This is an ECDSA method using the secp256p1 curve.

## Encryption

### X25519

There are 3 algorithms involved in asymmetric encryption for ED25519 accounts:

* Key exchange: X25519
* Encryption: XSalsa20 stream cipher
* Authentication: Poly1305 MAC

This is a public key encryption schema, where the public key is used to encrypt data and the private key is used to decrypt data.

_X25519 is sometimes referred to as Curve25519._

{% hint style="success" %}
Functions that use the combination of these algorithms are defined as sealed _box_ in [libsodium](https://download.libsodium.org/doc/public-key\_cryptography/authenticated\_encryption.html). When using [nacl](http://nacl.cr.yp.to/box.html), you can recreate this functionality using the _box_ functionality and a random ephemeral public-private key pair.
{% endhint %}

### ECIES

Encryption for ECDSA (secp256k1 and secp256r1) accounts is done using [ECIES Hybrid Encryption Scheme](https://cryptobook.nakov.com/asymmetric-key-ciphers/ecies-public-key-encryption), which combines ECDSA with AES.
