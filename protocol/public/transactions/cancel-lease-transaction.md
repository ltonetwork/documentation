---
description: The cancel lease transaction allows you to stop leasing to a node for staking.
---

# Cancel Lease

To cancel leasing, you need the id of the transaction that started the leasing. You can obtain a list of leases of an address via the `/leasing/active/{address}` endpoint on the REST API.

### JSON

```javascript
TODO
```

{% hint style="info" %}
* `id` and `height` should be omitted when broadcasting. These fields are set by the node.
* `chainId` can be obtained by taking the 2nd byte from the sender or recipient address.
* Binary strings are base58 encoded.
* `timestamp` is in microseconds since epoch.
* `fee`includes 8 digits, so `LTO * 10^8`
{% endhint %}

### Binary schema

The binary data structure of the unsigned transaction.

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction type | Byte \(constant, value=9\) | 1 |
| 2 | Version | Byte \(constant, value=3\) | 1 |
| 3 | Timestamp | Long | 8 |
| 4 | Sender's key type | KeyType \(Byte\) | 1 |
| 5 | Sender's public key | PublicKey \(Array\[Byte\]\) | 32 \| 33 |
| 6 | Sponsor key type | KeyType \(Byte\) | 1 |
| 7 | Sponsor public key | PublicKey \(Array\[Byte\]\) | 0 \| 32 \| 33 |
| 8 | Fee | Long | 8 |
| 9 | Lease id | Transaction \(Array\[Byte\]\) | 32 |

{% hint style="info" %}
* Chain id can be obtained by taking the 2nd byte from the sender or recipient address.
* Integers \(short, int, long\) have a big endian byte order.
{% endhint %}

