---
description: Data transaction sets data entries in sender's account data storage.
---

# Data

### JSON

```javascript
{
  "type": 12,
  "version": 3,
  "id": "8M6dgn85eh3bsHrVhWng8FNaHBcHEJD4MPZ5ZzCciyon",
  "sender": "3Jq8mnhRquuXCiFUwTLZFVSzmQt3Fu6F7HQ",
  "senderKeyType": "ed25519",
  "senderPublicKey": "AJVNfYjTvDD2GWKPejHbKPLxdvwXjAnhJzo6KCv17nne",
  "fee": 14000000,
  "timestamp": 1610397549043,
  "data": [
    {
      "type": "boolean",
      "value": true,
      "key": "bool"
    },
    {
      "type": "binary",
      "value": "base64:SGVsbG8gV2F2ZXM=",
      "key": "bin"
    },
    {
      "type": "integer",
      "value": 1234567,
      "key": "int"
    },
    {
      "type": "string",
      "value": "some text",
      "key": "str"
    }
  ],
  "proofs": [
    "4aMwABCZwtXrGGKmBdHdR5VVFqG51v5dPoyfDVZ7jfgD3jqc851ME5QkToQdfSRTqQmvnB9YT4tCBPcMzi59fZye"
  ],
  "height": 1069662
}
```

{% hint style="info" %}
* `id` and `height` should be omitted when broadcasting. These fields are set by the node.
* Binary strings are base58 encoded.
* `timestamp` is in microseconds since epoch.
* `fee` includes 8 digits, so `LTO * 10^8`
{% endhint %}

### Data entries

Data entries are one of 4 types: "boolean", "integer", "string", and "binary". A data transaction can have a maximum of 100 data entries.

In JSON, the value of a binary data entry must be base64 encoded and prefixed with `"base64:"`. The value of the integer type is a signed long (8 bytes).

### Fees

The fees of a data transaction are determined by the size of the data entries. The base fee is 1 LTO, with an additional fee of 0.1 LTO per 256 bytes.

### Binary schema

The binary data structure of the unsigned transaction.

{% tabs %}
{% tab title="V3 (current)" %}
| 1   |   Transaction type  | Byte (constant, value=12) | 1        |
| --- | :-----------------: | :-----------------------: | -------- |
| 2   |       Version       |  Byte (constant, value=3) | 1        |
| 3   |       Chain id      |            Byte           | 1        |
| 4   |      Timestamp      |            Long           | 8        |
| 5   |  Sender's key type  |       KeyType (Byte)      | 1        |
| 6   | Sender's public key |  PublicKey (Array\[Byte]) | 32 \| 33 |
| 7   |         Fee         |            Long           | 8        |
| 8   |  Number of entries  |           Short           | 2        |
| 9   |       Entry 1       |                           |          |
| ... |                     |                           |          |

{% hint style="info" %}
* Anchor length and Anchor can be repeated for each anchor hash.
* Chain id can be obtained by taking the 2nd byte from the sender address.
* Each [key type](../../accounts.md#key-types) has a numeric id in addition to the reference from the JSON.
* Integers (short, int, long) have a big endian byte order.
{% endhint %}
{% endtab %}
{% endtabs %}

The encoding of a data entry depends on the type.

#### Integer

| Type  | Byte (constant, value=0) | 1 |
| ----- | ------------------------ | - |
| Value | Long                     | 8 |

#### Boolean

| Type  | Byte (constant, value=1) | 1 |
| ----- | ------------------------ | - |
| Value | Byte                     | 1 |

#### Binary

| Type       | Byte (constant, value=2) | 1 |
| ---------- | ------------------------ | - |
| Length (N) | Short                    | 2 |
| Value      | Array\[Byte]             | N |

#### String

| Type       | Byte (constant, value=3) | 1 |
| ---------- | ------------------------ | - |
| Length (N) | Short                    | 2 |
| Value      | String (UTF-8)           | N |

