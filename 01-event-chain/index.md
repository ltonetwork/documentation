# Live Contracts - Event Chain

The event chain is a miniature blockchain that is shared between parties involved in a contract or process. Each event
is signed and that added referencing the previous event, forming a chain. All information of the system is derived from
event chains.

## Schemas

* [Event chain](#event-chain-schema)
* [Event](#event-schema)
* [Receipt](#receipt-schema)

## Event chain schema

[JSON Schema](http://schema.livecontracts.io/event-chain/schema.json#)

### $schema

The Live Contracts Event Chain [JSON schema](http://json-schema.org) URI that describes the JSON structure of the event
chain. To point to this version of the specification use `"$schema": "http://specs.livecontracts.io/draft-01/01-event-chain/schema.json#"`.

### id

A URI as a globally unique identifier for the event chain. This is typically an [LTRI](http://specs.livecontracts.io/draft-01/00-ltri/)
using a random UUID-4; `lt:/event-chains/<uuid-4>`.

The event chain is the only mutable component of Live Contacts in the fact that events may be added. Event chains
SHOULD NOT be versioned.

### events

The array of events.

## Event schema

[JSON Schema](http://schema.livecontracts.io/event-chain/schema.json#event)

### $schema

The event schema is an URL to a JSON schema that defines the type of event. This is typically one of schema's in the
live contracts specifications, but you MAY add custom event types using custom schemas.

### id

The id uri for an event that applies to a specific projection like a contract or a process.

### previous

The base58 encoded SHA256 hash of the previous event. This is the way events are chained.

The first event of the chain has a hash of the event chain id as `previous`.

### body

The event body is the information about the event that's specific to this event type. Information that can be calculated
or projected should be omitted from the event. The body should also not contain an id, if the event id is used.

The body is a base64 encoded JSON string. Having this as structured data, could lead to a mismatch of the hash or
signature due to differences in JSON encoders.

### signkey

The signer's public key, base58 encoded. This identifies the signer and MUST be used to verify the signature.

### signature

A base58 encoded `Curve25519` signature of the event. To create the signature, first create the message that needs to be
signed. Each line is separated by a single `\n` character.

```
<schema>
<id>
<body>
<previous>
<signkey>
```

### hash

A base58 encoded SHA256 hash of the event. To create the hash use the same message as was created to sign the event.

### receipt

A receipt for anchoring the event to a global blockchain. With anchoring the hash is written as attachment of a
transaction. This is done as proof of existence and for timestamping.

## Receipt schema

[JSON Schema](http://schema.livecontracts.io/event-chain/schema.json#receipt)

### system

The blockchain or system that was used for anchoring.

### transaction

The transaction id where the event hash can be found.

### root

In case multiple hashes are combined in a merkle tree for anchoring, the merkle root is required to verify that the
hash.
