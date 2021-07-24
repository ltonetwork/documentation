---
description: The lease transaction leases an amount of tokens to a node for staking.
---

# Lease Transaction

### JSON

```javascript
  {
    "type": 8,
    "id": "6XmeG7SRWiw8pD6Uad6D9AAaY354v5TV6AJMhPpHMkqy",
    "sender": "3JorA3ddE7i6fhgBjSuW6jNTYS8D4EZUzio",
    "senderPublicKey": "AWwAdRHFmSqTCMHJ346wFSbJUsGUzQYCzuqXWgaT4gL6",
    "fee": 100000000,
    "timestamp": 1607010190710,
    "proofs": [
      "2BK6wTH75N78ixT273kArQxTo6NHvSVWQvtubZ5PTVdybcwomoUFjcYdfxqY6Xk7BpePjDbyr9aWdE5iZxQLq63J"
    ],
    "version": 2,
    "amount": 553600000000,
    "recipient": "3JurbYJAuc8iMeujvS39L8cSv9kMfxDACYR",
    "height": 1012314
  }
```

{% hint style="info" %}
* `id` and `height` should be omitted when broadcasting. These fields are set by the node.
* Binary strings are base58 encoded.
* `timestamp` is in milliseconds since epoch.
* `fee` and `amount` include 8 digits, so `LTO * 10^8`
{% endhint %}

### Binary schema

The binary data structure of the unsigned transaction.

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Version flag | Byte \(constant, value=0\) | 1 |
| 2 | Transaction type | Byte \(constant, value=4\) | 1 |
| 3 | - | Byte \(constant, value=0\) | 1 |
| 4 | Sender's public key | PublicKey \(Array\[Byte\]\) | 32 |
| 5 | Recipient | Address \(Array\[Byte\]\) | 26 |
| 5 | Amount | Long | 8 |
| 6 | Fee | Long | 8 |
| 7 | Timestamp | Long | 8 |
|  |  |  | **85** |

{% hint style="info" %}
Integers \(short, int, long\) have a big endian byte order.
{% endhint %}

