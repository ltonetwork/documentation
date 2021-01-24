---
description: >-
  Revoke an association between accounts. Revoked associations are still
  visible.
---

# Revoke Association

The JSON and binary schema for revoking an association are identical to the schemas for creating an association.

{% hint style="success" %}
To revoke an association, the `sender`, `assocationType`, `party`, and `hash` need to be the same as in the transaction that created the association.
{% endhint %}

### JSON

```javascript
{
    "type": 17,
    "version": 1,
    "party": "3N9ChkxWXqgdWLLErWFrSwjqARB6NtYsvZh",
    "associationType": 1,
    "hash": "3yMApqCuCjXDWPrbjfR5mjCPTHqFG8Pux1TxQrEM35jj",
    "id": "HtxiY9x8aVBDfPvEUifYZuBEDge5TCDDAtqRGBW8HDef",
    "sender": "3NBcx7AQqDopBj3WfwCVARNYuZyt1L9xEVM",
    "senderPublicKey": "7gghhSwKRvshZwwh6sG97mzo1qoFtHEQK7iM4vGcnEt7",
    "timestamp": 1610406613000,
    "fee": 100000000,
    "proofs": [
        "N1tvyL3XNNPq9Ctx5o5gorSfVggFq1csGhwDQHcrwmict2AaoLfrVTvjZCxr8w1Qq9a3XUgBD5nTg21wmLQTUg5"
    ],
    "height": 1225745
}
```

{% hint style="info" %}
* `id` and `height` should be omitted when broadcasting. These fields are set by the node.
* `hash` is optional.
* Binary strings are base58 encoded.
* `timestamp` is in microseconds since epoch.
* `fee` includes 8 digits, so `LTO * 10^8`
{% endhint %}

### Binary schema

The binary data structure of the unsigned transaction.

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Version flag | Byte \(constant, value=0\) | 1 |
| 2 | Transaction type | Byte \(constant, value=17\) | 1 |
| 3 | Chain id | Byte | 1 |
| 4 | Sender's public key | PublicKey \(Array\[Byte\]\) | 32 |
| 5 | Party | Address \(Array\[Byte\]\) | 26 |
| 6 | Includes hash | Boolean \(Byte\) | 1 |
| 7 | Hash length \(N\) | Short | 2 |
| 8 | Hash | Array\[Byte\] | N |
| 9 | Timestamp | Long | 8 |
| 10 | Fee | Long | 8 |
|  |  |  | **80+N** |

{% hint style="warning" %}
If the association doesn't include a hash, the hash length and hash should be omitted from the binary data. The binary of an association without a hash is **78 bytes** long.
{% endhint %}

{% hint style="info" %}
* Chain id can be obtained by taking the 2nd byte from the sender or recipient address.
* Integers \(short, int, long\) have a big endian byte order.
{% endhint %}

