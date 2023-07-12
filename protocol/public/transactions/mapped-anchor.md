---
description: Mapped Anchoring stores hashes as key / value pairs on the blockchain.
---

# Mapped Anchor

{% hint style="success" %}
The mapped anchor transaction is part of the **Titanium** release.
{% endhint %}

### JSON

```json
{
  "type": 22,
  "version": 3,
  "id": "Ebq8DzaxstCL3iuuh1ByAiNEEbTRZ5T4ZzFMM4KNWdh9",
  "sender": "3NBcx7AQqDopBj3WfwCVARNYuZyt1L9xEVM",
  "senderKeyType": "ed25519",
  "senderPublicKey": "7gghhSwKRvshZwwh6sG97mzo1qoFtHEQK7iM4vGcnEt7",
  "fee": 45000000,
  "timestamp": 1689161703599,
  "anchors": {
    "8RJ5SFxLMtk3Hhewt3u43qGHt4QEjhkAQPH35VKNhuGB": "CXShAY5JEs29BKTjcq81rxAvcBJFWN966S88pr85Zgre",
    "EBPWyefK6BXDmANjw4kDoy3ZhyDVT7ngU8Bo6ASf3x3a": "Grr6KUBEMdfGmj2yZ1YrTrXLiNA4qj8KnBwHYxnVheht"
  },
  "proofs": [
    "5rtzM1JYAQd1Pi7q2oTCaR67yTCKwqgBJ3WmKjSMtHi8TLcFmoTG4t8v4z1gdmX3QBEDssy8oFaM6DJ8D3rbrC85"
  ],
  "height": 2547600,
  "effectiveFee": 37190250
}
```

{% hint style="info" %}
* `id` and `height` should be omitted when broadcasting. These fields are set by the node.
* Binary strings are base58 encoded.
* `timestamp` is in microseconds since epoch.
* `fee` includes 8 digits, so `LTO * 10^8`
{% endhint %}

### Batch transaction

Up to 100 pairs can be anchored in a single transaction. Bundling multiple pairs in 1 transaction can help reduce transaction fees.

The Mapped Anchor fee is made up of two amounts: a fixed one plus a per-anchor one. The fees are calculated as:

```
0.25 + 0.1 * N
```

where `N` is the number of anchor pairs in the transaction. The total is rounded up to the nearest 100\_000.

### Binary schema

The binary data structure of the unsigned transaction.

{% tabs %}
{% tab title="V3" %}
<table><thead><tr><th width="66">#</th><th width="279" align="center">Field Name</th><th width="254" align="center">Type</th><th>Length</th></tr></thead><tbody><tr><td>1</td><td align="center">Transaction type</td><td align="center">Byte (constant, value=15)</td><td>1</td></tr><tr><td>2</td><td align="center">Version</td><td align="center">Byte (constant, value=3)</td><td>1</td></tr><tr><td>3</td><td align="center">Network id</td><td align="center">Byte</td><td>1</td></tr><tr><td>4</td><td align="center">Timestamp</td><td align="center">Long</td><td>8</td></tr><tr><td>5</td><td align="center">Sender's key type</td><td align="center">KeyType (Byte)</td><td>1</td></tr><tr><td>6</td><td align="center">Sender's public key</td><td align="center">PublicKey (Array[Byte])</td><td>32 | 33</td></tr><tr><td>7</td><td align="center">Fee</td><td align="center">Long</td><td>8</td></tr><tr><td>8</td><td align="center">Number of anchors</td><td align="center">Short</td><td>2</td></tr><tr><td>9</td><td align="center">Anchor 1 key length (N)</td><td align="center">Short</td><td>2</td></tr><tr><td>10</td><td align="center">Anchor 1 key</td><td align="center">Array[Byte]</td><td>N</td></tr><tr><td>11</td><td align="center">Anchor 1 value length (M)</td><td align="center">Short</td><td>2</td></tr><tr><td>12</td><td align="center">Anchor 1 value</td><td align="center">Array[Byte]</td><td>N</td></tr><tr><td>...</td><td align="center"></td><td align="center"></td><td></td></tr></tbody></table>

{% hint style="info" %}
* Anchor key length, Anchor key, Anchor value length, and Anchor value are repeated for each anchor hash.
* The maximum length of an anchor is 100 bytes.
* Network id can be obtained by taking the 2nd byte from the sender address.
* Each [key type](../../accounts/#key-types) has a numeric id in addition to the reference from the JSON.
* Integers (short, int, long) have a big endian byte order.
{% endhint %}
{% endtab %}
{% endtabs %}
