---
description: Microledger on the private layer
---

# Event chain

The event chain is a microledger. Normally a ledger is a hash chain of blocks, with each block containing multiple transactions. The event chain is a hash chain of individual events.

### **Create an event chain**

The event id is generated from the account public key and a random value or a nonce, to prevent collisions.

```javascript
import LTO, { EventChain } from '@ltonetwork/lto';

const lto = new LTO('T');
const account = lto.account();
const chain = new EventChain(account); // Creates an empty event chain with a valid id
```

### Adding events

The genesis event determines the purpose of the event chain. For an Ownable the genesis event contains an instantiate message.

```javascript
import { EventChain, Account } from '@ltonetwork/lto';

const body = {
  '@context': 'instantiate_msg.json',
  ownable_id: '88pDRu52FpsU3kKHwdvPV21RMkBqVqNnthjfdCesTHQhLnUpanw49n6b2PzGnEy',
  package: 'bafybeie4ts4mbcw4pswzh45bj32ulcyztup2dr7zbbjv3y2ym3q3uuejba',
  network_id: 'T',
};

const lto = new LTO('T');
const account = lto.account();
const chain = new EventChain(account);

new Event(body).addTo(chain).signWith(account);
```

Note that events need to be added to the chain before they can be signed.
