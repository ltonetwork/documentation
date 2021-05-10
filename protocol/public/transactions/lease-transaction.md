---
description: The lease transaction leases an amount of tokens to a node for staking.
---

# Lease

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
* `timestamp` is in microseconds since epoch.
* `fee` and `amount` include 8 digits, so `LTO * 10^8`
{% endhint %}

### Binary schema

The binary data structure of the unsigned transaction.

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction type | Byte \(constant, value=8\) | 1 |
| 2 | Version | Byte \(constant, value=3\) | 1 |
| 3 | Timestamp | Long | 8 |
| 4 | Sender's key type | KeyType \(Byte\) | 1 |
| 5 | Sender's public key | PublicKey \(Array\[Byte\]\) | 32 \| 33 |
| 6 | Sponsor key type | KeyType \(Byte\) | 1 |
| 7 | Sponsor public key | PublicKey \(Array\[Byte\]\) | 0 \| 32 \| 33 |
| 8 | Fee | Long | 8 |
| 9 | Recipient | Address \(Array\[Byte\]\) | 26 |
| 10 | Amount | Long | 8 |

{% hint style="info" %}
Integers \(short, int, long\) have a big endian byte order.
{% endhint %}

