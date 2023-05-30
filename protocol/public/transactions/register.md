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
<table><thead><tr><th width="150">#</th><th align="center">Field Name</th><th align="center">Type</th><th>Length</th></tr></thead><tbody><tr><td>1</td><td align="center">Transaction type</td><td align="center">Byte (constant, value=20)</td><td>1</td></tr><tr><td>2</td><td align="center">Version</td><td align="center">Byte (constant, value=3)</td><td>1</td></tr><tr><td>3</td><td align="center">Chain id</td><td align="center">Byte</td><td>1</td></tr><tr><td>4</td><td align="center">Timestamp</td><td align="center">Long</td><td>8</td></tr><tr><td>5</td><td align="center">Sender's key type</td><td align="center">KeyType (Byte)</td><td>1</td></tr><tr><td>6</td><td align="center">Sender's public key</td><td align="center">PublicKey (Array[Byte])</td><td>32 | 33</td></tr><tr><td>7</td><td align="center">Fee</td><td align="center">Long</td><td>8</td></tr><tr><td>8</td><td align="center">Number of accounts</td><td align="center">Short</td><td>2</td></tr><tr><td>9</td><td align="center">Account 1 key type</td><td align="center">KeyType (Byte)</td><td>1</td></tr><tr><td>10</td><td align="center">Account 1 public key</td><td align="center">PublicKey (Array[Byte])</td><td>32 | 33</td></tr><tr><td>...</td><td align="center"></td><td align="center"></td><td></td></tr></tbody></table>



{% hint style="info" %}
* Account key type and public key is repeated for each account.
* Chain id can be obtained by taking the 2nd byte from the sender address.
* Each [key type](../../accounts.md#key-types) has a numeric id in addition to the reference from the JSON.
* Integers (short, int, long) have a big endian byte order.
{% endhint %}
{% endtab %}
{% endtabs %}
