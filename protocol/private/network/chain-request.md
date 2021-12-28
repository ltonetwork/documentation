# Chain request

## Chain request

A node may request a chain or a portion of a chain. When an identity is registers, a node might request the full event chain for that identity. If a node receives an event, but can't find the corresponding previous event, it will request the missing events.

### Schemas

* [Chain request](chain-request.md#chain-request-schema)
* [Request body](chain-request.md#request-body-schema)
* [Denied response](chain-request.md#denied-response-schema)

[JSON Schema](https://specs.livecontracts.io/v0.1.0/chain-request/schema.json) | [changelog](https://github.com/legalthings/livecontracts-specs/tree/f138bf7777b31f535d6fa21c0ddad3a4aaea45d5/chain-request/changelog.md)

### Request and response

A node can request a complete chain or a portion of the chain. A chain request is discarded when processed and not stored on the event chain. The node will validate the request and send an event chain with only the missing events.

It needs to sign the request using a `signkey` that matches with an identity that is on the chain. If the request doesn't validate or if the `signkey` doesn't belong to an identity, the request will be denied.

### Example

```javascript
{
  "$schema": "https://specs.livecontracts.io/v0.1.0/chain-request/schema.json#",
  "body": "ew0KICAiaWQiOiAibHQ6L2V2ZW50LWNoYWlucy9kMjlkMDQ5Ni1hZDQyLTRlZDctYWEzNy1iZTMxYTg3ZWE5M2UiLA0KICAiZnJvbSI6IFsNCiAgICAiOVRLNGZSYnFKS1dya2g4NXl6WDlpVHpEakZVMzkyUjE0YU5pNjdDUXRlaUEiLA0KICAgICI3bktzUnNGNGN1am9ORjluODdXMnkxV0ZjNm5DQUVnbzEzbmJneHdhZVl4NSIsDQogICAgIjc3SkQ1ZWJucmtDalJSYzNobVNjMmZaZHBRNzQ2emI0Y21YQjd6a2Z2amZDIg0KICBdLA0KICAiZW5jcnlwdGtleSI6ICJMNkZzZ0h4M2VpdHlOTFZmbmZLR3NwOFBUN2tKTzRuZVJCaEg2dVEyRUFrbCINCn0=",
  "timestamp": "2018-03-04T00:00:00+00:00",
  "signkey": "8MeRTc26xZqPmQ3Q29RJBwtgtXDPwR7P9QNArymjPLVQ",
  "signature": "5zk7MGmATLsdbW1C1kiZASuxtN7graME5FBcD6Uti9TGd21i9jgzEYHF9Qaz9cYVV6ZoHg2kPAyiukTtSCW36EuZ",
  "hash": "BAf5XVyzRwB1KNg6Q38RdPaYaRQMbqcXeGoQwj9rKidH"
}
```

The body is a base64 encoded JSON string of the request

```javascript
{
  "$schema": "https://specs.livecontracts.io/v0.1.0/chain-request/schema.json#body",
  "id": "lt:/event-chains/d29d0496-ad42-4ed7-aa37-be31a87ea93e",
  "from": [
    "9TK4fRbqJKWrkh85yzX9iTzDjFU392R14aNi67CQteiA",
    "7nKsRsF4cujoNF9n87W2y1WFc6nCAEgo13nbgxwaeYx5",
    "77JD5ebnrkCjRRc3hmSc2fZdpQ746zb4cmXB7zkfvjfC"
  ],
  "encryptkey": "L6FsgHx3eityNLVfnfKGsp8PT7kJO4neRBhH6uQ2EAkl"
}
```

The request can't be validated it's denied.

```javascript
{
  "$schema": "https://specs.livecontracts.io/v0.1.0/chain-request/schema.json#denied-response",
  "request": {
    "id": "lt:/event-chains/d29d0496-ad42-4ed7-aa37-be31a87ea93e",
    "hash": "BAf5XVyzRwB1KNg6Q38RdPaYaRQMbqcXeGoQwj9rKidH"
  },
  "comment": "",
  "timestamp": "",
  "signkey": "",
  "signature": "",
  "hash": ""
}
```

## Chain request schema

`https://specs.livecontracts.io/v0.1.0/chain-request/schema.json#`

To create a signature and hash, first create the message. Each line is separated by a single `\n` character. There is no trailing `\n` at the end of the message.

```
<body>
<timestamp>
<signkey>
```

_This is similar to the message of an event, however the chain request doesn't have a_ `previous` _field._

### Properties

#### $schema

The Live Contracts Chain request [JSON schema](http://json-schema.org) URI that describes the JSON structure of the chain request.

#### body

The body is a base64 encoded JSON string of the [request body](chain-request.md#request-body-schema).

#### timestamp

The is the date and time, in ISO 8601 format, of the request occurred according to the signer.

#### signkey

The signer's public key, base58 encoded. This identifies the signer and MUST be used to verify the signature.

#### signature

A base58 encoded `Curve25519` signature of the event. To create the signature, first create the message that needs to be signed.

#### hash

A base58 encoded SHA256 hash of the event. To create the hash use the same message as was created to sign the request.

## Request body schema

`https://specs.livecontracts.io/v0.1.0/chain-request/schema.json#body`

### Properties

#### $schema

The Live Contracts Chain request [JSON schema](http://json-schema.org) URI that describes the JSON structure of the chain request.

#### id

The globally unique identifier of the event chain you're requesting. This is typically an [LTRI](broken-reference) with an UUID-4; `lt:/event-chains/<uuid-4>`.

#### from

A hash or an array of hashes that function as starting point of the events the nodes need to receive. To receive the complete chain `from` must be the base58 encoded SHA256 hash of the event id.

A node can't match a received event, because the hash in `previous` doesn't match any events in the event chain it holds. In that case `from` SHOULD be an array with the last event an identity of the other node has signed up to the last of event in the chain. The last event of the other node has signed is the latest point to where you can be sure the chains are identical and didn't branch.

If a node finds out a the chain was branched it needs to handle it through [conflict resolution](conflict-resolution.md).

#### encryptkey

The public key for encrypting the response data. The private decrypt key is hold and kept secret by the node which is doing the request.

If `encryptkey` isn't specified, the identity's encryptkey is used to encrypt the response. Nodes SHOULD NOT send an unencrypted event chain as response.

## Denied response schema

`https://specs.livecontracts.io/v0.1.0/chain-request/schema.json#denied-response`

To create a signature and hash, first create the message. Each line is separated by a single `\n` character. There is no trailing `\n` at the end of the message.

```
<request.id>
<request.hash>
<comment>
<timestamp>
<signkey>
```

### Properties

#### $schema

The Live Contracts Denied response [JSON schema](http://json-schema.org) URI that describes the JSON structure of the response.

#### request.id

The identifier of the event chain you're requesting as LTRI `lt:/event-chains/<id>`.

#### request.hash

The hash of the request for which this is the response.

#### comment

The body is a base64 encoded JSON string of a [comment](broken-reference) explaining why the request is denied.

#### timestamp

The is the date and time, in ISO 8601 format, of the response occurred according to the signer.

#### signkey

The signer's public key, base58 encoded. This identifies the signer and MUST be used to verify the signature.

#### signature

A base58 encoded `ED25519` signature of the event. To create the signature, first create the message that needs to be signed.

#### hash

A base58 encoded SHA256 hash of the event. To create the hash use the same message as was created to sign the response.
