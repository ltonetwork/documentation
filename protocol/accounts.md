# Accounts

This section describes all the details of cryptographic algorithms which are used to:  
1. Create private and public keys from seed.  
2. Create addresses from public key.  
3. Create blocks and transactions signing.

We use:  
1. `Blake2b256` and `SHA2-256` algorithms \(in the form of hash chain\) to create a cryptographic hashes used .  
2. `ED25519` in order to create and verify signatures.  
3. `Base58` is used to create the string form of bytes.

## Bytes encoding Base58

All arrays of bytes in the project are encoded by Base58 algorithm with Bitcoin alphabet to make it easier for humans to read \(text readability\).

### Example

The string `teststring` is coded into the bytes `[5, 83, 9, -20, 82, -65, 120, -11]`. The bytes `[1, 2, 3, 4, 5]` are coded into the string `7bWpTW`.

## Creating a private key from a seed

A seed string is a representation of entropy, from which you can re-create deterministically all the private keys for one wallet. It should be long enough so that the probability of selection is an unrealistic negligible.

In fact, seed should be an array of bytes but for ease of memorization, the LTO wallet uses [Brainwallet](https://en.bitcoin.it/wiki/Brainwallet), to ensure that the seed is made up of words and easy to write down or remember. The application takes the UTF-8 bytes of the string and uses them to create keys and addresses.

For example, seed string `manage manual recall harvest series desert melt police rose hollow moral pledge kitten position add` after reading this string as UTF-8 bytes and encoding them to Base58, the string will be coded as `xrv7ffrv2A9g5pKSxt7gHGrPYJgRnsEMDyc4G7srbia6PhXYLDKVsDxnqsEqhAVbbko7N1tDyaSrWCZBoMyvdwaFNjWNPjKdcoZTKbKr2Vw9vu53Uf4dYpyWCyvfPbRskHfgt9q`.

A seed string is involved with the creation of private keys. To create private key using the official web wallet or the node, to 4 bytes of int 'nonce' field \(big-endian representation\), which initially has a value of 0 and increases every time you create the new address, should be prepended to seed bytes. Then we use this array of bytes for calculate hash `sha256(blake2b256(bytes))`. This resulting array of bytes we call `account seed`, from it you can deterministicly generate one private and public key pair. Then this bytes hash is passed in the method of creating a pair of public and private key of `ED25519` algorithm.

### Example

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

Account seed \( sha256\(blake2b256\(account seed bytes\)\) \)

```text
93dvzDQ8KBe4y7Nw89xsguWe8ZTVYGAA5kjvJ7miQS1v
```

Account seed after `Sha256` hashing \(optional, if your library does not do it yourself\)

```text
ETYQWXzC2h8VXahYdeUTXNPXEkan3vi9ikXbn912ijiw
```

Created private key

```text
4zsR9xoFpxfnNwLcY4hdRUarwf5xWtLj6FpKGDFBgscPxecPj2qgRNx4kJsFCpe9YDxBRNoeBWTh2SDAdwTySomS
```

Created public key

```text
GjSacB6a5DFNEHjDSmn724QsrRStKYzkahPH67wyrhAY
```

## Creating address from public key

Our network address obtained from the public key depends on the byte chainId \('T' for testnet and 'L' for mainnet\), so different networks obtained a different address for a single seed \(and hence public keys\). Creating a byte addresses described in more detail [here](https://github.com/ltonetwork/documentation/tree/c01951988c8797dc36ac6098133b139eaffade7c/technical-details/data-structures.md).

Example

For public key

```text
HBqhfdFASRQ5eBBpu2y6c6KKi1az6bMx8v1JxX4iW1Q8
```

in mainnet network \(chainId 'W'\) will be created this address

```text
3PPbMwqLtwBGcJrTA5whqJfY95GqnNnFMDX
```

