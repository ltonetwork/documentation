---
description: Transaction to create a smart account
---

# Set Script

Smart accounts have a custom script that defines how transactions should be validated. The script needs to be compiled by the node before it's broadcasted as a transaction.

Scripts are written in the [Ride programming language](https://docs.waves.tech/en/ride/).

```javascript
{
	"type": 13,
	"version": 3,
	"id": "BGUEn2TERW4nnAQuXYgJ3z6qp28ivk3kykb724wV7MZz",
	"sender": "3MtHYnCkd3oFZr21yb2vEdngcSGXvuNNCq2",
	"senderKeyType": "Ed25519",
	"senderPublicKey": "4EcSxUkMxqxBEBUBL2oKz3ARVsbyRJTivWpNrYQGdguz",
	"script": "base64:AQkAAfQAAAADCAUAAAACdHgAAAAJYm9keUJ5dGVzCQABkQAAAAIIBQAAAAJ0eAAAAAZwcm9vZnMAAAAAAAAAAAAIBQAAAAJ0eAAAAA9zZW5kZXJQdWJsaWNLZXmmsz2x",
	"timestamp": 1519862400,
	"fee": 500000000,
	"proofs": [
	  "jxW9T2iUSQ68yv41Wj8JKb8HykwzKzbuHLBG6eySLaXk45rNbDo3zr2AS9bGMggrBZUUJQTFjKHeiD1q69pPUxY"
	],
	"height": 1248629
}
```

### Examples

#### Restrict account

```javascript
match tx {
  case t:  TransferTransaction => false
  case mt: MassTransferTransaction => false
  case ss: SetScriptTransaction => false
  case _ => sigVerify(tx.bodyBytes, tx.proofs[0], tx.senderPublicKey)
}
```

The "Restrict Account" script disables transfers from the account, it also disables modifying the script. This means that any funds on the account can only be used for staking/leasing and paying transaction fees.

### Binary schema

The binary data structure of the unsigned transaction.

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction type | Byte \(constant, value=4\) | 1 |
| 2 | Version | Byte \(constant, value=3\) | 1 |
| 3 | Chain id | Byte | 1 |
| 4 | Timestamp | Long | 8 |
| 5 | Sender's key type | KeyType \(Byte\) | 1 |
| 6 | Sender's public key | PublicKey \(Array\[Byte\]\) | 32 \| 33 |
| 7 | Fee | Long | 8 |
| 8 | Script length \(N\) | Short | 2 |
| 9 | Script | Array\[Byte\] | N |

{% hint style="info" %}
* `script` is binary \(without "base64:" prefix\).
* Chain id can be obtained by taking the 2nd byte from the sender address.
* Each [key type](../../accounts.md#key-types) has a numeric id in addition to the reference from the JSON.
* Integers \(short, int, long\) have a big endian byte order.
{% endhint %}

