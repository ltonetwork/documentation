# Accounts

## Bytes encoding Base58

All arrays of bytes in the project are encoded by Base58 algorithm with Bitcoin alphabet to make it easier for humans to read \(text readability\).

### Example

The string `teststring` is coded into the bytes `[5, 83, 9, -20, 82, -65, 120, -11]`. The bytes `[1, 2, 3, 4, 5]` are coded into the string `7bWpTW`.

## Creating a private key from seed

A seed string is a representation of entropy, from which you can re-create deterministically all the private keys for one wallet. It should be long enough so that the probability of selection is an unrealistic negligible.

In fact, seed should be an array of bytes but for ease of memorization, the LTO wallet uses [Brainwallet](https://en.bitcoin.it/wiki/Brainwallet), to ensure that the seed is made up of words and easy to write down or remember. The application takes the UTF-8 bytes of the string and uses them to create keys and addresses.

For example, seed string `manage manual recall harvest series desert melt police rose hollow moral pledge kitten position add` after reading this string as UTF-8 bytes and encoding them to Base58, the string will be coded as `xrv7ffrv2A9g5pKSxt7gHGrPYJgRnsEMDyc4G7srbia6PhXYLDKVsDxnqsEqhAVbbko7N1tDyaSrWCZBoMyvdwaFNjWNPjKdcoZTKbKr2Vw9vu53Uf4dYpyWCyvfPbRskHfgt9q`.

A seed string is involved with the creation of private keys. To create private key using the official web wallet or the node, to 4 bytes of int 'nonce' field \(big-endian representation\), which initially has a value of 0 and increases every time you create the new address, should be prepended to seed bytes. Then we use this array of bytes for calculate hash `sha256(blake2b256(bytes))`. This resulting array of bytes we call `account seed`, from it you can deterministicly generate one private and public key pair. Then this bytes hash is passed in the method of creating a pair of public and private key of `ED25519` algorithm.

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

### Signing

**ED25519** is used for all the signatures in the project. Use NaCl or sodium to create an address from the account seed.

Created private key

```text
4zsR9xoFpxfnNwLcY4hdRUarwf5xWtLj6FpKGDFBgscPxecPj2qgRNx4kJsFCpe9YDxBRNoeBWTh2SDAdwTySomS
```

Created public key

```text
GjSacB6a5DFNEHjDSmn724QsrRStKYzkahPH67wyrhAY
```

### Asymmetric encryption

**X25519** is used for all the encryption in the project. Use NaCl or sodium to create an address from the account seed.

Created private key

```text
3kMEhU5z3v8bmer1ERFUUhW58Dtuhyo9hE5vrhjqAWYT
```

Created public key

```text
HBqhfdFASRQ5eBBpu2y6c6KKi1az6bMx8v1JxX4iW1Q8
```

## Creating address from public key

Our network address obtained from the **ED25519 \(signature\) public key.** It uses on the chain id \('T' for testnet and 'L' for mainnet\), so different networks result in a different address for the same seed / public key.

Example

For public key

```text
HBqhfdFASRQ5eBBpu2y6c6KKi1az6bMx8v1JxX4iW1Q8
```

in mainnet network \(chainId 'W'\) will be created this address

```text
3PPbMwqLtwBGcJrTA5whqJfY95GqnNnFMDX
```

