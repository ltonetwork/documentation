---
description: Stop sponsoring an account
---

# Cancel Sponsorship

```javascript
{
  "type": 19,
  "version": 3,
  "recipient": "3N3Cn2pYtqzj7N9pviSesNe8KG9Cmb718Y1",
  "id": "BLMA4vkfe2S5UFHnoPyTh8SJmpTA1deh5SnWk1bdfjhq",
  "sender": "3MtHYnCkd3oFZr21yb2vEdngcSGXvuNNCq2",
  "senderKeyType": "Ed25519",
  "senderPublicKey": "4EcSxUkMxqxBEBUBL2oKz3ARVsbyRJTivWpNrYQGdguz",
  "timestamp": 1519862400,
  "fee": 500000000,
  "proofs": [
    "2AKUBja93hF8AC2ee21m9AtedomXZNQG5J3FZMU85avjKF9B8CL45RWyXkXEeYb13r1AhpSzRvcudye39xggtDHv"
  ],
  "height": 1248655
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

{% tabs %}
{% tab title="V3 (current)" %}
| # |      Field Name     |            Type           | Length   |
| - | :-----------------: | :-----------------------: | -------- |
| 1 |   Transaction type  | Byte (constant, value=18) | 1        |
| 2 |       Version       |  Byte (constant, value=3) | 1        |
| 3 |       Chain id      |            Byte           | 1        |
| 4 |      Timestamp      |            Long           | 8        |
| 5 |  Sender's key type  |       KeyType (Byte)      | 1        |
| 6 | Sender's public key |  PublicKey (Array\[Byte]) | 32 \| 33 |
| 7 |         Fee         |            Long           | 8        |
| 8 |      Recipient      |   Address (Array\[Byte])  | 26       |

{% hint style="info" %}
* Chain id can be obtained by taking the 2nd byte from the sender address.
* Each [key type](../../accounts/#key-types) has a numeric id in addition to the reference from the JSON.
* Integers (short, int, long) have a big endian byte order.
{% endhint %}
{% endtab %}

{% tab title="V1" %}
| # |      Field Name     |            Type           | Length |
| - | :-----------------: | :-----------------------: | ------ |
| 1 |   Transaction type  | Byte (constant, value=19) | 1      |
| 2 |       Version       |  Byte (constant, value=1) | 1      |
| 3 |       Chain id      |            Byte           | 1      |
| 4 | Sender's public key |  PublicKey (Array\[Byte]) | 32     |
| 5 |      Recipient      |   Address (Array\[Byte])  | 26     |
| 6 |         Fee         |            Long           | 8      |
| 7 |      Timestamp      |            Long           | 8      |

{% hint style="info" %}
* Chain id can be obtained by taking the 2nd byte from the sender or recipient address.
* Integers (short, int, long) have a big endian byte order.
{% endhint %}
{% endtab %}
{% endtabs %}
