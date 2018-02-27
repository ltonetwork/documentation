# Live Contracts - Identity

### Abstract

An identity within the event chain. The identity is used for authentication and authorization. An identity is not a
user, a user has an identity for each event chain it's participating in.

### Note to Readers

The issues list for this draft can be found at <https://github.com/legalthings/livecontracts-specs/issues>.
For additional information, see <http://livecontracts.io/>.

To provide feedback, use this issue tracker, the communication methods listed on the homepage, or email the document
editors.

### Copyright Notice

Copyright (c) 2017 LegalThings One. All rights reserved.

You may use this specification under the [Creative Commons Attribution 4.0 International Public License](https://raw.githubusercontent.com/legalthings/livecontracts-specifications/master/LICENSE).

## Identity

### $schema

The Live Contracts Identity [JSON schema](http://json-schema.org) URI that describes the JSON structure of the identity.
To point to this version of the specification use `"$schema": "http://specs.livecontracts.io/draft-01/05-identity/schema.json#"`.

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

A list of privileges.

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

## Privilege

A privilege grant the identity to add one type events to the event chain.

### schema

A live contracts schema URI. This determines the type of events that may be added.

### id

Set the `id` if the privilege only applies to a specific projection like a process or a contract.

### only

When projecting an item from an event, filter the body so it only contains these properties. Properties listed here
are ignored. This doesn't go more than one level deep.

### not

When projecting an item from an event, filter the body so it does not contains these properties. Properties note listed
here are ignored. This doesn't go more than one level deep.

### signkey

If specified, only this type of sign keys may be used to sign the event.
