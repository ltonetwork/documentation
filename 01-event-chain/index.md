[← back](../)

# Event chain

The event chain is a miniature blockchain that is shared between parties involved in a contract or process. Each event
is signed and that added referencing the previous event, forming a chain. All information of the system is derived from
event chains.

## Schemas

[JSON Schema](schema.json) - http://specs.livecontracts.io/draft-01/01-event-chain/schema.json#

* [Event chain](#event-chain-schema)
* [Event](#event-schema)
* [Receipt](#receipt-schema)

## Event chain schema

`http://specs.livecontracts.io/draft-01/01-event-chain/schema.json#`

The event chain is the only mutable component of Live Contacts in the fact that events may be added. Event chains
SHOULD NOT be versioned.

### $schema

The Live Contracts Event chain [JSON schema](http://json-schema.org) URI that describes the JSON structure of the event
chain. To point to this version of the specification use
`"$schema": "http://specs.livecontracts.io/draft-01/01-event-chain/schema.json#"`.

### id

A globally unique identifier for the event chain.

The event chain id MUST be calculated from public key used to sign the genesis event of the chain. The `id` is a
base58 encoded value from the following data structure:

| # | Field name | Type | Position | Length |
| ---: | :--- | :---: | ---: | ---: |
| 1 | Version (0x01) | Byte | 0 | 1 |
| 2 | Random number | Long | 1 | 8 |
| 3 | Public key hash | Bytes | 2 | 20 |
| 4 | Checksum | Bytes | 22 | 4 |

Public key hash is first 20 bytes of _SecureHash_ of public key bytes.
Checksum is first 4 bytes of _SecureHash_ of version, random and hash bytes.
SecureHash is hash function `Keccak256(Blake2b256(data))`.

_This structure is similar to
[data structures of the Waves Platform](https://github.com/wavesplatform/Waves/wiki/Data-Structures)._

### events

The array of events.

## Event schema

`http://specs.livecontracts.io/draft-01/01-event-chain/schema.json#event`

### body

The event body is the information about the event that's specific to this event type. Information that can be calculated
or projected should be omitted from the event.

The body is a base58 encoded JSON string. Having this as structured data, could lead to a mismatch of the hash or
signature due to differences in JSON encoders.

The body MUST contain a `$schema` property. This is used to determine how to handle the event and if the identity is
authorized to add the event.

Typically the body also contains an `id` property as [LTRI](../00-ltri/). In that case a version number will
automatically be appended. This version number is the first 8 characters of the base58 encoded SHA256 hash of the
event `body` string.

### timestamp

The is the date and time, in ISO 8601 format, that the event occurred according to the signer.

### previous

The base58 encoded SHA256 hash of the previous event. This is the way events are chained.

The first event of the chain has a hash of the event chain id as `previous`.

### signkey

The signer's X25519 public key, base58 encoded. This identifies the signer and MUST be used to verify the signature.

### signature

A base58 encoded `ED25519` signature of the event. To create the signature, first create the message that needs to be
signed. Each line is separated by a single `\n` character.

```
<body>
<timestamp>
<previous>
<signkey>
```

### hash

A base58 encoded SHA256 hash of the event. To create the hash use the same message as was created to sign the event.

### receipt

A receipt for anchoring the event to a global (public) blockchain. With anchoring the hash is written as attachment of a
transaction. This is done as proof of existence and for timestamping.

## Receipt schema

`http://specs.livecontracts.io/draft-01/01-event-chain/schema.json#receipt`

The receipt follows the [Chainpoint specification v2](https://chainpoint.org/).

_The formatting differs from other structures in the Live Contracts specs, because this is a 3th party specification._

### @context

The [JSON-LD](https://json-ld.org/) context for the receipt. This MUST be `https://w3id.org/chainpoint/v2`.

### type

Receipt type definition specifying hash method and version. This MUST be `ChainpointSHA256v2`.

### targetHash

The hash value being anchored to the blockchain. This SHOULD be the event hash.

### merkleRoot

Merkle tree root value that is anchored to the blockchain. In case multiple hashes are combined in a merkle tree, the
merkle root is required to verify that a hash is part of the tree.

### proof

Merkle proof establishing link from the `targetHash` to the `merkleRoot`. In a merkle tree, the proof is required to
verify that a hash is part of the tree.

### anchors

Array with anchors. Each anchor is a transaction in on a blockchain. An anchor is an object with a `type` field which
determines the network / transaction type and a `sourceId` field that contains the transaction id.

Supported anchor types are

* `BTCOpReturn` - Anchored to a Bitcoin transaction within an OP_RETURN output.
* `ETHData` - Anchored to an Ethereum transaction within the data field.
* `WAVESData` - Anchored to an Waves data transaction.
