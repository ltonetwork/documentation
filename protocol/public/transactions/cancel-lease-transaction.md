---
description: The cancel lease transaction allows you to stop leasing to a node for staking.
---

# Cancel Lease

To cancel leasing, you need the id of the transaction that started the leasing. You can obtain a list of leases of an address via the `/leasing/active/{address}` endpoint on the REST API.

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

{% tabs %}
{% tab title="V3 \(current\)" %}
| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction type | Byte \(constant, value=9\) | 1 |
| 2 | Version | Byte \(constant, value=3\) | 1 |
| 3 | Chain id | Byte | 1 |
| 4 | Timestamp | Long | 8 |
| 5 | Sender's key type | KeyType \(Byte\) | 1 |
| 6 | Sender's public key | PublicKey \(Array\[Byte\]\) | 32 \| 33 |
| 7 | Fee | Long | 8 |
| 8 | Lease id | Transaction \(Array\[Byte\]\) | 32 |

{% hint style="info" %}
* Chain id can be obtained by taking the 2nd byte from the sender address.
* Each [key type](../../accounts.md#key-types) has a numeric id in addition to the reference from the JSON.
* Integers \(short, int, long\) have a big endian byte order.
{% endhint %}
{% endtab %}

{% tab title="V2" %}
| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction type | Byte \(constant, value=9\) | 1 |
| 2 | Version | Byte \(constant, value=2\) | 1 |
| 3 | Chain id | Byte | 1 |
| 4 | Sender's public key | PublicKey \(Array\[Byte\]\) | 32 |
| 5 | Fee | Long | 8 |
| 6 | Timestamp | Long | 8 |
| 7 | Lease id | Transaction \(Array\[Byte\]\) | 32 |

{% hint style="info" %}
* Chain id can be obtained by taking the 2nd byte from the sender address.
* Integers \(short, int, long\) have a big endian byte order.
{% endhint %}
{% endtab %}
{% endtabs %}

