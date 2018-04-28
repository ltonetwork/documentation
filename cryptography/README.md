# Cryptography

Live Contracts uses the `SHA256` to create a cryptographic hashes. The `Blake2b256` and `Keccak256` hashing algorithms
are used for creating public/secret key pairs. The `ED25519` scheme is applied to create and verify signatures. `X25519`
is used to for asymmetric encryption. For symmetric encrypt use `AES256` in Galois/Counter Mode (gcm). `Base58` is used
to create the string from of bytes.

If you want to create an application, you should find the implementation of these algorithms on your programming
language.

### Base58

In order to ease human readable, all arrays of bytes in the project are encoded by
[Base58 algorithm with Bitcoin alphabet].

**Example**

The string `teststring` are coded into the bytes `[5, 83, 9, -20, 82, -65, 120, -11]`. The bytes `[1, 2, 3, 4, 5]` are
coded into the string `7bWpTW`.

[Base58 algorithm with Bitcoin alphabet]: https://en.bitcoin.it/wiki/Base58Check_encoding

### Asymmetric encryption

Live Contracts uses `Curve25519` (Montgomery form) for encryption (`X25519`) and signing (`ED25519`).

#### Creating a private key from a seed

A seed string is a representation of entropy, from which you can re-create deterministically all the private keys for an
account. It should be long enough so that the probability of selection was a unrealistic negligible.

For ease of memorization the LegalThings One front-end uses [Brainwallet](https://en.bitcoin.it/wiki/Brainwallet), to
ensure that the seed is made up of words, that is easy to write down or remember. The app takes the UTF-8 bytes of the
string and uses them to create keys and addresses.

For example, seed string `manage manual recall harvest series desert melt police rose hollow moral pledge kitten
position add` after reading as UTF-8 bytes and encoding them to Base58 string are coded as
`xrv7ffrv2A9g5pKSxt7gHGrPYJgRnsEMDyc4G7srbia6PhXYLDKVsDxnqsEqhAVbbko7N1tDyaSrWCZBoMyvdwaFNjWNPjKdcoZTKbKr2Vw9vu53Uf4dYpyWCyvfPbRskHfgt9q`.

A seed string is involved with the creation of private keys. When creating a private key using the LegalThings One
front-end, the seed is prepended with a 'nonce' field of 4 bytes (big-endian representation). This initially has a
value of 0 and increases every time you create the new account. We use this array of bytes for calculate hash
`keccak256(blake2b256(bytes))`. This resulting array of bytes we call `account seed`, from it you can generate a private
and public key pair. This is then converted to a public and private key pair for the `ED25519` algorithm.

Generate an `ED25519` key from the seed and than convert it to create the `X25519` key, or generate both keys from the
seed.

_NOTE: Not all random 32 bytes can be used as private keys (but any bytes of any size can be a seed). The signature
scheme for the ED25519 introduces restrictions on the keys, so create the keys only through the methods of the
Curve25519 libraries and be sure to make a test of the ability to sign data with a private key and then check it with a
public key, however obvious this test might seem._

**Recommended libraries**

* [libsodium]
* [tweetacl]

_Other libraries might work as well, but make sure they yield the same result as the recommended libraries. The Waves
API library is known to yield another result and shouldn't be used to create an account from seed._

[libsodium]: https://download.libsodium.org/doc/
[tweetacl]: https://github.com/dchest/tweetnacl-js/blob/master/README.md

##### Example

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

blake2b256\(account seed bytes\)

```text
6sKMMHVLyCQN7Juih2e9tbSmeE5Hu7L8XtBRgowJQvU7
```

Account seed \( keccak256\(blake2b256\(account seed bytes\)\) \)

```text
H4do9ZcPUASvtFJHvESapnxfmQ8tjBXMU7NtUARk9Jrf
```

Account seed after `SHA256` hashing \(optional, if your library does not do it yourself\)

```text
49mgaSSVQw6tDoZrHSr9rFySgHHXwgQbCRwFssboVLWX
```

Created private key

```text
3kMEhU5z3v8bmer1ERFUUhW58Dtuhyo9hE5vrhjqAWYT
```

Created public key

```text
HBqhfdFASRQ5eBBpu2y6c6KKi1az6bMx8v1JxX4iW1Q8
```

#### Signing

**ED25519** is used for all the signatures in the project.

The process is as follows: create the message for signing [example for an event], then create a signature from this
message using the private key.

Validation of signature requires the signature, the message and the public key.

Do not forget that there are many valid (not unique!) signatures for a one message. Also you should should not rely on
any information before the hash and/or signature are checked.

[example for an event]: http://schema.livecontract.io/event-chain/#signature

#### Encryption

Live Contracts uses **X25519** to encrypt data. This is a public key encryption schema, where the public key is used to
encrypt data and the private key is used to decrypt data.

### Symmetric encrypt

`AES256` in Galois/Counter Mode (gcm) may also be used to encrypt data. This is a symmetric encryption schema, where
the same key is used to encrypt and decrypt data.

Symmetric encryption SHOULD be applied when linking to external content in a template, document or comment.

You SHOULD always generate a new random key when using symmetric encryption. While the key SHOULD NOT be shared with
third parties, anybody that has a copy of the key can still do so.
