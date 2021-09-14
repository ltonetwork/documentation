---
description: >-
  Create an association between account. An association can represent any kind
  of relationship. The meaning is defined by the association type.
---

# Association

### JSON

```javascript
{
    "type": 16,
    "version": 1,
    "party": "3N9ChkxWXqgdWLLErWFrSwjqARB6NtYsvZh",
    "associationType": 1,
    "hash": "3yMApqCuCjXDWPrbjfR5mjCPTHqFG8Pux1TxQrEM35jj",
    "id": "1uZqDjRjaehEceSxrVxz6WD6wt8su8TBHyDLQ1KFnJo",
    "sender": "3NBcx7AQqDopBj3WfwCVARNYuZyt1L9xEVM",
    "senderPublicKey": "7gghhSwKRvshZwwh6sG97mzo1qoFtHEQK7iM4vGcnEt7",
    "timestamp": 1610404930000,
    "fee": 100000000,
    "proofs": [
        "2jQMruoLoshfKe6FAUbA9vmVVvAt8bVpCFyM75Z2PLJiuRmjmLzFpM2UmgQ6E73qn46AVQprQJPBhQe92S7iSXbZ"
    ],
    "height": 1225712
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
| 1 | Transaction type | Byte \(constant, value=16\) | 1 |
| 2 | Version | Byte \(constant, value=1\) | 1 |
| 3 | Chain id | Byte | 1 |
| 4 | Sender's public key | PublicKey \(Array\[Byte\]\) | 32 |
| 5 | Party | Address \(Array\[Byte\]\) | 26 |
| 6 | Association type | Int | 4 |
| 7 | Includes hash | Boolean \(Byte\) | 1 |
| 8 | Hash length \(N\) | Short | 2 |
| 9 | Hash | Array\[Byte\] | N |
| 10 | Timestamp | Long | 8 |
| 11 | Fee | Long | 8 |
|  |  |  | **84+N** |

{% hint style="warning" %}
If the association doesn't include a hash, the hash length and hash should be omitted from the binary data. The binary of an association without a hash is **82 bytes** long.
{% endhint %}

{% hint style="info" %}
* Chain id can be obtained by taking the 2nd byte from the sender or recipient address.
* Integers \(short, int, long\) have a big endian byte order.
{% endhint %}

