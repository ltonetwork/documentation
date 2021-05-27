---
description: >-
  Anchoring stores a hash on the blockchain, allowing anyone to verify that data
  hasn't been tempered with.
---

# Anchor

### JSON

```javascript
{
  "type": 15,
  "version": 3,
  "id": "8M6dgn85eh3bsHrVhWng8FNaHBcHEJD4MPZ5ZzCciyon",
  "sender": "3Jq8mnhRquuXCiFUwTLZFVSzmQt3Fu6F7HQ",
  "senderKeyType": "Ed25519",
  "senderPublicKey": "AJVNfYjTvDD2GWKPejHbKPLxdvwXjAnhJzo6KCv17nne",
  "fee": 35000000,
  "timestamp": 1610397549043,
  "anchors": [
    "5SbkwAekNbaG8P1mTDdAE88mpWtCdET9vTmV2v9vQsCK",
    ...
  ],
  "proofs": [
    "4aMwABCZwtXrGGKmBdHdR5VVFqG51v5dPoyfDVZ7jfgD3jqc851ME5QkToQdfSRTqQmvnB9YT4tCBPcMzi59fZye"
  ],
  "height": 1069662
}
```

{% hint style="info" %}
* `id` and `height` should be omitted when broadcasting. These fields are set by the node.
* Binary strings are base58 encoded.
* `timestamp` is in microseconds since epoch.
* `fee` includes 8 digits, so `LTO * 10^8`
{% endhint %}

### Constraints

The maximum number of anchors in a single transaction is 100. There is no minimum number of anchors.

Other than that, we've decided not to put any restrictions on transactions that are harmless, even if they may seem against common sense. For example, transfers to self are allowed, as well as zero-valued transfers. In the recipient list, a recipient can occur several times, this is not considered an error.

{% hint style="success" %}
An anchor transaction with 0 anchors is the cheapest method to register a public key for an [implicit DID](../../identities/decentralized-identifiers-did.md#implicit-identities).
{% endhint %}

### Fees

The Anchor fee is made up of two amounts: a base value, plus a value per anchor. By default the formula looks like:

```text
0.3 + 0.05 * N
```

where `N` is the number of anchors in the transaction. Fee can be configured in miner node settings using the usual syntax, just keep in mind that there are two parts to it, the base \(`BASE`\) fee and the variable \(`VAR`\) fee. Below is an excerpt from the configuration file that ships with the node:

```cpp
anchor {
  BASE = 30000000
  VAR = 5000000
}
```

### Binary schema

The binary data structure of the unsigned transaction.

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction type | Byte \(constant, value=15\) | 1 |
| 2 | Version | Byte \(constant, value=3\) | 1 |
| 3 | Chain id | Byte | 1 |
| 4 | Timestamp | Long | 8 |
| 5 | Sender's key type | KeyType \(Byte\) | 1 |
| 6 | Sender's public key | PublicKey \(Array\[Byte\]\) | 32 \| 33 |
| 7 | Fee | Long | 8 |
| 8 | Number of anchors | Short | 2 |
| 9 | Anchor length 1 \(N\) | Byte | 2 |
| 10 | Anchor 1 | Array\[Byte\] | N1 |
| ... |  |  |  |

{% hint style="info" %}
* Anchor length and Anchor are repeated for each transfer.
* Chain id can be obtained by taking the 2nd byte from the sender address.
* Each [key type](../../accounts.md#key-types) has a numeric id in addition to the reference from the JSON.
* Integers \(short, int, long\) have a big endian byte order.
{% endhint %}

