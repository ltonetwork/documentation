# Event

## Event schema

`https://specs.livecontracts.io/v0.2.0/event/schema.json#`

### body

The event body is the information about the event that's specific to this event type. Information that can be calculated or projected should be omitted from the event.

The body is a base58 encoded JSON string of a [Resource](../resource.md).

_Having the body as structured data, could lead to a mismatch of the hash or signature due to differences in JSON encoders. Therefor the body is always a base58 encoded string._

### timestamp

The is the date and time, that the event occurred according to the signer, as seconds sinds UNIX Epoch.

### previous

The base58 encoded SHA256 hash of the previous event. This is the way events are chained.

The first event of the chain has a hash of the event chain id as `previous`.

### signkey

The signer's base58 encoded ED25519 public key. This identifies the signer and MUST be used to verify the signature.

### signature

A base58 encoded ED25519 signature of the event. To create the signature, first create the message that needs to be signed. Each line is separated by a single `\n` character.

```text
<body>
<timestamp>
<previous>
<signkey>
```

### hash

A base58 encoded SHA256 hash of the event. To create the hash use the same message as was created to sign the event.

### original

The properties of the original event in case the event was rebased to resolve a conflict. This object has all the properties except for `body` and `original` , so

* `timestamp`
* `previous`
* `signkey`
* `signature`
* `hash`



