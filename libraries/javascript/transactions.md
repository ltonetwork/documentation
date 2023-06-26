---
description: Send transactions on the public layer
---

# Transactions

## Basic usage

```js
import LTO, { Binary } from '@ltonetwork/lto';
enum RELATIONSHIP { MEMBER_OF=0x3400 };

lto = new LTO('T');
const account = lto.account({seed: seed});

lto.transfer(account, recipient, 100 * 10^8);
lto.massTransfer(account, [{recipient: recipient1, amount: 100 * 10^8}, {recipient: recipient2, amount: 50 * 10^8}]);
lto.anchor(account, new Binary('some value').hash(), new Binary('other value').hash());
lto.associate(account, RELATIONSHIP.MEMBER_OF, recipient);
lto.revokeAssociation(account, RELATIONSHIP.MEMBER_OF, recipient);
lto.lease(account, recipient, 10000 * 10^8);
lto.cancelLease(account, leaseId);
lto.sponsor(account, otherAccount);
lto.cancelSponsorship(account, otherAccount);

lto.getBalance(account);
lto.setData(account, {foo: 'bar'});
lto.getData(account);
```

_Amounts are in `LTO * 10^8`. Eg: 12.46 LTO is `1246000000`._

## Executing Transactions

The `LTO` class provides a simple way for doing transactions. Alternatively you can create a transaction object, sign it, and broadcast it.

### Create transaction

```js
import {Transfer} from '@ltonetwork/lto';

const transaction = new Transfer(recipient, amount);
```

### Sign transaction

The Transaction needs then to be signed. In order to sign a transaction an account is needed.

```js
account.sign(transaction);
```

### Broadcasting transaction

For last the transaction needs to be broadcasted to the node. In order to do so we need to connect to the node using the PublicNode class.

```js
const broadcastedTx = await lto.node.broadcast(transaction);
```

### Fluent interface

Transaction classes have convenience methods, providing a fluent interface

```js
import {Transfer} from '@ltonetwork/lto';

const transaction = await new Transfer(recipient, amount)
    .signWith(account)
    .broadcastTo(lto.node);
```

### Sponsoring transactions

A second account can offer to pay for the transaction fees by co-signing the transaction.

```js
import {Anchor} from '@ltonetwork/lto';

const transaction = await new Anchor(new Binary('foo').hash())
    .signWith(someAccount)
    .sponsorWith(mainAccount)
    .broadcastTo(lto.node);
```

Alternatively, you can set the `parent` property of an account to automatically have the parent sponsor all transactions of the child.

## Transaction types

### Transfer Transaction

```js
import {Transfer} from '@ltonetwork/lto';

const transaction = new Transfer(recipient, amount, attachment)
```

### Mass Transfer Transaction

```js
import {MassTransfer} from '@ltonetwork/lto';

const transaction = new MassTransfer([{recipient: recipient1, amount: amount1}, {recipient: recipient2, amount: amount2}], attachment)
```

### Anchor Transaction

```js
import {Anchor} from '@ltonetwork/lto';

const transaction = new Anchor(hash);
```

### Lease Transaction

```js
import {Lease} from '@ltonetwork/lto';

const transaction = new Lease(recipient, amount);
```

### Cancel Lease Transaction

```js
import {CancelLease} from '@ltonetwork/lto';

const transaction = new CancelLease(leaseId);
```

### SetScript Transaction

Create a `SetScript` transaction using the `compile` method of the public node.

```js
const transaction = lto.node.compile(script);
```

Clear a script by using `null` as compiled script.

```js
import {SetScript} from '@ltonetwork/lto';

const transaction = new SetScript(null);
```

### Sponsorship transaction

```js
import {SetScript} from '@ltonetwork/lto';

const transaction = new Sponsorship(recipient);
```

### Cancel Sponsorship transaction

```js
import {CancelSponsorship} from '@ltonetwork/lto';

const transaction = new CancelSponsorship(recipient);
```

### Association transaction

```js
import {Association} from '@ltonetwork/lto';

transaction = new Association(recipient, association_type, hash);
```

### Revoke Association transaction

```js
import {RevokeAssociation} from '@ltonetwork/lto';

transaction = new RevokeAssociation(recipient, association_type, hash);

```

## Public Node

By default the following public nodes are used

* **Mainnet** - https://nodes.lto.network
* **Testnet** - https://testnet.lto.network

To use your own public node, set the node address of the `LTO` object.

```
lto.nodeAddress = "http://localhost:6869";
```

The `lto.node` object will automatically be replaced when the node address is changed.
