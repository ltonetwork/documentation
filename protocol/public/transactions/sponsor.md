---
description: 'Sponsor an account, offering to pay for all transaction fees for that account.'
---

# Sponsor

{% hint style="danger" %}
You should only sponsor an account you trust, and/or have a legally binding agreement with. A sponsored account holder can easily drain your account through spam transactions. If the account holder is running a node, he/she can claim part of the spend tokens as mining reward. **Limit the amount of tokens on the sponsoring account**, adding funds when necessary.
{% endhint %}

{% hint style="success" %}
See [transaction fees with sponsored accounts](./#transaction-fees) for more info.
{% endhint %}

### JSON

```javascript
{
  "type": 18,
  "version": 3,
  "id": "HtxiY9x8aVBDfPvEUifYZuBEDge5TCDDAtqRGBW8HDef",
  "sender": "3NBcx7AQqDopBj3WfwCVARNYuZyt1L9xEVM",
  "senderKeyType": 1,
  "senderPublicKey": "7gghhSwKRvshZwwh6sG97mzo1qoFtHEQK7iM4vGcnEt7",
  "recipient": "3N9ChkxWXqgdWLLErWFrSwjqARB6NtYsvZh",
  "timestamp": 1610410901000,
  "fee": 500000000,
  "proofs": [
    "QKef6R8LrMBupBF9Ry8zjFTu3mexC55J6XNofDDQEcJnZJsRjZPnAk6Yn2eiHkqqd2uSjB2r58fC8QVLaVegQEz"
  ],
  "height": 1225821
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

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction type | Byte \(constant, value=18\) | 1 |
| 2 | Version | Byte \(constant, value=3\) | 1 |
| 3 | Timestamp | Long | 8 |
| 4 | Sender's key type | KeyType \(Byte\) | 1 |
| 5 | Sender's public key | PublicKey \(Array\[Byte\]\) | 32 \| 33 |
| 6 | Fee | Long | 8 |
| 7 | Recipient | Address \(Array\[Byte\]\) | 26 |

{% hint style="info" %}
Integers \(short, int, long\) have a big endian byte order.
{% endhint %}

