# Live Contracts - Chain request

A node may request a chain or a portion of a chain. When an identity is registers, a node might request the full event
chain for that identity. If a node receives an event, but can't find the corresponding previous event, it will request
the missing events.

## Schemas

[Chain request](#chain-request-schema)
[Request body](#request-body-schema)
[Denied response](#denied-response-schema)

## Request and response

A node can request a complete chain or a portion of the chain. A chain request is discarded when processed and not
stored on the event chain. The node will validate the request and send an event chain with only the missing events. 

It needs to sign the request using a `signkey` that matches with an identity that is on the chain. If the request
doesn't validate or if the `signkey` doesn't belong to an identity, the request will be denied.

## Example

```json
{
  "$schema": "http://specs.livecontracts.io/draft-01/12-chain-request/schema.json#",
  "id": "lt:/event-chains/d29d0496-ad42-4ed7-aa37-be31a87ea93e",
  "body": "ew0KICAiaWQiOiAibHQ6L2V2ZW50LWNoYWlucy9kMjlkMDQ5Ni1hZDQyLTRlZDctYWEzNy1iZTMxYTg3ZWE5M2UiLA0KICAiZnJvbSI6IFsNCiAgICAiOVRLNGZSYnFKS1dya2g4NXl6WDlpVHpEakZVMzkyUjE0YU5pNjdDUXRlaUEiLA0KICAgICI3bktzUnNGNGN1am9ORjluODdXMnkxV0ZjNm5DQUVnbzEzbmJneHdhZVl4NSIsDQogICAgIjc3SkQ1ZWJucmtDalJSYzNobVNjMmZaZHBRNzQ2emI0Y21YQjd6a2Z2amZDIg0KICBdLA0KICAiZW5jcnlwdGtleSI6ICJMNkZzZ0h4M2VpdHlOTFZmbmZLR3NwOFBUN2tKTzRuZVJCaEg2dVEyRUFrbCINCn0=",
  "timestamp": "2018-03-04T00:00:00+00:00",
  "signkey": "8MeRTc26xZqPmQ3Q29RJBwtgtXDPwR7P9QNArymjPLVQ",
  "signature": "5zk7MGmATLsdbW1C1kiZASuxtN7graME5FBcD6Uti9TGd21i9jgzEYHF9Qaz9cYVV6ZoHg2kPAyiukTtSCW36EuZ",
  "hash": "BAf5XVyzRwB1KNg6Q38RdPaYaRQMbqcXeGoQwj9rKidH"
}
```

The body is a base64 encoded JSON string of the request

```json
{
  "from": [
    "9TK4fRbqJKWrkh85yzX9iTzDjFU392R14aNi67CQteiA",
    "7nKsRsF4cujoNF9n87W2y1WFc6nCAEgo13nbgxwaeYx5",
    "77JD5ebnrkCjRRc3hmSc2fZdpQ746zb4cmXB7zkfvjfC"
  ],
  "encryptkey": "L6FsgHx3eityNLVfnfKGsp8PT7kJO4neRBhH6uQ2EAkl"
}
```

The request can't be validated it's denied.

```json
  "$schema": "http://specs.livecontracts.io/draft-01/12-chain-request/schema.json#denied-response",
  "id": "lt:/event-chains/d29d0496-ad42-4ed7-aa37-be31a87ea93e",
  "body": "",
  "timestamp": "",
  "previous": "BAf5XVyzRwB1KNg6Q38RdPaYaRQMbqcXeGoQwj9rKidH",
  "signkey": "",
  "signature": "",
  "hash": ""
```

## Chain request schema

[JSON Schema](http://specs.livecontracts.io/01-draft/12-chain-request/schema.json#)

### $schema

The Live Contracts Chain request [JSON schema](http://json-schema.org) URI that describes the JSON structure of the
chain request. To point to this version of the specification use
`"$schema": "http://specs.livecontracts.io/draft-01/12-chain-request/schema.json#"`.

### id

The globally unique identifier of the event chain you're requesting. This is typically an
[LTRI](http://specs.livecontracts.io/draft-01/00-ltri/) with an UUID-4; `lt:/event-chains/<uuid-4>`.

### body

The body is a base64 encoded JSON string of the [request body](#request-body-schema).

### timestamp

The is the date and time, in ISO 8601 format, of the request occurred according to the signer.

### signkey

The signer's public key, base58 encoded. This identifies the signer and MUST be used to verify the signature.

### signature

A base58 encoded `Curve25519` signature of the event. To create the signature, first create the message that needs to be
signed. Each line is separated by a single `\n` character.

```
<schema>
<id>
<body>
<timestamp>
<signkey>
```

_This is similar to the message of an event, however the chain request doesn't have a `previous` field._

### hash

A base58 encoded SHA256 hash of the event. To create the hash use the same message as was created to sign the request.

## Request body schema

### from

A hash or an array of hashes that function as starting point of the events the nodes need to receive. To receive the
complete chain `from` must be the base58 encoded SHA256 hash of the event id.

A node can't match a received event, because the hash in `previous` doesn't match any events in the event chain it
holds. In that case `from` SHOULD be an array with the last event an identity of the other node has signed up to the
last of event in the chain. The last event of the other node has signed is the latest point to where you can be sure the
chains are identical and didn't branch.

If a node finds out a the chain was branched it needs to handle it through
[conflict resolution](http://specs.livecontracts.io/draft-01/13-conflict-resolution/).

### encryptkey

The public key for encrypting the response data. The private decrypt key is hold and kept secret by the node which is
doing the request.

If `encryptkey` isn't specified, the identity's encryptkey is used to encrypt the response. Nodes SHOULD NOT send an
unencrypted event chain as response.

## Denied response schema

[JSON Schema](http://specs.livecontracts.io/01-draft/12-chain-request/schema.json#)

### $schema

The Live Contracts Denied response [JSON schema](http://json-schema.org) URI that describes the JSON structure of the
response. To point to this version of the specification use
`"$schema": "http://specs.livecontracts.io/draft-01/12-chain-request/schema.json#denied-response"`.

### id

The globally unique identifier of the event chain you're requesting. This is typically an
[LTRI](http://specs.livecontracts.io/draft-01/00-ltri/) with an UUID-4; `lt:/event-chains/<uuid-4>`.

### body

The body is a base64 encoded JSON string of a [comment](http://specs.livecontracts.io/draft-01/11-comment/) explaining
why the request is denied.

### timestamp

The is the date and time, in ISO 8601 format, of the response occurred according to the signer.

### previous

The hash of the request for which this is the response.

### signkey

The signer's public key, base58 encoded. This identifies the signer and MUST be used to verify the signature.

### signature

A base58 encoded `Curve25519` signature of the event. To create the signature, first create the message that needs to be
signed. Each line is separated by a single `\n` character.

```
<schema>
<id>
<body>
<timestamp>
<previous>
<signkey>
```

### hash

A base58 encoded SHA256 hash of the event. To create the hash use the same message as was created to sign the response.
