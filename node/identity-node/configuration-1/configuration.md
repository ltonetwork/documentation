---
description: How to configure a trust network?
---

# Trust network

The configuration of roles is flexible and can be done through modifying your node's configuration files. All of the available properties can be found under `trust_network`. For the roles to take effect, `indexing` must be set to `true`, otherwise the roles will not be stored by the indexer.

```javascript
{
  "trust_network": {
    "indexing": true,
    "roles": {
      "root": { "description": "The root role" }
    }
  }
}
```

Under `roles`, you can describe what are the roles for your trust network, giving each of them a `description`.

## Structure

The blockchain address of your _own node_ is always the **root **of the network. In the example, the **university **role is an association from the **root **account to the **university** account.

```javascript
"roles": {
  "root": {
    "description": "My identity",
    "issues": [
       {"type": 100, "role": "university"},
    ]
  },
  "university": {
     "description": "University"
  }
}
```

{% hint style="warning" %}
When the trust network configuration changes, the roles need to be reindexed. Resolving roles is only deterministic if the configuration is unchanged.
{% endhint %}

### Hierarchy

A role can also issue other roles, which can be configured through the `issues` property. This can be used to create a hierarchy, where your node only creates an association to follow an authority.

```javascript
"roles": {
  "root": {
    "description": "My identity",
    "issues": [
       {"type": 101, "role": "authority"},
    ]
  },
  "authority": {
    "description": "Authority to appoint universities",
     "issues": [
       {"type": 100, "role": "university"}
     ]
  },
  "university": {
     "description": "University"
  }
}
```

Under the `issues` property, you specify which roles can be issued, and what's the type of association for that role. In the example above, **authority **role can issue **university** role to an address when the association is `type: 100`.&#x20;

{% hint style="info" %}
The `issues` property is optional. When omitted, the role is an endpoint; it can't issue other roles.
{% endhint %}

### Sub-roles

The `type` property does not mean the _id_ of the role, but instead the association type. Different roles can issue differently based on the `type`. This can be used to create more complex structures.

```javascript
"roles": {
  "root": {
    "description": "My identity",
    "issues": [
       {"type": 900, "role": "ministry"},
    ]
  },
  "ministry": {
     "description": "Ministry of education",
     "issues": [
       {"type": 100, "role": "university"},
       {"type": 101, "role": "authority"} // issues "authority" for type 101
     ]
  },
  "authority": {
     "description": "Delegated authority to appoint universities",
     "issues": [
       {"type": 100, "role": "university"},
       {"type": 101, "role": "sub_authority"}, // issues "sub_authority" for type 101
     ]
  },
  "sub_authority": {
     "description": "Secondary delegated authority to appoint universities",
     "issues": [
       {"type": 100, "role": "university"},
     ]
  },
  "university": {
     "description": "University",
  }
}
```

In the example above, we can see that **ministry** can issue the roles of **university **and **authority** by resolving associations of `type: 100`** **and `type: 101` respectively. But **authority **issues the role of **sub\_authority** when the association is `type: 101`.

### Web of trust

In a hierarchy, there is a centralized authority who should be trusted. A web of trust is a decentralized trust network that revolves around trust relationships between identities.

```javascript
"roles": {
  "root": {
    "issues": [
       {"type": 32, "role": "level1"}
    ]
  },
  "level1": {
     "issues": [
       {"type": 32, "role": "level2"}
     ]
  },
  "level2": {
     "issues": [
       {"type": 32, "role": "level3"}
     ]
  },
  "level3": {
  }
}
```

## Authorization

[Verifiable credentials](../../../protocol/identities/verifiable-credentials.md) are used for identifying subjects, but VCs alone are not enough to provide a complete toolset for verifying the authorization layer. With the trust network, we can build an [authorization framework](https://www.w3.org/TR/vc-data-model/#authorization) to fill that gap.

The `authorization` property is an array of credential URLs that the role can issue. The framework is built on this property, which can then be used to verify the authority of a credential.

```javascript
"roles": {
  "root": {
    "description": "My identity",
    "issues": [
       {"type": 101, "role": "authority"},
    ]
  },
  "authority": {
    "description": "Authority to appoint universities",
     "issues": [
       {"type": 100, "role": "university"}
     ]
  },
  "university": {
     "description": "University",
     "authorization": [
       "https://www.w3.org/2018/credentials/examples/v1"
     ]
  }
}
```

Now we can validate that the role of **university** is an authority for issuing **/examples/v1 **credential, thus providing another layer of security for the network.

{% hint style="info" %}
Every role is flexible to have multiple credentials, and those can be repeated between different roles. Both **university **and **authority** could issue the same credentials for example.
{% endhint %}

The value for `authorization` should match [JSON-LD](https://www.w3.org/TR/vc-data-model/#json-ld) URLs as used in the `@context` property of the verifiable credential. Our example of university roles contains the context for college degrees:

```javascript
// Verifiable Credential
{
  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://www.w3.org/2018/credentials/examples/v1"
  ],
  "id": "http://example.edu/credentials/1872",
  "credentialSubject": {
    "id": "did:example:ebfeb1f712ebc6f1c276e12ec21",
    "alumniOf": {
      "id": "did:example:c276e12ec21ebfeb1f712ebc6f1",
      "name": "Example University"
    }
  },
  ...
}
```

## Sponsored Roles

You can configure roles to be **sponsored roles**, meaning once an address receives them, a [sponsorship transaction](https://docs.ltonetwork.com/v/edge/protocol/public/transactions/sponsor) will be sent to the configured node. This allows for the authority to effectively sponsor the transactions for an address.

```javascript
"roles": {
  "authority": {
    "description": "Authority to appoint universities",
    "issues": [ {"type": 100, "role": "university"} ]
  },
  "university": {
    "description": "University",
    "sponsored": true // anyone with the role "university" will be sponsored
  }
}
```

In the same way, when an account loses its sponsored roles through revoking, a [cancel sponsorship transaction](https://docs.ltonetwork.com/v/edge/protocol/public/transactions/cancel-sponsor) will be sent, stopping the sponsor.

{% hint style="warning" %}
For sponsored roles, the account configured on the _node wallet_ will be the one sponsoring the accounts that are granted the roles.
{% endhint %}

## Resolving an identity's roles

To retrieve the roles associated with an identity, you need to know the associations made with this particular address. These associations are indexed on the node, so that resolving the roles is easier and faster.

See the documentation on the [REST API](../rest-api.md) for retrieving roles.
