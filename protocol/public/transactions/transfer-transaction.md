---
description: >-
  The transfer transaction allows the sender to transfer LTO tokens to the
  recipient.
---

# Transfer

### JSON

```javascript
{
    "id": "5a1ZVJTu8Y7mPA6BbkvGdfmbjvz9YSppQXPnb5MxihV5",
    "type": 4,
    "version": 3,
    "sender": "3N9ChkxWXqgdWLLErWFrSwjqARB6NtYsvZh",
    "senderPublicKey": "9NFb1rvMyr1k8f3wu3UP1RaEGsozBt9gF2CmPMGGA42m",
    "fee": 100000000,
    "timestamp": 1609639213556,
    "amount": 100000000000,
    "recipient": "3NBcx7AQqDopBj3WfwCVARNYuZyt1L9xEVM",
    "attachment": "9Ajdvzr",
    "proofs": [
        "3ftQ2ArKKXw655WdHy2TK1MGXeyzKRqMQYwFidekkyxLpzFGsTziSFsbM5RCFxrn32EzisMgPWtQVQ4e5UqKUcES"
    ],
    "height": 1212761
}
```

{% hint style="info" %}
* `id` and `height` should be omitted when broadcasting. These fields are set by the node.
* Binary strings \(including `attachment`\) are base58 encoded.
* `timestamp` is in microseconds since epoch.
* `fee` and `amount` include 8 digits, so `LTO * 10^8`
{% endhint %}

### Binary schema

The binary data structure of the unsigned transaction.

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction type | Byte \(constant, value=4\) | 1 |
| 2 | Version | Byte \(constant, value=3\) | 1 |
| 3 | Timestamp | Long | 8 |
| 4 | Sender's key type | KeyType \(Byte\) | 1 |
| 5 | Sender's public key | PublicKey \(Array\[Byte\]\) | 32 \| 33 |
| 6 | Sponsor key type | KeyType \(Byte\) | 1 |
| 7 | Sponsor public key | PublicKey \(Array\[Byte\]\) | 0 \| 32 \| 33 |
| 8 | Fee | Long | 8 |
| 9 | Recipient | Address \(Array\[Byte\]\) | 26 |
| 10 | Amount | Long | 8 |
| 11 | Attachment length \(N\) | Byte | 2 |
| 12 | Attachment | Array\[Byte\] | N |

{% hint style="info" %}
Integers \(short, int, long\) have a big endian byte order.
{% endhint %}

