# Event

An event on the event chain can be compared to a transaction on a blockchain. An event has the following structure

<table><thead><tr><th width="183">Field</th><th></th></tr></thead><tbody><tr><td><code>mediaType</code></td><td>MIME type of the data</td></tr><tr><td><code>data</code></td><td>Binary</td></tr><tr><td><code>previous</code></td><td>The hash of the previous event</td></tr><tr><td><code>timestamp</code></td><td>Unix timestamp in milliseconds. Time of when the event was signed</td></tr><tr><td><code>signKey</code></td><td>Public key with key type</td></tr><tr><td><code>signature</code></td><td>Signature of the binary representation of the event</td></tr><tr><td><code>hash</code></td><td>Hash of the binary representation of the event</td></tr></tbody></table>

### JSON

```json
{
  "timestamp": 1519883600,
  "previous": "BRFnaH3UFnABQ1gV1SvT9PLo5ZMFzH7NhqDSgyn1z8wD",
  "signKey": {
    "keyType": "ed25519",
    "publicKey": "2KduZAmAKuXEL463udjCQkVfwJkBQhpciUC4gNiayjSJ"
  },
  "signature": "2hqLhbmh2eX2WhAgbwHhBZqzdpFcjWBYYN5WBj8zcYVKzVbnVH7mESCC9c9acihxWFwfvufnFYxxgFMgJPbpbU4N",
  "hash": "9Y9DhjXHdrsUE93TZzSAYBWZS5TDWWNKKh2mihqRCGXh",
  "mediaType": "application/json",
  "data": "base64:eyJmb28iOiJiYXIiLCJjb2xvciI6ImdyZWVuIn0="
}
```

* `previous`, `signKey.publicKey`, `signature`, and `hash` are base58 encoded
* `data` is base64 encoded, and must be prefixed with `base64`.

### Binary schema

The binary representation is used for signing and hashing the event.

{% tabs %}
{% tab title="v2" %}
| Field                 | Type                     | Length   |
| --------------------- | ------------------------ | -------- |
| Previous              | Hash (Bytes)             | 32       |
| Key type              | KeyType (Byte)           | 1        |
| Public key            | PublicKey (Array\[Byte]) | 32 \| 33 |
| Timestamp             | Long                     | 8        |
| Media type length (N) | Short                    | 2        |
| Media type            | Array\[Byte]             | N        |
| Data length (M)       | Short                    | 2        |
| Data                  | Array\[Byte]             | M        |
{% endtab %}

{% tab title="v1 (deprecated)" %}
| Field      | Type                     | Length   |
| ---------- | ------------------------ | -------- |
| Previous   | Hash (Bytes)             | 32       |
| Key type   | KeyType (Byte)           | 1        |
| Public key | PublicKey (Array\[Byte]) | 32 \| 33 |
| Timestamp  | Long                     | 8        |
| Media type | Array\[Byte]             | ?        |
| Data       | Array\[Byte]             | ?        |
{% endtab %}
{% endtabs %}
