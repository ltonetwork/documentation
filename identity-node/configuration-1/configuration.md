---
description: How to configure a trust network?
---

# Trust network

The configuration of roles is flexible and can be done through modifying your node's configuration files. Under `roles`, you can describe what are the roles for your trust network, giving each of them a `description`.

## Structure

The blockchain address of your _own node_ is always the **root** of the network. In the example, the **notary** role is an association from the **node** account to the **notary** account. This makes the approach deterministic, allowing for a change in the configuration without modifying the results.

```javascript
"roles": {
  "root": {
    "description": "My identity",
    "issues": [
       {"type": 100, "role": "notary"},
    ]
  },
  "notary": {
     "description": "Notary"
  }
}
```

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
    "description": "Authority to appoint notaries",
     "issues": [
       {"type": 100, "role": "notary"}
     ]
  },
  "notary": {
     "description": "Notary"
  }
}
```

Under the `issues` property, you specify which roles can be issued, and what's the type of association for that role. In the example above, **authority** role can issue **notary** role to an address when the association is `type: 100`. 

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
       {"type": 900, "role": "king"},
    ]
  },
  "king": {
     "description": "The king of the Netherlands",
     "issues": [
       {"type": 100, "role": "notary"},
       {"type": 101, "role": "authority"} // issues "authority" for type 101
     ]
  },
  "authority": {
     "description": "Delegated authority to appoint notaries",
     "issues": [
       {"type": 100, "role": "notary"},
       {"type": 101, "role": "sub_authority"}, // issues "sub_authority" for type 101
     ]
  },
  "sub_authority": {
     "description": "Secondary delegated authority to appoint notaries",
     "issues": [
       {"type": 100, "role": "notary"},
     ]
  },
  "notary": {
     "description": "Notary",
  }
}
```

In the example above, we can see that **king** can issue the roles of **notary** and **authority** by resolving associations of `type: 100` ****and `type: 101` respectively. But **authority** issues the role of **sub\_authority** when the association is `type: 101`.

### Web of trust

In a hierarchy, there is a centralized autorhority who should be trusted. A web of trust is a decentralized trust network that revolves around trust relationships between identities.

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

_TODO: Say something about an authorization framework for Verifiable Credentials._ [_https://www.w3.org/TR/vc-data-model/\#authorization_](https://www.w3.org/TR/vc-data-model/#authorization)\_\_

```javascript
"roles": {
  "root": {
    "description": "My identity",
    "issues": [
       {"type": 101, "role": "authority"},
    ]
  },
  "authority": {
    "description": "Authority to appoint notaries",
     "issues": [
       {"type": 100, "role": "notary"}
     ]
  },
  "notary": {
     "description": "Notary",
     "authorization": [
       "https://www.w3.org/2018/credentials/examples/v1"
     ]
  }
}
```

Authorization outside of VCs. \(Maybe start with this\).

## Resolving an identity's roles

To retrieve the roles associated with an identity, you need to know the associations made with this particular address. These associations are indexed on the node, so that resolving the roles is easier and faster.

See the documentation on the [REST API](../rest-api.md) for retrieving roles.

