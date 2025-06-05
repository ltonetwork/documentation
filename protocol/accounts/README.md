# Accounts

LTO network supports multiple cryptographic algorithms for signatures and encryption.

| id | reference                 | type                | curve      |
| -- | ------------------------- | ------------------- | ---------- |
| 1  | [ed25519](ed25519.md)     | EdDSA               | curve25519 |
| 2  | [secp256k1](secp256k1.md) | ECDSA               | secp256k1  |
| 3  | [secp256r1](secp256r1.md) | ECDSA               | secp256r1  |
| 4  | bls12-381                 | BLS (pairing-based) | BLS12-381  |

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

Derived identity addresses are only used for [decentralized identifiers (DIDs)](../identities/decentralized-identifiers.md) and can't be used to sign transactions on the public blockchain.
