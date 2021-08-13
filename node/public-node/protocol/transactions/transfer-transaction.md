---
description: >-
  The transfer transaction allows the sender to transfer LTO tokens to the
  recipient.
---

# Transfer Transaction

### JSON

```javascript
{
    "id": "5a1ZVJTu8Y7mPA6BbkvGdfmbjvz9YSppQXPnb5MxihV5",
    "type": 4,
    "version": 2,
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
| 1 | Version flag | Byte \(constant, value=0\) | 1 |
| 2 | Transaction type | Byte \(constant, value=4\) | 1 |
| 3 | Sender's public key | PublicKey \(Array\[Byte\]\) | 32 |
| 4 | Timestamp | Long | 8 |
| 5 | Amount | Long | 8 |
| 6 | Fee | Long | 8 |
| 7 | Recipient | Address \(Array\[Byte\]\) | 26 |
| 8 | Attachment length \(N\) | Short | 2 |
| 9 | Attachment | Array\[Byte\] | N |
|  |  |  | **86+N** |

{% hint style="info" %}
Integers \(short, int, long\) have a big endian byte order.
{% endhint %}

