---
description: Stop sponsoring an account
---

# Cancel sponsor

### JSON

```javascript
{
    "type": 19,
    "version": 1,
    "recipient": "3N9ChkxWXqgdWLLErWFrSwjqARB6NtYsvZh",
    "id": "53r3mwknCUJmyacf1TP1A5zUGCF9z3N951Zegs9UrkZD",
    "sender": "3NBcx7AQqDopBj3WfwCVARNYuZyt1L9xEVM",
    "senderPublicKey": "7gghhSwKRvshZwwh6sG97mzo1qoFtHEQK7iM4vGcnEt7",
    "timestamp": 1610412950000,
    "fee": 500000000,
    "proofs": [
        "RexaACH8AVfNKQcKDRVCvF2nSAzJLZPyUTtD9KmtikBy5CVCpVeBp78m2Myy7ekkecDMaJwERjgTVxjSxeLd8Da"
    ],
    "height": 1225860
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

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Version flag | Byte \(constant, value=0\) | 1 |
| 2 | Transaction type | Byte \(constant, value=19\) | 1 |
| 3 | Chain id | Byte | 1 |
| 4 | Sender's public key | PublicKey \(Array\[Byte\]\) | 32 |
| 5 | Recipient | Address \(Array\[Byte\]\) | 26 |
| 6 | Fee | Long | 8 |
| 7 | Timestamp | Long | 8 |
|  |  |  | **77** |

{% hint style="info" %}
* Chain id can be obtained by taking the 2nd byte from the sender or recipient address.
* Integers \(short, int, long\) have a big endian byte order.
{% endhint %}

