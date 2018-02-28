# Live Contracts - Identity

An identity within the event chain. The identity is used for authentication and authorization. An identity is not a
user, a user has an identity for each event chain it's participating in.

## Schemas

* [Identity](#identity-schema)
* [Privilege](#privilege-schema)

## Registration

The first event on the event chain MUST be an identity. This event enables the user or organization to add subsequent
events on the chain. This is the initiator of the chain.

This initial identity can start adding other identities on the chain. If it knows the public key(s) of, it can simply
add them and notify the user about this new chain. If the public key is unknown a new key pair may be generated.

## Example

```json
{
    "$schema": "http://specs.livecontracts.io/draft-01/02-identity/schema.json#",
    "id": "lt:/identities/75baa885-5862-4f81-80f4-60df746ff002",
    "name": "John Doe",
    "email": "john.doe@example.com",
    "node": "app.legalthings.one",
    "privileges": [
        {
            "schema": "http://specs.livecontracts.io/draft-01/02-identity/schema.json#",
            "id": "lt:/identities/75baa885-5862-4f81-80f4-60df746ff002",
            "not": [ "privileges" ],
            "signkey": [ "user", "registration" ]
        },
        {
            "schema": "http://specs.livecontracts.io/draft-01/08-contract/schema.json#",
            "id": "lt:/contracts/c9ddeeca-ad2f-4204-92da-eda18ed0bd21"
        },
        {
            "schema": "http://specs.livecontracts.io/draft-01/11-comment/schema.json#",
            "signkey": "user"
        }
    ],
    "signkeys": {
        "user": "8MeRTc26xZqPmQ3Q29RJBwtgtXDPwR7P9QNArymjPLVQ",
        "system": "FA2CiSAWUEANTxcffctxm8XQfTugZv7VX5C1Qb59vbxj"
    }
}
```

### Identity for invitation to the event chain

```json
{
    "$schema": "http://specs.livecontracts.io/draft-01/02-identity/schema.json#",
    "id": "lt:/identities/3f9bf36c-2245-4fb7-9e0f-e55f1b7ace15",
    "name": "Arnold",
    "email": "arnold@jasny.net",
    "privileges": [
        {
            "schema": "http://specs.livecontracts.io/draft-01/02-identity/schema.json#",
            "id": "lt:/identities/75baa885-5862-4f81-80f4-60df746ff002",
            "not": [ "privileges" ],
            "signkey": [ "user", "registration" ]
        }
    ],
    "signkeys": {
        "registration": "8MeRTc26xZqPmQ3Q29RJBwtgtXDPwR7P9QNArymjPLVQ"
    }
}
```

## Identity schema

[JSON Schema](http://specs.livecontracts.io/draft-01/02-identity/schema.json#)

### $schema

The Live Contracts Identity [JSON schema](http://json-schema.org) URI that describes the JSON structure of the identity.
To point to this version of the specification use `"$schema": "http://specs.livecontracts.io/draft-01/02-identity/schema.json#"`.

### id

A URI as a globally unique identifier for the identity. This is typically an [LTRI](http://specs.livecontracts.io/draft-01/00-ltri/)
using a random UUID-4; `lt:/identities/<uuid-4>`.

### name

The name of the user that has taken this identity.

### email

The email address of the user that has taken this identity.

### image

A URL to a (publicly available) image of the user.

### node

The domain name of the LegalThings One node that the user is running the chain on. Additions to the chain are not
broadcasted, but send to the nodes of the active identities extracted from the event chain.

### privileges

A list of [privileges](#privilege).

### signkeys

The public encryption keys that the identity uses for signing events. It's an object where the key is the type.

Common types are `user` and `system`. The `user` key is kept client side, so you can be sure that the user takes that
action. The system key doesn't belong to the user but to the node the user is running on. It can be specified for an
automated step.

```json
{
    "user": "8MeRTc26xZqPmQ3Q29RJBwtgtXDPwR7P9QNArymjPLVQ",
    "system": "FA2CiSAWUEANTxcffctxm8XQfTugZv7VX5C1Qb59vbxj"
}
```

## Privilege schema

[JSON Schema](http://specs.livecontracts.io/draft-01/02-identity/schema.json#privilege)

A privilege grant the identity to add one type events to the event chain.

### schema

A live contracts schema URI. This determines the type of events that may be added to the event chain.

### id

Set the `id` if the privilege only applies to a specific projection like a process or a contract.

### only

When projecting an item from an event, filter the body so it only contains these properties. Properties listed here
are ignored. This doesn't go more than one level deep.

### not

When projecting an item from an event, filter the body so it does not contains these properties. Properties note listed
here are ignored. This doesn't go more than one level deep.

### signkey

If specified, only this type / these types of sign keys may be used to sign the event.
