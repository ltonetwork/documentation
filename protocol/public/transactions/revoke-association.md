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
  "subject": "3yMApqCuCjXDWPrbjfR5mjCPTHqFG8Pux1TxQrEM35jj",
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
{% tab title="V4" %}
{% hint style="success" %}
Version 4 of the revoke anchor transaction is part of the **Titanium** release.
{% endhint %}

<table><thead><tr><th width="76">#</th><th width="245" align="center">Field Name</th><th width="273" align="center">Type</th><th>Length</th></tr></thead><tbody><tr><td>1</td><td align="center">Transaction type</td><td align="center">Byte (constant, value=17)</td><td>1</td></tr><tr><td>2</td><td align="center">Version</td><td align="center">Byte (constant, value=3)</td><td>1</td></tr><tr><td>3</td><td align="center">Chain id</td><td align="center">Byte</td><td>1</td></tr><tr><td>4</td><td align="center">Timestamp</td><td align="center">Long</td><td>8</td></tr><tr><td>5</td><td align="center">Sender's key type</td><td align="center">KeyType (Byte)</td><td>1</td></tr><tr><td>6</td><td align="center">Sender's public key</td><td align="center">PublicKey (Array[Byte])</td><td>32 | 33</td></tr><tr><td>7</td><td align="center">Fee</td><td align="center">Long</td><td>8</td></tr><tr><td>8</td><td align="center">Association type</td><td align="center">Int</td><td>4</td></tr><tr><td>9</td><td align="center">Recipient</td><td align="center">Address (Array[Byte])</td><td>26</td></tr><tr><td>10</td><td align="center">Subject length (N)</td><td align="center">Short</td><td>2</td></tr><tr><td>11</td><td align="center">Subject</td><td align="center">Array[Byte]</td><td>N</td></tr></tbody></table>

{% hint style="info" %}
* Network id can be obtained by taking the 2nd byte from the sender address.
* If the association doesn't have a hash, the hash length should be zero.
* Each [key type](../../accounts/#key-types) has a numeric id in addition to the reference from the JSON.
* Integers (short, int, long) have a big endian byte order.
{% endhint %}
{% endtab %}

{% tab title="V3 (current)" %}
<table><thead><tr><th width="76">#</th><th width="261" align="center">Field Name</th><th width="273" align="center">Type</th><th>Length</th></tr></thead><tbody><tr><td>1</td><td align="center">Transaction type</td><td align="center">Byte (constant, value=17)</td><td>1</td></tr><tr><td>2</td><td align="center">Version</td><td align="center">Byte (constant, value=3)</td><td>1</td></tr><tr><td>3</td><td align="center">Chain id</td><td align="center">Byte</td><td>1</td></tr><tr><td>4</td><td align="center">Timestamp</td><td align="center">Long</td><td>8</td></tr><tr><td>5</td><td align="center">Sender's key type</td><td align="center">KeyType (Byte)</td><td>1</td></tr><tr><td>6</td><td align="center">Sender's public key</td><td align="center">PublicKey (Array[Byte])</td><td>32 | 33</td></tr><tr><td>7</td><td align="center">Fee</td><td align="center">Long</td><td>8</td></tr><tr><td>8</td><td align="center">Recipient</td><td align="center">Address (Array[Byte])</td><td>26</td></tr><tr><td>9</td><td align="center">Association type</td><td align="center">Int</td><td>4</td></tr><tr><td>10</td><td align="center">Subject length (N)</td><td align="center">Short</td><td>2</td></tr><tr><td>11</td><td align="center">Subject</td><td align="center">Array[Byte]</td><td>N</td></tr></tbody></table>

{% hint style="info" %}
* Network id can be obtained by taking the 2nd byte from the sender address.
* If the association doesn't have a hash, the hash length should be zero.
* Each [key type](../../accounts/#key-types) has a numeric id in addition to the reference from the JSON.
* Integers (short, int, long) have a big endian byte order.
{% endhint %}


{% endtab %}

{% tab title="V1" %}
<table><thead><tr><th width="76">#</th><th width="253" align="center">Field Name</th><th width="259" align="center">Type</th><th>Length</th></tr></thead><tbody><tr><td>1</td><td align="center">Transaction type</td><td align="center">Byte (constant, value=17)</td><td>1</td></tr><tr><td>2</td><td align="center">Version</td><td align="center">Byte (constant, value=1)</td><td>1</td></tr><tr><td>3</td><td align="center">Chain id</td><td align="center">Byte</td><td>1</td></tr><tr><td>4</td><td align="center">Sender's public key</td><td align="center">PublicKey (Array[Byte])</td><td>32</td></tr><tr><td>5</td><td align="center">Party</td><td align="center">Address (Array[Byte])</td><td>26</td></tr><tr><td>6</td><td align="center">Association type</td><td align="center">Int</td><td>4</td></tr><tr><td>7</td><td align="center">Includes subject</td><td align="center">Boolean (Byte)</td><td>1</td></tr><tr><td>8</td><td align="center">Subject length (N)</td><td align="center">Short</td><td>2</td></tr><tr><td>9</td><td align="center">Subject</td><td align="center">Array[Byte]</td><td>N</td></tr><tr><td>10</td><td align="center">Timestamp</td><td align="center">Long</td><td>8</td></tr><tr><td>11</td><td align="center">Fee</td><td align="center">Long</td><td>8</td></tr></tbody></table>

{% hint style="warning" %}
If the association doesn't include a hash, the hash length and hash should be omitted from the binary data.
{% endhint %}

{% hint style="info" %}
* Chain id can be obtained by taking the 2nd byte from the sender or recipient address.
* Integers (short, int, long) have a big endian byte order.
{% endhint %}
{% endtab %}
{% endtabs %}
