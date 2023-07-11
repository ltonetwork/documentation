---
description: >-
  A public statement made by an account. The meaning is defined by the statement
  type.
---

# Statement

{% hint style="success" %}
The statement transaction is part of the **Titanium** release.
{% endhint %}

### JSON

```javascript
{
  "type": 23,
  "version": 3,
  "id": "1uZqDjRjaehEceSxrVxz6WD6wt8su8TBHyDLQ1KFnJo",
  "sender": "3NBcx7AQqDopBj3WfwCVARNYuZyt1L9xEVM",
  "senderKeyType": "Ed25519",
  "senderPublicKey": "7gghhSwKRvshZwwh6sG97mzo1qoFtHEQK7iM4vGcnEt7",
  "recipient": "3N9ChkxWXqgdWLLErWFrSwjqARB6NtYsvZh",
  "statementType": 1,
  "subject": "3yMApqCuCjXDWPrbjfR5mjCPTHqFG8Pux1TxQrEM35jj",
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
* `recipient` and`subject` are optional.
* Binary strings are base58 encoded.
* `timestamp` and `expires` is in microseconds since epoch.
* `fee` includes 8 digits, so `LTO * 10^8`
{% endhint %}

{% hint style="warning" %}
Associations that are recently expired may still be returned by the node. The public node will use the time of the last mined block to determine if an association is expired or not.
{% endhint %}

### Association identifier

An association is identified by the sender, recipient, association type, and subject. With this information, the association can be [revoked](revoke-association.md).

Submitting a new association transaction with the same sender, recipient, association type, and subject will overwrite the expiry date and data.

Since the subject is part of the identifier, it's not possible to change the subject of an existing association.

### Data entries

Since version 4, association transactions can have data entries. These are similar to those of the [data transaction](data.md).

### Binary schema

The binary data structure of the unsigned transaction.

{% tabs %}
{% tab title="V3" %}
<table data-full-width="true"><thead><tr><th width="70">#</th><th width="240" align="center">Field Name</th><th width="285" align="center">Type</th><th>Length</th></tr></thead><tbody><tr><td>1</td><td align="center">Transaction type</td><td align="center">Byte (constant, value=16)</td><td>1</td></tr><tr><td>2</td><td align="center">Version</td><td align="center">Byte (constant, value=3)</td><td>1</td></tr><tr><td>3</td><td align="center">Network id</td><td align="center">Byte</td><td>1</td></tr><tr><td>4</td><td align="center">Timestamp</td><td align="center">Long</td><td>8</td></tr><tr><td>5</td><td align="center">Sender's key type</td><td align="center">KeyType (Byte)</td><td>1</td></tr><tr><td>6</td><td align="center">Sender's public key</td><td align="center">PublicKey (Array[Byte])</td><td>32 | 33</td></tr><tr><td>7</td><td align="center">Fee</td><td align="center">Long</td><td>8</td></tr><tr><td>8</td><td align="center">Recipient</td><td align="center">Address (Array[Byte])</td><td>26</td></tr><tr><td>9</td><td align="center">Association type</td><td align="center">Int</td><td>4</td></tr><tr><td>10</td><td align="center">Expires</td><td align="center">Long</td><td>8</td></tr><tr><td>11</td><td align="center">Subject length (N)</td><td align="center">Short</td><td>2</td></tr><tr><td>12</td><td align="center">Subject</td><td align="center">Array[Byte]</td><td>N</td></tr><tr><td>13</td><td align="center">Number of entries</td><td align="center">Short</td><td>2</td></tr><tr><td>14</td><td align="center">Entry 1</td><td align="center"></td><td></td></tr><tr><td>...</td><td align="center"></td><td align="center"></td><td></td></tr></tbody></table>

{% hint style="info" %}
* Network id can be obtained by taking the 2nd byte from the sender address.
* Each [key type](../../accounts/#key-types) has a numeric id in addition to the reference from the JSON.
* Integers (short, int, long) have a big endian byte order.
{% endhint %}
{% endtab %}
{% endtabs %}

The encoding of a data entry depends on the type. For more information see ["Binary schema of data entries"](data.md#binary-schema-of-data-entries).
