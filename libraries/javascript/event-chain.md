---
description: Microledger on the private layer
---

# Event chain

The event chain is a microledger. Normally a ledger is a hash chain of blocks, with each block containing multiple transactions. The event chain is a hash chain of individual events.

## **Create an event chain**

The event id is generated from the account public key and a random value or a nonce, to prevent collisions.

```javascript
import LTO from '@ltonetwork/lto';
import { EventChain } from '@ltonetwork/lto/events';

const lto = new LTO('T');
const account = lto.account();
const chain = new EventChain(account); // Creates an empty event chain with a valid id
```

## Adding events

The genesis event determines the purpose of the event chain. For an Ownable the genesis event contains an instantiate message.

```javascript
import { EventChain, Event } from '@ltonetwork/lto/events';

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

{% hint style="warning" %}
Events need to be added to the chain before they can be signed.
{% endhint %}

## Loading an event chain

You can store or submit an event chain as JSON. To create an event chain object from JSON data use the `from` method.

```javascript
import { EventChain } from '@ltonetwork/lto/events';

const data = {
  id: '2dZKMnHHsM1MGqTPZ5p3NmmGmAFE4hYFtMwb2e6tGVDMGZT13cBomKoo8DLEWh',
  events: [
    {
      timestamp: 1519882600,
      previous: 'A332JTKSBZipjXxjC1xPxQoheF83WkEBMwLYaYs8yUBa',
      signKey: {
        keyType: 'ed25519',
        publicKey: '2KduZAmAKuXEL463udjCQkVfwJkBQhpciUC4gNiayjSJ',
      },
      signature: '4xn3xqLFXDLVtUjyKXAjTVGfjWkbCbtyQxFSVoYGLRzePGeyRAeEU7a29ZFztgD3ifwBBMWv9T51ecY2ZBNyWvXV',
      hash: 'BRFnaH3UFnABQ1gV1SvT9PLo5ZMFzH7NhqDSgyn1z8wD',
      mediaType: 'application/json',
      data: 'base64:eyJmb28iOiJiYXIiLCJjb2xvciI6InJlZCJ9',
    },
    {
      timestamp: 1519883600,
      previous: 'BRFnaH3UFnABQ1gV1SvT9PLo5ZMFzH7NhqDSgyn1z8wD',
      signKey: {
        keyType: 'ed25519',
        publicKey: '2KduZAmAKuXEL463udjCQkVfwJkBQhpciUC4gNiayjSJ',
      },
      signature: '2hqLhbmh2eX2WhAgbwHhBZqzdpFcjWBYYN5WBj8zcYVKzVbnVH7mESCC9c9acihxWFwfvufnFYxxgFMgJPbpbU4N',
      hash: '9Y9DhjXHdrsUE93TZzSAYBWZS5TDWWNKKh2mihqRCGXh',
      mediaType: 'application/json',
      data: 'base64:eyJmb28iOiJiYXIiLCJjb2xvciI6ImdyZWVuIn0=',
    },
    {
      timestamp: 1519884600,
      previous: '9Y9DhjXHdrsUE93TZzSAYBWZS5TDWWNKKh2mihqRCGXh',
      signKey: {
        keyType: 'ed25519',
        publicKey: '2KduZAmAKuXEL463udjCQkVfwJkBQhpciUC4gNiayjSJ',
      },
      signature: 'BDtUgUJRmbumMMw3V35v7AJrnJ954cBVYQDPyyMc1Hx2x5LZYZkByuUzNJ2zvUWUhCUL3PJF86FQE6WFyQ7VCZU',
      hash: 'C2TsRTTsj7V923RQnEARYL596AXvccd1np32N9of4FaP',
      mediaType: 'application/json',
      data: 'base64:eyJmb28iOiJiYXIiLCJjb2xvciI6ImJsdWUifQ==',
    },
  ],
};

const chain = EventChain.from(data);
```

### Validation

When receiving an event chain from an untrusted source, you should always validate it. The `validate` method checks the integrity of the hash chain and verifies the signatures.

```javascript
chain.validate();
```

The `validate` function does not return any value, but throws an error in case something is wrong with the chain.

Additionally, you may want to check if the signer of the genesis event is the account that created the event chain.

```javascript
const genesisSigner = lto.account(chain.events[0].signKey);
if (!chain.isCreatedBy(genesisSigner))
  throw new Error('Event chain hijacking: genesis event not signed by chain creator');
```

## Partial chain

There are several reasons why you'd only want to get part of the event chain. For instance, you may only need to share added events with another party. To do so you can `startingWith` or `startingAfter` method.

```javascript
const partial1 = chain.startingWith('9Y9DhjXHdrsUE93TZzSAYBWZS5TDWWNKKh2mihqRCGXh');
const partial2 = chain.startingAfter('9Y9DhjXHdrsUE93TZzSAYBWZS5TDWWNKKh2mihqRCGXh');
```

You can either pass an event hash or an `Event` object.

```javascript
const partial = chain.startAfter(chain.event[1]);
```

### Appending a partial chain

You can append the events of a partial chain to the full chain. If there's an overlap in events, the `add` method will skip those events.

If both chains have the same genesis event, but branch off, a `MergeConflict` will be thrown.

```javascript
try {
  chain.add(partial);
} catch (error) {
  if (!(error instanceof MergeConflict)) throw error;
  // handle merge conflict
}
```

The event chain doesn't have an internal consensus mechanism. Instead, it relies on anchoring events on the public chain to determine which branch should be accepted in case of a merge conflict.

## Anchoring events

To detect rollbacks, resolve merge conflicts, and prevent tampering, events should be anchored. Anchoring is the action of writing the event hash to the public chain.

For rollback detection, we use mapped anchoring where the event hash is combined with a state.

```javascript
import LTO from '@ltonetwork/lto';
const lto = new LTO('T');
const account = lto.account();

const appendedEvents = chain.startingAfter(lastKnownEvent);
const achorMap = appendedEvents.anchorMap();

await lto.anchor(account, anchorMap);
```
