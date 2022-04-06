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
  "senderKeyType": "ed25519",
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

### Binary schema

The binary data structure of the unsigned transaction.

{% tabs %}
{% tab title="V3 (current)" %}
| #   |      Field Name     |            Type           | Length   |
| --- | :-----------------: | :-----------------------: | -------- |
| 1   |   Transaction type  | Byte (constant, value=15) | 1        |
| 2   |       Version       |  Byte (constant, value=3) | 1        |
| 3   |       Chain id      |            Byte           | 1        |
| 4   |      Timestamp      |            Long           | 8        |
| 5   |  Sender's key type  |       KeyType (Byte)      | 1        |
| 6   | Sender's public key |  PublicKey (Array\[Byte]) | 32 \| 33 |
| 7   |         Fee         |            Long           | 8        |
| 8   |  Number of anchors  |           Short           | 2        |
| 9   | Anchor 1 length (N) |            Byte           | 2        |
| 10  |       Anchor 1      |        Array\[Byte]       | N        |
| ... |                     |                           |          |

{% hint style="info" %}
* Anchor length and Anchor can be repeated for each anchor hash.
* Chain id can be obtained by taking the 2nd byte from the sender address.
* Each [key type](../../accounts.md#key-types) has a numeric id in addition to the reference from the JSON.
* Integers (short, int, long) have a big endian byte order.
{% endhint %}
{% endtab %}

{% tab title="V1" %}
| #   |      Field Name     |            Type           | Length |
| --- | :-----------------: | :-----------------------: | ------ |
| 1   |   Transaction type  | Byte (constant, value=15) | 1      |
| 2   |       Version       |  Byte (constant, value=1) | 1      |
| 3   | Sender's public key |  PublicKey (Array\[Byte]) | 32     |
| 4   |  Number of anchors  | Short (constant, value=1) | 2      |
| 5   | Anchor 1 length (N) |           Short           | 2      |
| 6   |       Anchor 1      |        Array\[Byte]       | N      |
| ... |                     |                           |        |
| 7   |      Timestamp      |            Long           | 8      |
| 8   |         Fee         |            Long           | 8      |

{% hint style="info" %}
* Anchor length and Anchor can be repeated for each anchor hash.
* Integers (short, int, long) have a big endian byte order.
{% endhint %}
{% endtab %}
{% endtabs %}
