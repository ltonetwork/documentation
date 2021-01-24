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
  "version": 1,
  "id": "8M6dgn85eh3bsHrVhWng8FNaHBcHEJD4MPZ5ZzCciyon",
  "sender": "3Jq8mnhRquuXCiFUwTLZFVSzmQt3Fu6F7HQ",
  "senderPublicKey": "AJVNfYjTvDD2GWKPejHbKPLxdvwXjAnhJzo6KCv17nne",
  "fee": 35000000,
  "timestamp": 1610397549043,
  "anchors": [
    "5SbkwAekNbaG8P1mTDdAE88mpWtCdET9vTmV2v9vQsCK"
  ],
  "proofs": [
    "4aMwABCZwtXrGGKmBdHdR5VVFqG51v5dPoyfDVZ7jfgD3jqc851ME5QkToQdfSRTqQmvnB9YT4tCBPcMzi59fZye"
  ],
  "height": 1069662
}
```

{% hint style="warning" %}
The JSON schema suggests that multiple anchors per transaction are supported, but this is disallowed by the consensus model. Only one hash per transaction is permitted.
{% endhint %}

{% hint style="info" %}
* `id` and `height` should be omitted when broadcasting. These fields are set by the node.
* Binary strings are base58 encoded.
* `timestamp` is in microseconds since epoch.
* `fee` includes 8 digits, so `LTO * 10^8`
{% endhint %}

### Binary schema

The binary data structure of the unsigned transaction.

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Version flag | Byte \(constant, value=0\) | 1 |
| 2 | Transaction type | Byte \(constant, value=15\) | 1 |
| 3 | Sender's public key | PublicKey \(Array\[Byte\]\) | 32 |
| 4 | Number of anchors | Short \(constant, value=1\) | 2 |
| 5 | Anchor length \(N\) | Short | 2 |
| 6 | Anchor | Array\[Byte\] | N |
| 7 | Timestamp | Long | 8 |
| 8 | Fee | Long | 8 |
|  |  |  | **54+N** |

{% hint style="info" %}
Integers \(short, int, long\) have a big endian byte order.
{% endhint %}

