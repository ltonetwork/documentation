# Verifiable credentials

A verifiable credential is a tamper-evident credential that has authorship that can be cryptographically verified.

[Verifiable credentials](https://www.w3.org/TR/vc-data-model/) are a W3C standard.

A credential contains claims about a subject made by the issuer. Both the subject and issuer are identified through a [DID](decentralized-identifiers.md). Verifying a claim requires resolving the issuer's DID to validate the credential's signatures (proofs).

Issuing credentials doesn't require the use of a blockchain. Cryptographic signatures of the claims are embedded within the credential.

The LTO public chain can be used as a decentralized revocation registry. In addition, the blockchain can be used for on-chain proof, adding a layer of security in case a private key is ever compromised.

## Issuer

DID documents are dynamic. Verification methods may be added and removed over time. This means that the verification method that was used to sign a credential may have been revoked afterward.

When resolving the DID to verify the credential, it's important to get the document of the issuer that was valid at the moment the credential was issued. To accomplish this, you should add the `versionTime` parameter to the DID.

```json
{
  "@context": "https://www.w3.org/2018/credentials/v1",
  "type": ["VerifiableCredential"],
  "issuer": "did:lto:3Mw3EddCivSFmMD68yRJQsM6awDxJoXUCfa?versionTime=2023-01-01T12:00:00Z",
  "issuanceDate": "2023-01-01T12:00:00Z",
  "credentialSubject": {
    "id": "did:lto:3N1hVvUMuVKEP8m1W3XuHL3ShXn6nimAiMv"
  }
}
```

{% hint style="warning" %}
Dealing with the mutability of DID documents, when verifying credentials is not well established and not part of the verifiable credentials standard. Most implementations simply resolve the issuer DID as-is.
{% endhint %}

## Credential status

The `credentialStatus` property of a verifiable credential is used for the discovery of information about the current status of a verifiable credential, such as whether it is suspended or revoked.

### Identifier

The status id should be the base58 encoded sha256 hash of the credential.

To create the hash, use the canonicalization JSON of the credential, as defined by [RFC8785](https://tools.ietf.org/html/rfc8785). This ensures that the JSON document has the same format when validating the credential.

The `credentialStatus` and `proof` fields should be omitted when calculating the hash.

### LTO Status Registry

LTO Network identity nodes are able to resolve the [credential status](https://www.w3.org/TR/vc-data-model/#status) based on this DID URL. The credential document should contain a `credentialStatus` property with this value.

```
{
  ...
  "credentialStatus": {
    "id": "BYQRy1DbXpDKHTM1qmEAdrv3XzgUu5RfmrgSANkUibwY",
    "type": "LtoStatusRegistry2023"
  }
}
```

{% hint style="warning" %}
There's currently no open standard for decentralized status registries for verifiable credentials.

[Credential Status List 2017](https://w3c-ccg.github.io/vc-csl2017/) is not well supported and the initiative seems abandoned. [Status List 2021](https://www.w3.org/TR/vc-status-list/) is a centralized approach where the issuer maintains a bitstring list of all issued credentials.

Libraries like [Veramo](https://veramo.io/) support custom implementations for credential verification.
{% endhint %}

## Statements

Accounts can make public statements about a verifiable credential through [Statement transactions](broken-reference).

<table><thead><tr><th width="149.33333333333331">Statement</th><th width="108">Type</th><th width="361">Description</th><th>Who?</th></tr></thead><tbody><tr><td>issue</td><td><code>0x10</code></td><td>Issue a credential</td><td>issuer</td></tr><tr><td>revoke</td><td><code>0x11</code></td><td>Revoke a credential</td><td>issuer</td></tr><tr><td>suspend</td><td><code>0x12</code></td><td>Suspend a credential</td><td>issuer</td></tr><tr><td>reinstate</td><td><code>0x13</code></td><td>Reinstate a suspended credential</td><td>issuer</td></tr><tr><td>dispute</td><td><code>0x14</code></td><td>Dispute the validity of the credential</td><td>anyone</td></tr><tr><td>acknowledge</td><td><code>0x15</code></td><td>Acknowledge the validaty of the credential</td><td>anyone</td></tr></tbody></table>

The subject must be the (unencoded) credential status id.

All credential statements, except a dispute, can only be done by the credential issuer. The transaction should be signed with a verification method of the issuer that is active at the moment of the statement. Statements made by others should be ignored.

### Issuance

When issuing a verifiable credential, the issuer can issue a statement with type `0x10` to the public blockchain.

The on-chain statement should have a timestamp of no more than 30 minutes after the issue date of the credential.

A verifier can use the on-chain proof to prevent backdating credentials.

{% hint style="success" %}
The `issue` statement is optional, but highly **recommended**.
{% endhint %}

If a key with the `assertionMethod` relationship is ever compromised, an attacker can continue to issue backdated credentials, that will appear to be valid, even if the verification method is revoked.&#x20;

Without on-chain proof, verifying which credentials are authentic and which are created by the attacker will be impossible.&#x20;

### Revocation

To revoke a credential the issuer can issue a statement transaction with type `0x11`. Revocation is irreversible.

The issuer may add a data entry with the key `reason` to the statement tx. The reason should be a string explaining why the credential has been revoked.

### Suspension

The issuer is able to suspend a credential through a statement transaction with type `0x12`. A suspended credential can either be permanently revoked or reinstated.

To reinstate a credential use a statement with type `0x13`.

The issuer may add a data entry with the key `reason` to the statement tx. The reason should be a string explaining why the credential has been revoked or reinstated.

### Dispute

Anyone can either dispute the claims of a credential. This is done through a statement transaction with type `0x14`.

The issuer should add a data entry with the key `reason` to the statement tx. The reason should be a string explaining why the credential has been revoked or reinstated.

The account that has made the dispute, can cancel it using an acknowledgment statement.

If the credential is compromised, the issuer could issue a dispute on the credential. This dispute can be picked up by the issuer to suspend or revoke the credential.

{% hint style="danger" %}
The `reason` is public and may lead to privacy concerns. When issuing a dispute, consider omitting this field and handling communication off-chain. The credential subject or verifier could [send a message](../private/messaging/) over the LTO Network private layer instead.
{% endhint %}

### Acknowledgment

If a verifier has independently verified the information of a credential, it can acknowledge the validity of it. This is done using a statement transaction with type `0x15`.

If a single account has made multiple dispute and/or acknowledgment statements about a single credential, only the last statement should be used.

***

## Resolving the status

The [identity node](../../node/identity-node/) can resolve the status of a verifiable credential using the credential status id. The response will have a list of statements about the credential made on the public chain.

```json
{
    "id": "GKot5hBsd81kMupNCXHaqbhv3huEbxAFMLnpcX2hniwn",
    "statements": [
        {
            "type": "issue",
            "timestamp": "2023-01-01T12:00:00Z",
            "signer": {
                "id": "did:lto:3Mw3EddCivSFmMD68yRJQsM6awDxJoXUCfa#sign",
                "type": "Ed25519VerificationKey2020",
                "publicKeyMultibase": "zH3C2AVvLMv6gmMNam3uVAjZpfkcJCwDwnZn6z3wXmqPV"
            }
        },
        {
            "type": "dispute",
            "timestamp": "2023-06-01T12:00:00Z",
            "signer": {
                "id": "did:lto:3MqmT15dkZW4a6v4ynVhca1EdPryjCwbahH#sign",
                "type": "Ed25519VerificationKey2020",
                "publicKeyMultibase": "zGL293fxZ2uVG6KEtyJ1dKAfXJBMR2264jHivbhN5zpfD"
            },
            "reason": "Credentials compromised"
        },
        {
            "type": "suspend",
            "timestamp": "2023-06-01T12:03:00Z",
            "signer": {
                "id": "did:lto:3Mw3EddCivSFmMD68yRJQsM6awDxJoXUCfa#sign",            
                "type": "Ed25519VerificationKey2020",
                "publicKeyMultibase": "zH3C2AVvLMv6gmMNam3uVAjZpfkcJCwDwnZn6z3wXmqPV"
            },
            "reason": "Dispute by trusted party"
        },
        {
            "type": "revoke",
            "timestamp": "2023-06-02T12:00:00Z",
            "signer": {
                "id": "did:lto:3Mw3EddCivSFmMD68yRJQsM6awDxJoXUCfa#sign",            
                "type": "Ed25519VerificationKey2020",
                "publicKeyMultibase": "zH3C2AVvLMv6gmMNam3uVAjZpfkcJCwDwnZn6z3wXmqPV"
            }
        }
    ]
}
```

[Anyone can make any statement](../../what-is-lto-network.md#truly-permissionless) on LTO network. You should pass the `issuer` as query parameter when resolving the credential status to filter the statements.

{% hint style="success" %}
For privacy considerations, verifiers should run their own node and not use a public index node. The node maintainer would be capable to gather statistics for verifiable credentials by tracking verification requests.
{% endhint %}

### Trust network

It's up to the verifier to choose what to do with disputes and acknowledgments. Not all statements should be considered. Setting up a [trust network](trust-network.md) can help determine which to consider and which to ignore.
