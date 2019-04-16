# Cryptography

Live Contracts uses the `SHA256` to create a cryptographic hashes. The `BLAKE2b` and `Keccak256` hashing algorithms are used for creating public/secret key pairs. The `ED25519` scheme is applied to create and verify signatures. `X25519` is used for asymmetric encryption as key exchange. `Salsa20` \(with `Poly1305`\) is used for both asymmetric and symmetric encryption. `Base58` is used to create the string from of bytes.

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

## Asymmetric encryption

Live Contracts uses `Curve25519` \(Montgomery form\) for encryption \(`X25519`\) and signing \(`ED25519`\).

### Creating a private key from a seed

A seed string is a representation of entropy, from which you can re-create deterministically all the private keys for an account. It should be long enough so that the probability of selection was a unrealistic negligible.

For ease of memorization the LegalThings One front-end uses [Brainwallet](https://en.bitcoin.it/wiki/Brainwallet), to ensure that the seed is made up of words, that is easy to write down or remember. The app takes the UTF-8 bytes of the string and uses them to create keys and addresses.

For example, seed string `manage manual recall harvest series desert melt police rose hollow moral pledge kitten position add` after reading as UTF-8 bytes and encoding them to Base58 string are coded as _\(split in 3 lines\)_`xrv7ffrv2A9g5pKSxt7gHGrPYJgRnsEMDyc4G7srbia6P  
hXYLDKVsDxnqsEqhAVbbko7N1tDyaSrWCZBoMyvdwaFNj  
WNPjKdcoZTKbKr2Vw9vu53Uf4dYpyWCyvfPbRskHfgt9q`

A seed string is involved with the creation of private keys. When creating a private key using the LTO Network front-end, the seed is prepended with a 'nonce' field of 4 bytes \(big-endian representation\). This initially has a value of 0 and increases every time you create the new account. We use this array of bytes for calculate hash `sha256(blake2b256(bytes))`. This resulting array of bytes we call `account seed`, from it you can generate a private and public key pair. This is then converted to a public and private key pair for the `ED25519` algorithm.

Generate an `ED25519` key from the seed and than convert it to create the `X25519` key, or generate both keys from the seed.

_NOTE: Not all random 32 bytes can be used as private keys \(but any bytes of any size can be a seed\). The signature scheme for the ED25519 introduces restrictions on the keys, so create the keys only through the methods of the cryptography libraries and be sure to make a test of the ability to sign data with a private key and then check it with a public key, however obvious this test might seem._

**Recommended libraries**

* [libsodium](https://download.libsodium.org/doc/)
* [NaCl](https://nacl.cr.yp.to/) \(or similar like tweetnacl\)

[_Other encryption libraries_](https://en.wikipedia.org/wiki/Comparison_of_cryptography_libraries) _might work as well, but make sure they yield the same result as the recommended libraries. The Waves API library is known to yield another result and shouldn't be used to create an account from seed._

#### Example

Brainwallet seed string

```text
manage manual recall harvest series desert melt police rose hollow moral pledge kitten position add
```

As UTF-8 bytes encoded

```text
xrv7ffrv2A9g5pKSxt7gHGrPYJgRnsEMDyc4G7srbia6PhXYLDKVsDxnqsEqhAVbbko7N1tDyaSrWCZBoMyvdwaFNjWNPjKdcoZTKbKr2Vw9vu53Uf4dYpyWCyvfPbRskHfgt9q
```

Account seed bytes with nonce 0 before apply hash function in Base58

```text
1111xrv7ffrv2A9g5pKSxt7gHGrPYJgRnsEMDyc4G7srbia6PhXYLDKVsDxnqsEqhAVbbko7N1tDyaSrWCZBoMyvdwaFNjWNPjKdcoZTKbKr2Vw9vu53Uf4dYpyWCyvfPbRskHfgt9q
```

blake2b256v \(account seed bytes\)

```text
6sKMMHVLyCQN7Juih2e9tbSmeE5Hu7L8XtBRgowJQvU7
```

Account seed \(`keccak256(blake2b256(account seed bytes))`\)

```text
H4do9ZcPUASvtFJHvESapnxfmQ8tjBXMU7NtUARk9Jrf
```

Account seed after `SHA256` hashing \(optional, if your library does not do it yourself\)

```text
49mgaSSVQw6tDoZrHSr9rFySgHHXwgQbCRwFssboVLWX
```

Created private key \(using encryption library\)

```text
3kMEhU5z3v8bmer1ERFUUhW58Dtuhyo9hE5vrhjqAWYT
```

Created public key \(using encryption library\)

```text
HBqhfdFASRQ5eBBpu2y6c6KKi1az6bMx8v1JxX4iW1Q8
```

### Signing

**ED25519** is used for all the signatures in the project.

The process is as follows: create the message for signing [example for an event](http://schema.livecontract.io/event-chain/#signature), then create a signature from this message using the private key.

Validation of signature requires the signature, the message and the public key.

Functions for ED25519 are defined as `sign` in [libsodium](https://download.libsodium.org/doc/public-key_cryptography/public-key_signatures.html) and [nacl](https://nacl.cr.yp.to/sign.html).

_Do not forget that there are many valid \(not unique!\) signatures for a one message. Also you should should not rely on any information before the hash and/or signature are checked._

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

