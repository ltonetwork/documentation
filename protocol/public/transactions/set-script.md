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
	"senderKeyType": 1,
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

