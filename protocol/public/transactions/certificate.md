---
description: On-chain X.509 certificates
---

# Certificate

{% hint style="success" %}
The certificate transaction is part of the **Palladium** release.
{% endhint %}

A **Certificate** transaction allows an identity on the LTO Network to anchor an X.509 certificate on-chain, typically an **organizational** or **Extended Validation (EV)** certificate. This enables trusted identification of legal entities, such as companies, within decentralized workflows.

By registering the certificate on-chain, the organization’s identity can be cryptographically verified and linked to its LTO account. This serves as a foundation for verifiable credentials, cross-chain identity proofs, and secure interactions in regulated environments.

{% hint style="info" %}
The Subject Public Key Info (SPKI) in the X.509 certificate must match the sender’s public key and key type in the transaction.

If they differ, the transaction is considered invalid and will be rejected by the network.
{% endhint %}

### JSON

```json
{
  "type": 24,
  "version": 3,
  "id": "2GqzNqFvxRQh7DQENecUj6kQMsA6JPXjvS7p1yx7PePx",
  "sender": "3Jq8mnhRquuXCiFUwTLZFVSzmQt3Fu6F7HQ",
  "senderKeyType": "secp256r1",
  "senderPublicKey": "AJQn2L4EhJhQh2NX5NvyDDB5BUPuiZBiNRmqRcSmj3g7",
  "fee": 500000000,
  "timestamp": 1326499200000,
  "certificate": "-----BEGIN CERTIFICATE-----\nMIIBmjCCAUGgAwIBAgIUBTg9WprxEdpxu8cLV2CKyGJ7bVQwCgYIKoZIzj0EAwIw\nIzEhMB8GA1UEAwwYQWxpY2UsTz1FeGFtcGxlIEx0ZCxDPU5MMB4XDTI1MDYwMjEy\nNTYzMloXDTI2MDYwMjEyNTYzMlowIzEhMB8GA1UEAwwYQWxpY2UsTz1FeGFtcGxl\nIEx0ZCxDPU5MMFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAE2/WATtZvChbb3xrQ\nEXzszXz3IgpUyA7jbLVQ9B2ibL/SZtvhjU84S8fI1HhzyE5eAqKvkh/pdArBjyXL\naqw0Q6NTMFEwHQYDVR0OBBYEFEb3OV2UesAgnXz8VOieyXgEilyHMB8GA1UdIwQY\nMBaAFEb3OV2UesAgnXz8VOieyXgEilyHMA8GA1UdEwEB/wQFMAMBAf8wCgYIKoZI\nzj0EAwIDRwAwRAIgVo0OBEFkXDgJGuIrOl15UKdkvrhe0THS8MO64Jw2F7cCIBpC\nNLnbu23KWkzoIdACHRTGc3MqZrWh53lGq/+tK13P\n-----END CERTIFICATE-----",
  "proofs": [
    "2omugkAQdrm9P7YPx6WZbXMBTifRS6ptaTT8rPRRvKFr1EPFafHSosq6HzkuuLv78gR6vaXLA9WtMsTSBgi3H1qe"
  ],
  "height": 1070000
}
```

{% hint style="info" %}
* `id` and `height` should be omitted when broadcasting. These fields are set by the node.
* `recipient` and`subject` are optional.
* Binary strings are base58 encoded.
* `timestamp` is in microseconds since epoch.
* `fee` includes 8 digits, so `LTO * 10^8`
* `certificate` is PEM encoded
* `certificate` can be null to clear the certificate of the address
{% endhint %}

### Fields

{% tabs %}
{% tab title="V3" %}
<table data-full-width="true"><thead><tr><th width="70">#</th><th width="240" align="center">Field Name</th><th width="285" align="center">Type</th><th>Length</th></tr></thead><tbody><tr><td>1</td><td align="center">Transaction type</td><td align="center">Byte (constant, value=16)</td><td>1</td></tr><tr><td>2</td><td align="center">Version</td><td align="center">Byte (constant, value=3)</td><td>1</td></tr><tr><td>3</td><td align="center">Network id</td><td align="center">Byte</td><td>1</td></tr><tr><td>4</td><td align="center">Timestamp</td><td align="center">Long</td><td>8</td></tr><tr><td>5</td><td align="center">Sender's key type</td><td align="center">KeyType (Byte)</td><td>1</td></tr><tr><td>6</td><td align="center">Sender's public key</td><td align="center">PublicKey (Array[Byte])</td><td>32 | 33</td></tr><tr><td>7</td><td align="center">Fee</td><td align="center">Long</td><td>8</td></tr><tr><td>8</td><td align="center">Certificate length (N)</td><td align="center">Short</td><td>1</td></tr><tr><td>9</td><td align="center">Certificate</td><td align="center">Array[Byte]</td><td>N</td></tr></tbody></table>

{% hint style="info" %}
* Network id can be obtained by taking the 2nd byte from the sender address.
* Each [key type](../../accounts/#key-types) has a numeric id in addition to the reference from the JSON.
* Integers (short, int, long) have a big endian byte order.
* Certificate is DER encoded
{% endhint %}
{% endtab %}
{% endtabs %}
