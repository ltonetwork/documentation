---
description: The cancel lease transaction allows you to stop leasing to a node for staking.
---

# Cancel Lease

To cancel leasing, you need the id of the transaction that started the leasing. You can obtain a list of leases of an address via the `/leasing/active/{address}` endpoint on the REST API.

### JSON

```javascript
{
  "type": 19,
  "version": 1,
  "recipient": "3N3Cn2pYtqzj7N9pviSesNe8KG9Cmb718Y1",
  "id": "BLMA4vkfe2S5UFHnoPyTh8SJmpTA1deh5SnWk1bdfjhq",
  "sender": "3MtHYnCkd3oFZr21yb2vEdngcSGXvuNNCq2",
  "senderPublicKey": "4EcSxUkMxqxBEBUBL2oKz3ARVsbyRJTivWpNrYQGdguz",
  "timestamp": 1519862400,
  "fee": 500000000,
  "proofs": [
    "2AKUBja93hF8AC2ee21m9AtedomXZNQG5J3FZMU85avjKF9B8CL45RWyXkXEeYb13r1AhpSzRvcudye39xggtDHv"
  ]
}
```

{% hint style="info" %}
* `id` and `height` should be omitted when broadcasting. These fields are set by the node.
* `chainId` can be obtained by taking the 2nd byte from the sender or recipient address.
* Binary strings are base58 encoded.
* `timestamp` is in milliseconds since epoch.
* `fee`includes 8 digits, so `LTO * 10^8`
{% endhint %}

### Binary schema

The binary data structure of the unsigned transaction.

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction type | Byte \(constant, value=9\) | 1 |
| 2 | Version | Byte \(constant, value=2\) | 1 |
| 3 | Chain id | Byte | 1 |
| 4 | Sender's public key | PublicKey \(Array\[Byte\]\) | 32 |
| 5 | Fee | Long | 8 |
| 6 | Timestamp | Long | 8 |
| 7 | Lease id | Transaction \(Array\[Byte\]\) | 32 |
|  |  |  | **83** |

{% hint style="info" %}
* Chain id can be obtained by taking the 2nd byte from the sender or recipient address.
* Integers \(short, int, long\) have a big endian byte order.
{% endhint %}

