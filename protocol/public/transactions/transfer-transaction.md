---
description: >-
  The transfer transaction allows the sender to transfer LTO tokens to the
  recipient.
---

# Transfer

```javascript
{
  "type": 4,
  "version": 3,
  "id": "5a1ZVJTu8Y7mPA6BbkvGdfmbjvz9YSppQXPnb5MxihV5",
  "sender": "3N9ChkxWXqgdWLLErWFrSwjqARB6NtYsvZh",
  "senderKeyType": "Ed25519",
  "senderPublicKey": "9NFb1rvMyr1k8f3wu3UP1RaEGsozBt9gF2CmPMGGA42m",
  "fee": 100000000,
  "timestamp": 1609639213556,
  "amount": 100000000000,
  "recipient": "3NBcx7AQqDopBj3WfwCVARNYuZyt1L9xEVM",
  "attachment": "9Ajdvzr",
  "proofs": [
    "3ftQ2ArKKXw655WdHy2TK1MGXeyzKRqMQYwFidekkyxLpzFGsTziSFsbM5RCFxrn32EzisMgPWtQVQ4e5UqKUcES"
  ],
  "height": 1212761
}
```

{% hint style="info" %}
* `id` and `height` should be omitted when broadcasting. These fields are set by the node.
* Binary strings (including `attachment`) are base58 encoded.
* `timestamp` is in microseconds since epoch.
* `fee` and `amount` include 8 digits, so `LTO * 10^8`
{% endhint %}

### Binary schema

The binary data structure of the unsigned transaction.

{% tabs %}
{% tab title="V3 (current)" %}
<table><thead><tr><th width="150" data-type="number">#</th><th align="center">Field Name</th><th align="center">Type</th><th>Length</th></tr></thead><tbody><tr><td>1</td><td align="center">Transaction type</td><td align="center">Byte (constant, value=4)</td><td>1</td></tr><tr><td>2</td><td align="center">Version</td><td align="center">Byte (constant, value=3)</td><td>1</td></tr><tr><td>3</td><td align="center">Chain id</td><td align="center">Byte</td><td>1</td></tr><tr><td>4</td><td align="center">Timestamp</td><td align="center">Long</td><td>8</td></tr><tr><td>5</td><td align="center">Sender's key type</td><td align="center">KeyType (Byte)</td><td>1</td></tr><tr><td>6</td><td align="center">Sender's public key</td><td align="center">PublicKey (Array[Byte])</td><td>32 | 33</td></tr><tr><td>7</td><td align="center">Fee</td><td align="center">Long</td><td>8</td></tr><tr><td>8</td><td align="center">Recipient</td><td align="center">Address (Array[Byte])</td><td>26</td></tr><tr><td>9</td><td align="center">Amount</td><td align="center">Long</td><td>8</td></tr><tr><td>10</td><td align="center">Attachment length (N)</td><td align="center">Short</td><td>2</td></tr><tr><td>11</td><td align="center">Attachment</td><td align="center">Array[Byte]</td><td>N</td></tr></tbody></table>

{% hint style="info" %}
* Chain id can be obtained by taking the 2nd byte from the sender address.
* Each [key type](../../accounts/#key-types) has a numeric id in addition to the reference from the JSON.
* Integers (short, int, long) have a big endian byte order.
{% endhint %}
{% endtab %}

{% tab title="V2" %}
<table><thead><tr><th width="150">#</th><th align="center">Field Name</th><th align="center">Type</th><th>Length</th></tr></thead><tbody><tr><td>1</td><td align="center">Transaction type</td><td align="center">Byte (constant, value=4)</td><td>1</td></tr><tr><td>2</td><td align="center">Version</td><td align="center">Byte (constant, value=2)</td><td>1</td></tr><tr><td>3</td><td align="center">Sender's public key</td><td align="center">PublicKey (Array[Byte])</td><td>32</td></tr><tr><td>4</td><td align="center">Timestamp</td><td align="center">Long</td><td>8</td></tr><tr><td>5</td><td align="center">Amount</td><td align="center">Long</td><td>8</td></tr><tr><td>6</td><td align="center">Fee</td><td align="center">Long</td><td>8</td></tr><tr><td>7</td><td align="center">Recipient</td><td align="center">Address (Array[Byte])</td><td>26</td></tr><tr><td>8</td><td align="center">Attachment length (N)</td><td align="center">Short</td><td>2</td></tr><tr><td>9</td><td align="center">Attachment</td><td align="center">Array[Byte]</td><td>N</td></tr></tbody></table>

{% hint style="info" %}
Integers (short, int, long) have a big endian byte order.
{% endhint %}
{% endtab %}

{% tab title="V1" %}
<table><thead><tr><th width="150">#</th><th align="center">Field Name</th><th align="center">Type</th><th>Length</th></tr></thead><tbody><tr><td>1</td><td align="center">Transaction type</td><td align="center">Byte (constant, value=4)</td><td>1</td></tr><tr><td>2</td><td align="center">Sender's public key</td><td align="center">PublicKey (Array[Byte])</td><td>32</td></tr><tr><td>3</td><td align="center">Timestamp</td><td align="center">Long</td><td>8</td></tr><tr><td>4</td><td align="center">Amount</td><td align="center">Long</td><td>8</td></tr><tr><td>5</td><td align="center">Fee</td><td align="center">Long</td><td>8</td></tr><tr><td>6</td><td align="center">Recipient</td><td align="center">Address (Array[Byte])</td><td>26</td></tr><tr><td>7</td><td align="center">Attachment length (N)</td><td align="center">Short</td><td>2</td></tr><tr><td>8</td><td align="center">Attachment</td><td align="center">Array[Byte]</td><td>N</td></tr></tbody></table>

{% hint style="info" %}
Integers (short, int, long) have a big endian byte order.
{% endhint %}
{% endtab %}
{% endtabs %}
