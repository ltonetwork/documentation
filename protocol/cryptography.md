# Cryptography

Live Contracts uses the `SHA256` to create a cryptographic hashes. The `BLAKE2b` and `SHA256` hashing algorithms are used for creating public/secret key pairs. The `ED25519` scheme is applied to create and verify signatures. `X25519` is used for asymmetric encryption as key exchange. `Salsa20` \(with `Poly1305`\) is used for both asymmetric and symmetric encryption. `Base58` is used to create the string from of bytes.

If you want to create an application, you should find the implementation of these algorithms on your programming language.

## Encoding

### Base58

Arrays of bytes \(like strings and hashes\) in the project are encoded by [Base58 algorithm with Bitcoin alphabet](https://en.bitcoin.it/wiki/Base58Check_encoding).

When encoding a hash, use the raw bytes and not the hexidecimal notation.

_Note that encoding is not encryption. You can decode a base58 encoded message without the need of any type of key._

**Examples**

* The string `hello world` is encoded as `StV1DL6CwTryKyV`.
* SHA256 hash `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855` is encoded as

  `GKot5hBsd81kMupNCXHaqbhv3huEbxAFMLnpcX2hniwn` 

## Hashing

### SHA256

In general Live Contracts uses the 256 bit version of SHA-2 for encoding. SHA-2 is one of the most common hashing algorithms which means it's available for almost all platforms and programming languages.

### BLAKE2b + SHA256

When secure hashes are required, a combination of BLAKE2b and SHA256 are used.

BLAKE2b is supported by NaCL compatible libraries like libsodium as ['Generic hashing'](https://download.libsodium.org/doc/hashing/generic_hashing.html). It's widely supported across platforms and languages.

## Accounts

Accounts use `Curve25519` \(Montgomery form\) for encryption \(`X25519`\) and signing \(`ED25519`\).

### Signing

**ED25519** is used for all the signatures in the project.

The process is as follows: create the message for signing [example for an event](http://schema.livecontract.io/event-chain/#signature), then create a signature from this message using the private key.

Validation of signature requires the signature, the message and the public key.

Functions for ED25519 are defined as `sign` in [libsodium](https://download.libsodium.org/doc/public-key_cryptography/public-key_signatures.html) and [nacl](https://nacl.cr.yp.to/sign.html).

_Do not forget that there are many valid \(not unique!\) signatures for one message. Also, you should not rely on any information before the hash and/or signature are checked._

### Encryption

There are 3 algorithms involved for asymmetric encryption:

* Key exchange: X25519
* Encryption: XSalsa20 stream cipher
* Authentication: Poly1305 MAC

This is a public key encryption schema, where the public key is used to encrypt data and the private key is used to decrypt data.

Functions that use the combination of these algorithms are defined as `box` in [libsodium](https://download.libsodium.org/doc/public-key_cryptography/authenticated_encryption.html) and [nacl](http://nacl.cr.yp.to/box.html). _X25519 is sometimes referred to as Curve25519._

## Symmetric encryption

### Encryption

There are 2 algorithms involved for symmetric encryption:

* Encryption: XSalsa20 stream cipher
* Authentication: Poly1305 MAC

Symmetric encryption SHOULD be applied when linking to external content in a template, document or comment.

You SHOULD always generate a new random key when using symmetric encryption. While the key SHOULD NOT be shared with third parties, anybody that has a copy of the key can still do so.

Typically both access to the chain, as the encryption key are required to gain access to sensitive data.

Functions that use a combination of these algorithms is are defined as `secretbox` in [libsodium](https://download.libsodium.org/doc/secret-key_cryptography/authenticated_encryption.html) and [nacl](http://nacl.cr.yp.to/secretbox.html).

