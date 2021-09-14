---
description: >-
  Revoke an association between accounts. Revoked associations are still
  visible.
---

# Revoke Association

The JSON and binary schema for revoking an association are identical to the schemas for creating an association.

{% hint style="success" %}
To revoke an association, the `sender`, `assocationType`, `recipient`, and `hash` need to be the same as in the transaction that created the association.
{% endhint %}

### JSON

```javascript
{
  "type": 17,
  "version": 3,
  "id": "HtxiY9x8aVBDfPvEUifYZuBEDge5TCDDAtqRGBW8HDef",
  "sender": "3NBcx7AQqDopBj3WfwCVARNYuZyt1L9xEVM",
  "senderKeyType": "Ed25519",
  "senderPublicKey": "7gghhSwKRvshZwwh6sG97mzo1qoFtHEQK7iM4vGcnEt7",
  "recipient": "3N9ChkxWXqgdWLLErWFrSwjqARB6NtYsvZh",
  "associationType": 1,
  "hash": "3yMApqCuCjXDWPrbjfR5mjCPTHqFG8Pux1TxQrEM35jj",
  "timestamp": 1610406613000,
  "fee": 100000000,
  "proofs": [
    "N1tvyL3XNNPq9Ctx5o5gorSfVggFq1csGhwDQHcrwmict2AaoLfrVTvjZCxr8w1Qq9a3XUgBD5nTg21wmLQTUg5"
  ],
  "height": 1225745
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

{% tabs %}
{% tab title="V3 \(current\)" %}
| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction type | Byte \(constant, value=17\) | 1 |
| 2 | Version | Byte \(constant, value=3\) | 1 |
| 3 | Chain id | Byte | 1 |
| 4 | Timestamp | Long | 8 |
| 5 | Sender's key type | KeyType \(Byte\) | 1 |
| 6 | Sender's public key | PublicKey \(Array\[Byte\]\) | 32 \| 33 |
| 7 | Fee | Long | 8 |
| 8 | Recipient | Address \(Array\[Byte\]\) | 26 |
| 9 | Association type | Int | 4 |
| 10 | Hash length \(N\) | Short | 2 |
| 11 | Hash | Array\[Byte\] | N |

{% hint style="info" %}
* Chain id can be obtained by taking the 2nd byte from the sender address.
* If the association doesn't have a hash, the hash length should be zero.
* Each [key type](../../accounts.md#key-types) has a numeric id in addition to the reference from the JSON.
* Integers \(short, int, long\) have a big endian byte order.
{% endhint %}
{% endtab %}

{% tab title="V1" %}
| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction type | Byte \(constant, value=17\) | 1 |
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

{% hint style="warning" %}
If the association doesn't include a hash, the hash length and hash should be omitted from the binary data.
{% endhint %}

{% hint style="info" %}
* Chain id can be obtained by taking the 2nd byte from the sender or recipient address.
* Integers \(short, int, long\) have a big endian byte order.
{% endhint %}
{% endtab %}
{% endtabs %}

