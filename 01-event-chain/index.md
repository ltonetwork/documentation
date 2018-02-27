# Live Contracts - Event Chain

### Abstract

The event chain is a miniature blockchain that is shared between parties involved in a contract or process. Each event
is signed and that added referencing the previous event, forming a chain. All information of the system is derived from
event chains.

### Note to Readers

The issues list for this draft can be found at <https://github.com/legalthings/livecontracts-specs/issues>.
For additional information, see <http://livecontracts.io/>.

To provide feedback, use this issue tracker, the communication methods listed on the homepage, or email the document
editors.

### Copyright Notice

Copyright (c) 2017 LegalThings One. All rights reserved.

You may use this specification under the [Creative Commons Attribution 4.0 International Public License](https://raw.githubusercontent.com/legalthings/livecontracts-specifications/master/LICENSE).

## Event chain

### $schema

The Live Contracts Event Chain [JSON schema](http://json-schema.org) URI that describes the JSON structure of the event
chain. To point to this version of the specification use `"$schema": "http://specs.livecontracts.io/draft-01/01-event-chain/schema.json#"`.

### id

A URI as a unique identifier for the event chain. This is typically an [LTRI](http://specs.livecontracts.io/draft-01/00-ltri/)
using a random UUID-4; `ltri:/event-chains/<uuid-4>`.

The event chain is the only mutable component of Live Contacts in the fact that events may be added. Event chains
SHOULD NOT be versioned.

### events

The array of events.

## Event

### $schema

The event schema is an URL to a JSON schema that defines the type of event. This is typically one of schema's in the
live contracts specifications, but you MAY add custom event types using custom schemas.

### previous

The base58 encoded SHA256 hash of the previous event. This is the way events are chained.

The first event of the chain has a hash of the event chain id as `previous`.

### body

The event body is the information about the event that's specific to this event type. Information that can be calculated
or projected should be omitted from the event.

The body is a base64 encoded JSON string. Having this as structured data, could lead to a mismatch of the hash or
signature due to differences in JSON encoders.

### signkey

The signer's public key, base58 encoded. This identifies the signer and MUST be used to verify the signature.

### signature

A base58 encoded `Curve25519` signature of the event. To create the signature, first create the message that needs to be
signed. Each line is separated by a single `\n` character.

```
<schema>
<body>
<previous>
<signkey>
```

### hash

A base58 encoded SHA256 hash of the event. To create the hash use the same message as was created to sign the event.

### receipt

A receipt for anchoring the event to a global blockchain. With anchoring the hash is written as attachment of a
transaction. This is done as proof of existence and for timestamping.

## Receipt

### system

The blockchain or system that was used for anchoring.

### transaction

The transaction id where the event hash can be found.

### root

In case multiple hashes are combined in a merkle tree for anchoring, the merkle root is required to verify that the
hash.
