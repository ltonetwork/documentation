---
description: Create a DID Document for an account
---

# Identities

Any account on LTO network, for which the public key is known, can be resolved as DID (decentralized identifier). To explicitly create a DID use the identity builder.

```js
import {IdentityBuilder} from '@ltonetwork/lto';

const account = lto.account();

new IdentityBuilder(account)
  .transactions.map(tx => lto.broadcast(tx));
```

### Verification methods

By default the account's public key is the only verification method of the DID. Other verification methods can be added through associations with other accounts.

```js
import {IdentityBuilder, VerificationRelationship as VR} from '@ltonetwork/lto';

const account = lto.account();
const key1 = lto.account({publicKey: "8cMyCW5Esx98zBqQCy9N36UaGZuNcuJhVe17DuG42dHS"});
const key2 = lto.account({publicKey: "9ubzzV9tRYTcQee68v1mUPJW7PHdB74LZEgG1MgZUExf"});

new IdentityBuilder(account)
  .addVerificationMethod(key1)
  .addVerificationMethod(key2, VR.authentication | VR.capabilityInvocation)
  .transactions.map(tx => lto.broadcast(tx));
```

_Use `Promise.all()` if you wait to await for the transactions to be broadcasted._
