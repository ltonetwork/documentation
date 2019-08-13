---
description: >-
  The event chain is a miniature blockchain that is shared between parties
  involved in a process.
---

# Event Chain

Each event is signed and that added referencing the previous event, forming a chain. All information of the system is derived from event chains.

## Event chain schema

`https://specs.livecontracts.io/v0.2.0/event-chain/schema.json#`

The event chain is the only mutable component of Live Contacts in the fact that events may be added. Event chains SHOULD NOT be versioned.

### $schema

The Live Contracts Event chain [JSON schema](http://json-schema.org) URI that describes the JSON structure of the event chain.

### id

A globally unique identifier for the event chain.

The event chain id MUST be calculated from public key used to sign the genesis event of the chain. The `id` is a base58 encoded value from the following data structure:

| \# | Field name | Type | Position | Length |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Type \(0x40\) | Byte | 0 | 1 |
| 2 | Random number | Long | 1 | 20 |
| 3 | Public key hash | Bytes | 21 | 20 |
| 4 | Checksum | Bytes | 41 | 4 |

Public key hash is first 20 bytes of _SecureHash_ of public key bytes. Checksum is first 4 bytes of _SecureHash_ of version, random and hash bytes. SecureHash is hash function `sha256(Blake2b(data))`. The random number can be random number or generated from a nonce. This nonce might be the first 20 bytes of a `sha256` hash of a string.

### events

The array of events.

[Read more](event.md)

### identities

A projected set of identities that participate on the chain.

