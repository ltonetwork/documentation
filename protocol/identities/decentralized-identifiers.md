---
description: >-
  Decentralized identifiers (DIDs) are a new type of identifier that enables
  verifiable, decentralized digital identity.
---

# Decentralized identifiers (DID)

DIDs are unique universally resolvable identifiers. A DID represents a _subject_, which can be a person, but also an organization, object, document, etc. There is no central register that registers all DIDs. Instead, there a DID has a method that indicates how it can be resolved.

[Decentralized identifiers](https://www.w3.org/TR/did-core/) are a W3C standard. LTO Network implements v1.0.

![](../../.gitbook/assets/did.png)

Resolving a DID results in a DID document. This document contains cryptographic material that allows the _DID controller_ to prove control of the DID.

{% hint style="info" %}
What proofing control of a DID means depends on the subject. If the DID represents a person, it allows that person to identify themselves. In case the DID represents an object, it allows a person to prove he owns that object.
{% endhint %}

DID documents do not contain other (identifying) information, like a name, address, etc.

{% hint style="warning" %}
#### DID vs account

A decentralized identifier (DID) isn't the same as an account. Both correspond with a blockchain address. However, an account is based on a single public/private key pair. A DID can have multiple verification methods, managed through associations.
{% endhint %}

## LTO DID method

DID with the method "**lto**" can be resolved by the LTO Network identity node.

![](../../.gitbook/assets/screenshot-www.w3.org-2021.04.01-12\_44\_05.png)

The method-specific string is an address on the public chain. In the case of derived DIDs, it's followed by a path.

```
lto-did = "did:lto:" lto-address
lto-address = 35\*( ALPHA / DIGIT )
```

The address is case-sensitive.

## Implicit DID

Any address on the LTO public chain can be represented by a DID. The DID document always contains a single verification method containing the public key of the account. This method is applicable for both authentication and assertion.

```javascript
{
  "@context": "https://www.w3.org/ns/did/v1",
  "id": "did:lto:3JugjxT51cTjWAsgnQK4SpmMqK6qua1VpXH",
  "verificationMethod": [
    {
      "id": "did:lto:3JugjxT51cTjWAsgnQK4SpmMqK6qua1VpXH#sign",
      "type": "Ed25519VerificationKey2020",
      "controller": "did:lto:3JugjxT51cTjWAsgnQK4SpmMqK6qua1VpXH",
      "publicKeyMultibase": "zmMyJxTQuXW9bQVLmJeCrWNCSKzsEMkbZQ3xuNavj6Mk"
    },
  ],
  "authentication": [
    "did:lto:3JugjxT51cTjWAsgnQK4SpmMqK6qua1VpXH#sign"
  ],
  "assertionMethod": [
    "did:lto:3JugjxT51cTjWAsgnQK4SpmMqK6qua1VpXH#sign"
  ],
  "keyAgreement": [
    {
      "id": "did:lto:3JugjxT51cTjWAsgnQK4SpmMqK6qua1VpXH#encrypt",
      "type": "X25519KeyAgreementKey2019",
      "controller": "did:lto:3JugjxT51cTjWAsgnQK4SpmMqK6qua1VpXH",
      "publicKeyMultibase": "zmMyJxTQuXW9bQVLmJeCrWNCSKzsEMkbZQ3xuNavj6Mk"
    },
  ],
  "capabilityInvocation": [
    "did:lto:3JugjxT51cTjWAsgnQK4SpmMqK6qua1VpXH#sign"
  ],
  "capabilityDelegation": [
    "did:lto:3JugjxT51cTjWAsgnQK4SpmMqK6qua1VpXH#sign"
  ]
}
```

{% hint style="info" %}
It's only possible to resolve a DID for accounts for which the public key is known on-chain. This is the case for any account that has signed at least one on-chain transaction. Alternatively, public keys can be registered using the [Register](../public/transactions/register.md) transaction.
{% endhint %}

## Verification methods

It's possible to explicitly specify verification methods for a DID document using [associations](../public/transactions/association.md). An association with the type `0x0100` specifies that the public key of the association recipient is a valid verification method for the association sender.

The `subject` field of the association transaction should not be set.

{% hint style="info" %}
The public key of the recipient must be known. The [register transaction](../public/transactions/register.md) allows the management account to register the public keys of all verification methods.
{% endhint %}

#### Revocation

The verification method can be revoked using a [Revoke Association](../public/transactions/revoke-association.md) transaction with association type `0x100` and an empty subject.

#### Key type

LTO Network supports 3 cryptographic algorithms: ed25519, secp256k1, and secp256r1.

<table data-header-hidden><thead><tr><th width="186">Key type</th><th>Verification method key type</th></tr></thead><tbody><tr><td>ed25519</td><td><a href="https://www.w3.org/community/reports/credentials/CG-FINAL-di-eddsa-2020-20220724/"><code>Ed25519VerificationKey2020</code></a></td></tr><tr><td>secp256k1</td><td><a href="https://w3c-ccg.github.io/lds-ecdsa-secp256k1-2019/"><code>EcdsaSecp256k1VerificationKey2019</code></a></td></tr><tr><td>secp256r1</td><td><a href="https://www.w3.org/community/reports/credentials/CG-FINAL-di-ecdsa-2019-20220724/"><code>EcdsaSecp256r1VerificationKey2019</code></a></td></tr></tbody></table>

#### Expiration

Association transactions support expiration. Set the `expires` field to add a verification method that's automatically revoked at a specific date.

The expiration date can be changed by issuing a new association with an updated `expires` field.

### Verification relationships

Different verification relationships enable the associated verification methods to be used for different purposes. It is up to a verifier to ascertain the validity of a verification attempt by checking that the verification method used is contained in the appropriate verification relationship.

By default, a verification method does not have any relationships. These can be specified using the `data` property of the association transaction.

<table><thead><tr><th width="275">Relationship</th><th>Example purpose</th></tr></thead><tbody><tr><td><a href="https://www.w3.org/TR/did-core/#authentication"><code>authentication</code></a></td><td>Authentication like logging into a website</td></tr><tr><td><a href="https://www.w3.org/TR/did-core/#assertion"><code>assertion</code></a></td><td>Issue Verifiable Credential</td></tr><tr><td><a href="https://www.w3.org/TR/did-core/#key-agreement"><code>keyAgreement</code></a></td><td>Encryption and secure communication</td></tr><tr><td><a href="https://www.w3.org/TR/did-core/#capability-invocation"><code>capabilityInvocation</code></a></td><td>Update the DID document</td></tr><tr><td><a href="https://www.w3.org/TR/did-core/#capability-delegation"><code>capabilityDelegation</code></a></td><td>Delegate authority to a subordinate</td></tr></tbody></table>

To change the relationships of an existing verification method, issue a new [Association](../public/transactions/association.md) transaction with updated data.

#### Encryption

For [ed25519 accounts](../accounts/ed25519.md), the _key relationship_ verification method must have type `X25519KeyAgreementKey2019`. The X25519 public key is generated from the ED25519 public key used to sign a transaction.

For secp256k1 and secp256r1 accounts, you can use `EcdsaSecp256k1VerificationKey2019` for encryption. We support [ECIES Hybrid Encryption](https://cryptobook.nakov.com/asymmetric-key-ciphers/ecies-public-key-encryption). However, this may not be understood by verifiers.

### Management key

The key pair of the main account of the DID is known as the management key. This is the only key that can sign for any on-chain transaction. That means it's the only key that can be used to modify the DID document.

{% hint style="warning" %}
Giving a verification method capability relationships does not allow it to do on-chain transactions. In case you want to allow other keys to modify the DID document and make changes in a [trust network](../../node/identity-node/configuration/trust-network.md), you must a [smart account script](../public/transactions/set-script.md).

You should not add verification methods with capability relationships without using a smart account.
{% endhint %}

By default, the management key has all verification relationships. To change the relationships, issue an association with type `0x100`, setting the data to include specific relationships.

{% hint style="success" %}
It's recommended to use the management key only for on-chain transactions and remove the authentication, assertion and key agreement relationships.
{% endhint %}

### Deactivation

If the management key is compromised, the DID should no longer be used. Issuing a statement with type `0x101`, will mark the DID as [deactivated](https://www.w3.org/TR/did-core/#methods).

In addition, it will revoke all verification methods, including the management key. This ensures that the DID document can't be used, including implementations that don't check the metadata.

It's not possible to only revoke the management key, without also revoking all other verification methods.

The recipient of the statement transaction should be omitted. Optionally, a reason can be specified as data entry.

{% hint style="warning" %}
Deactivating the DID account does not prevent the associated account to do transactions on the public chain or private layer.
{% endhint %}

#### Trusted party deactivation

Creating an association with type `0x108` will add a key that can be used to deactivate the DID. This key will be listed in the `capabilityInvocation` relationship (and not as a generic verification method).

The main reason to use this is to allow a trusted authorized party to deactivate a DID document in case the management key is lost.

An authorized party can deactivate a DID using a statement transaction with type `0x102`. The recipient of that statement should be the address of the management key of the DID. Optionally a reason can be specified as data entry.

{% hint style="danger" %}
Adding a verification method through an association with type `0x100` and with the capability invocation relationship, will not allow that key to deactivate the account. You must use association type `0x108`.
{% endhint %}

## Services

Services are used in DID documents to express ways of communicating with the DID subject or associated entities. A [service](https://www.w3.org/TR/did-core/#services) can be any type of service the DID subject wants to advertise, including decentralized identity management services for further discovery, authentication, authorization, or interaction.

Defining services is done through [data transactions](../public/transactions/data.md). These transactions allow setting metadata of an account. This data is used by the indexer service when resolving a DID.

Services must be JSON encoded. Any data entry with a key starting with `did:service:` will be decoded and added as data entry. Beyond that, the entry key is ignored.

A data instruction with the following data entries

```javascript
[
  {
    key: 'did:service:lto-relay',
    type: 'string',  
    value: '{"id":"did:lto:3JugjxT51cTjWAsgnQK4SpmMqK6qua1VpXH#lto-relay","type":"LTORelay","serviceEndpoint":"ampq://relay.lto.network"}'
  },
  {
    key: 'did:service:https://bar.example.com',
    type: 'string',
    value: '{"id":"https://bar.example.com","type":"LinkedDomains","serviceEndpoint":"https://bar.example.com"}'
  }
]
```

results in the following `service` property of the DID document

```json
{
  "service": [
    {
      "id": "did:lto:3JugjxT51cTjWAsgnQK4SpmMqK6qua1VpXH#lto-relay",
      "type": "LTORelay",
      "serviceEndpoint": "ampq://relay.lto.network"
    },
    {
      "id": "https://bar.example.com",
      "type": "LinkedDomains", 
      "serviceEndpoint": "https://bar.example.com"
    }    
  ]
}
```

## Versioning

LTO Did as versioned and support the [`versionTime` DID parameter](https://www.w3.org/TR/did-core/#did-parameters).

```
did:lto:3JugjxT51cTjWAsgnQK4SpmMqK6qua1VpXH?versionTime=2023-03-01T17:00:00Z
```

Using `versionTime` will return the DID document at the moment of the given time. If the DID document has changed since that time, the metadata will contain an `updated` and a `nextUpdate` property.

The DID will be marked as `created` at the moment when the public key is known on-chain. Requesting a DID with `versionTime` before the creation date will result in a not-found.

### History

Adding the `history` DID parameter will add a list of changes to the DID metadata. This parameter is non-standard.
