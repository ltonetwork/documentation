---
description: >-
  Create an association between account. An association can represent any kind
  of relationship. The meaning is defined by the association type.
---

# Association

### JSON

```javascript
{
  "type": 16,
  "version": 3,
  "id": "1uZqDjRjaehEceSxrVxz6WD6wt8su8TBHyDLQ1KFnJo",
  "sender": "3NBcx7AQqDopBj3WfwCVARNYuZyt1L9xEVM",
  "senderKeyType": "Ed25519",
  "senderPublicKey": "7gghhSwKRvshZwwh6sG97mzo1qoFtHEQK7iM4vGcnEt7",
  "recipient": "3N9ChkxWXqgdWLLErWFrSwjqARB6NtYsvZh",
  "associationType": 1,
  "hash": "3yMApqCuCjXDWPrbjfR5mjCPTHqFG8Pux1TxQrEM35jj",
  "timestamp": 1610404930000,
  "expires": 1641961856000,
  "fee": 100000000,
  "proofs": [
    "2jQMruoLoshfKe6FAUbA9vmVVvAt8bVpCFyM75Z2PLJiuRmjmLzFpM2UmgQ6E73qn46AVQprQJPBhQe92S7iSXbZ"
  ],
  "height": 1225712
}
```

{% hint style="info" %}
* `id` and `height` should be omitted when broadcasting. These fields are set by the node.
* `hash` is optional.
* Binary strings are base58 encoded.
* `timestamp` and `expires` is in microseconds since epoch.
* `fee` includes 8 digits, so `LTO * 10^8`
{% endhint %}

{% hint style="warning" %}
Associations that are recently expired may still be returned by the node. The public node will use the time of the last mined block to determine if an association is expired or not.
{% endhint %}

### Binary schema

The binary data structure of the unsigned transaction.

{% tabs %}
{% tab title="V3 (current)" %}
| #  |      Field Name     |            Type           | Length   |
| -- | :-----------------: | :-----------------------: | -------- |
| 1  |   Transaction type  | Byte (constant, value=16) | 1        |
| 2  |       Version       |  Byte (constant, value=3) | 1        |
| 3  |      Network id     |            Byte           | 1        |
| 4  |      Timestamp      |            Long           | 8        |
| 5  |  Sender's key type  |       KeyType (Byte)      | 1        |
| 6  | Sender's public key |  PublicKey (Array\[Byte]) | 32 \| 33 |
| 7  |         Fee         |            Long           | 8        |
| 8  |      Recipient      |   Address (Array\[Byte])  | 26       |
| 9  |   Association type  |            Int            | 4        |
| 10 |       Expires       |            Long           | 8        |
| 11 |   Hash length (N)   |           Short           | 2        |
| 12 |         Hash        |        Array\[Byte]       | N        |

{% hint style="info" %}
* If the association doesn't expire, the expiry timestamp in the binary data must be zero.
* Network id can be obtained by taking the 2nd byte from the sender address.
* Each [key type](../../accounts/#key-types) has a numeric id in addition to the reference from the JSON.
* Integers (short, int, long) have a big endian byte order.
{% endhint %}
{% endtab %}

{% tab title="V1" %}
| #  |      Field Name     |            Type           | Length |
| -- | :-----------------: | :-----------------------: | ------ |
| 1  |   Transaction type  | Byte (constant, value=16) | 1      |
| 2  |       Version       |  Byte (constant, value=1) | 1      |
| 3  |      Network id     |            Byte           | 1      |
| 4  | Sender's public key |  PublicKey (Array\[Byte]) | 32     |
| 5  |        Party        |   Address (Array\[Byte])  | 26     |
| 6  |   Association type  |            Int            | 4      |
| 7  |    Includes hash    |       Boolean (Byte)      | 1      |
| 8  |   Hash length (N)   |           Short           | 2      |
| 9  |         Hash        |        Array\[Byte]       | N      |
| 10 |      Timestamp      |            Long           | 8      |
| 11 |         Fee         |            Long           | 8      |

{% hint style="warning" %}
If the association doesn't include a hash, the hash length and hash should be omitted from the binary data.
{% endhint %}

{% hint style="info" %}
* Chain id can be obtained by taking the 2nd byte from the sender or recipient address.
* Integers (short, int, long) have a big endian byte order.
{% endhint %}
{% endtab %}
{% endtabs %}
