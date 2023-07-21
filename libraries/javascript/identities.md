---
description: Create a DID Document for an account
---

# Identities

Any account on LTO network, for which the public key is known, can be resolved as DID (decentralized identifier). To explicitly create a DID use the identity builder.

```js
import LTO from '@ltonetwork/lto';
import { IdentityBuilder } from '@ltonetwork/lto/identities';

const lto = new LTO('T');
const account = lto.account();

new IdentityBuilder(account)
  .transactions.map(tx => lto.node.broadcast(tx));
```

The main account is known as the management key.

{% hint style="info" %}
Use `Promise.all()` if you wait to await for the transactions to be broadcasted.
{% endhint %}

### Verification methods

By default, the account's public key is the only verification method of the DID. Other verification methods can be added through associations with other accounts.

```js
import LTO from '@ltonetwork/lto';
import { IdentityBuilder } from '@ltonetwork/lto/identities';

const lto = new LTO('T');
const account = lto.account();
const key1 = lto.account({ publicKey: "8cMyCW5Esx98zBqQCy9N36UaGZuNcuJhVe17DuG42dHS" });
const key2 = lto.account({ publicKey: "9ubzzV9tRYTcQee68v1mUPJW7PHdB74LZEgG1MgZUExf" });

const expires = new Date();
expires.setFullYear(expires.getFullYear() + 1);

new IdentityBuilder(account)
  .addVerificationMethod(key1)
  .addVerificationMethod(key2, ['authentication', 'assertionMethod'], expires)
  .transactions.map(tx => lto.node.broadcast(tx));
```

If no verification relationships are specified, it is only listed as a verification method, which is typically not what you want. Optionally, you can have the verification method automatically expire.

#### Revoking verification methods

```javascript
import LTO from '@ltonetwork/lto';
import { IdentityBuilder } from '@ltonetwork/lto/identities';

const lto = new LTO('T');
const account = lto.account();
const key = lto.account({publicKey: "8cMyCW5Esx98zBqQCy9N36UaGZuNcuJhVe17DuG42dHS"});

new IdentityBuilder(account)
  .removeVerificationMethod(key)
  .transactions.map(tx => lto.node.broadcast(tx));
```

Verification methods can also be removed by address.

### Services

```javascript
import LTO from '@ltonetwork/lto';
import { IdentityBuilder } from '@ltonetwork/lto/identities';

const lto = new LTO('T');
const account = lto.account();

new IdentityBuilder(account)
  .addService({type: 'LTORelay', serviceEndpoint: 'ampq://relay.lto.network'})
  .transactions.map(tx => lto.node.broadcast(tx));
```

#### Removing services

```javascript
import LTO from '@ltonetwork/lto';
import { IdentityBuilder } from '@ltonetwork/lto/identities';

const lto = new LTO('T');
const account = lto.account();

new IdentityBuilder(account)
  .removeService({type: 'LTORelay'})
  .transactions.map(tx => lto.node.broadcast(tx));
```

A service may also be removed by id.

### Deactivation

If the management key is compromised, the DID should be deactivated.

```javascript
import LTO from '@ltonetwork/lto';
import { IdentityBuilder } from '@ltonetwork/lto/identities';

const lto = new LTO('T');
const account = lto.account();

new IdentityBuilder(account).deactivate().broadcastTo(lto.node);
```

#### Grant deactivation capability

Allow a trusted party to deactivate the DID in case the management key is lost.

```javascript
import LTO from '@ltonetwork/lto';
import { IdentityBuilder } from '@ltonetwork/lto/identities';

const lto = new LTO('T');
const account = lto.account();
const trustedAccount = lto.account({publicKey: "8cMyCW5Esx98zBqQCy9N36UaGZuNcuJhVe17DuG42dHS"});

const expires = new Date();
expires.setFullYear(expires.getFullYear() + 1);

const revokeDelay = 86400_000; // 24h in ms

new IdentityBuilder(account)
  .grantDisableCapability(trustedAccount, expires, revokeDelay)
  .transactions.map(tx => lto.node.broadcast(tx));
```

The `expires` and `revokeDelay` arguments are optional.

#### Revoke deactivation capability

```javascript
import LTO from '@ltonetwork/lto';
import { IdentityBuilder } from '@ltonetwork/lto/identities';

const lto = new LTO('T');
const account = lto.account();
const trustedAccount = lto.account({publicKey: "8cMyCW5Esx98zBqQCy9N36UaGZuNcuJhVe17DuG42dHS"});

new IdentityBuilder(account)
  .revokeDisableCapability(trustedAccount)
  .transactions.map(tx => lto.node.broadcast(tx));
```
