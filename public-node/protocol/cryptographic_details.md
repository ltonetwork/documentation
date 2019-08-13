# Cryptographic Details

## Description

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

Our network address obtained from the public key depends on the byte chainId \('T' for testnet and 'L' for mainnet\), so different networks obtained a different address for a single seed \(and hence public keys\). Creating a byte addresses described in more detail [here](/technical-details/data-structures.md).

Example

For public key

```text
HBqhfdFASRQ5eBBpu2y6c6KKi1az6bMx8v1JxX4iW1Q8
```

in mainnet network \(chainId 'W'\) will be created this address

```text
3PPbMwqLtwBGcJrTA5whqJfY95GqnNnFMDX
```

## Signing

`ED25519` is used for all the signatures in the project.

The process is as follows: create the special bytes for signing \(for transaction or block, you can find it [here](/technical-details/data-structures.md)\), then create a signature using these bytes and the private key bytes.

For the validation of signature is enough signature bytes, signed object bytes and the public key.

Do not forget that there are many valid \(not unique!\) signatures for a one array of bytes \(block or transaction\). Also you should not assume that the id of block or transaction is unique. The collision can occur one day! They have already taken place for some weak keys.

### Example

Transaction data:

| Field | Value |
| :--- | :--- |
| Sender address \(not used, just for information\) | 3N9Q2sdkkhAnbR4XCveuRaSMLiVtvebZ3wp |
| Private key \(used for signing, not in tx data\) | 7VLYNhmuvAo5Us4mNGxWpzhMSdSSdEbEPFUDKSnA6eBv |
| Public key | EENPV1mRhUD9gSKbcWt84cqnfSGQP5LkCu5gMBfAanYH |
| Recipient address | 3NBVqYXrapgJP9atQccdBPAgJPwHDKkh6A8 |
| Amount | 1 |
| Fee | 1 |
| Fee asset id | BG39cCNUFWPQYeyLnu7tjKHaiUGRxYwJjvntt9gdDPxG |
| Timestamp | 1479287120875 |
| Attachment \(as byte array\) | \[1, 2, 3, 4\] |

Bytes:

| \# | Field name | Type | Position | Length | Value | Base58 bytes value |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Transaction type \(0x04\) | Byte | 0 | 1 | 4 | 5 |
| 2 | Sender's public key | Bytes | 1 | 32 | ... | EENPV1mRhUD9gSKbcWt84cqnfSGQP5LkCu5gMBfAanYH |
| 3 | Timestamp | Long | 33 | 8 | 1479287120875 | 11frnYASv |
| 4 | Amount | Long | 41 | 8 | 1 | 11111112 |
| 5 | Fee | Long | 49 | 8 | 1 | 11111112 |
| 6 | Recipient's address | Bytes | 57 | 26 | ... | 3NBVqYXrapgJP9atQccdBPAgJPwHDKkh6A8 |
| 7 | Attachment's length \(N\) | Short | 83 | 2 | 4 | 15 |
| 8 | Attachment's bytes | Bytes | 85 | N | \[1,2,3,4\] | 2VfUX |

_**Total data bytes for sign:**_

`Ht7FtLJBrnukwWtywum4o1PbQSNyDWMgb4nXR5ZkV78krj9qVt17jz74XYSrKSTQe6wXuPdt3aCvmnF5hfjhnd1gyij36hN1zSDaiDg3TFi7c7RbXTHDDUbRgGajXci8PJB3iJM1tZvh8AL5wD4o4DCo1VJoKk2PUWX3cUydB7brxWGUxC6mPxKMdXefXwHeB4khwugbvcsPgk8F6YB`

_**Signature of transaction data bytes**_ **\(one of an infinite number of valid signatures\):**

`2mQvQFLQYJBe9ezj7YnAQFq7k9MxZstkrbcSKpLzv7vTxUfnbvWMUyyhJAc1u3vhkLqzQphKDecHcutUrhrHt22D`

_**Total transaction bytes with signature:**_

`6zY3LYmrh981Qbzj7SRLQ2FP9EmXFpUTX9cA7bD5b7VSGmtoWxfpCrP4y5NPGou7XDYHx5oASPsUzB92aj3623SUpvc1xaaPjfLn6dCPVEa6SPjTbwvmDwMT8UVoAfdMwb7t4okLcURcZCFugf2Wc9tBGbVu7mgznLGLxooYiJmRQSeAACN8jYZVnUuXv4V7jrDJVXTFNCz1mYevnpA5RXAoehPRXKiBPJLnvVmV2Wae2TCNvweHGgknioZU6ZaixSCxM1YzY24Prv9qThszohojaWq4cRuRHwMAA5VUBvUs`

## Calculating Transaction Id

Transaction Id is not stored in the transaction bytes and for most of transactions \(except Payment\) it can be easily calculated from the special bytes for signing using`blake2b256(bytes_for_signing)`. For Payment transaction Id is just the signature of this transaction.

