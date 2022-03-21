---
description: Register an account by public key
---

# Register

{% hint style="info" %}
Registering an account isn't needed to use LTO You can simply sign and broadcast transactions. The register transaction publishes a public key, so a new account can be used for a DID verification method.
{% endhint %}

### JSON

```javascript
{
  "type": 20,
  "version": 3,
  "id": "8M6dgn85eh3bsHrVhWng8FNaHBcHEJD4MPZ5ZzCciyon",
  "sender": "3Jq8mnhRquuXCiFUwTLZFVSzmQt3Fu6F7HQ",
  "senderKeyType": "ed25519",
  "senderPublicKey": "AJVNfYjTvDD2GWKPejHbKPLxdvwXjAnhJzo6KCv17nne",
  "fee": 35000000,
  "timestamp": 1647867270043,
  "accounts": [
    {
      "keyType": "ed25519",
      "publicKey": "8cMyCW5Esx98zBqQCy9N36UaGZuNcuJhVe17DuG42dHS"
    },
    ...
  ],
  "proofs": [
    "3xB85BVKRooXtYfz1VnJcU6rWfgnPbdwCyB3RBdFyPHKzpazeSbk7BGsP233LTSf8wojxfhymCdHc9oBQ92DhvoS"
  ]
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

{% tabs %}
{% tab title="V3 (current)" %}
| #   |      Field Name      |            Type           | Length   |
| --- | :------------------: | :-----------------------: | -------- |
| 1   |   Transaction type   | Byte (constant, value=20) | 1        |
| 2   |        Version       |  Byte (constant, value=3) | 1        |
| 3   |       Chain id       |            Byte           | 1        |
| 4   |       Timestamp      |            Long           | 8        |
| 5   |   Sender's key type  |       KeyType (Byte)      | 1        |
| 6   |  Sender's public key |  PublicKey (Array\[Byte]) | 32 \| 33 |
| 7   |          Fee         |            Long           | 8        |
| 8   |  Number of accounts  |           Short           | 2        |
| 9   |  Account 1 key type  |       KeyType (Byte)      | 1        |
| 10  | Account 1 public key |  PublicKey (Array\[Byte]) | 32 \| 33 |
| ... |                      |                           |          |



{% hint style="info" %}
* Account key type and public key is repeated for each account.
* Chain id can be obtained by taking the 2nd byte from the sender address.
* Each [key type](../../accounts.md#key-types) has a numeric id in addition to the reference from the JSON.
* Integers (short, int, long) have a big endian byte order.
{% endhint %}
{% endtab %}
{% endtabs %}
