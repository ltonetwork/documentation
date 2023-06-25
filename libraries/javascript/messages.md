---
description: Send messages on the private layer
---

# Messages

To share information on the private layer, it should be wrapped as a message.

```javascript
import LTO, { Message } from '@ltonetwork/lto';

const lto = new LTO('T');
const account = lto.account();

const message = new Message('hello')
  .to('3MsAuZ59xHHa5vmoPG45fBGC7PxLCYQZnbM')
  .signWith(account);
```

To send a message through a public [LTO Relay service](https://github.com/ltonetwork/relay), it needs to be anchored.

```javascript
await lto.anchor(account, message.hash);
await lto.relay.send(message);
```

### Encrypted messages

Messages can be encrypted so the data can only be read or processed by a specific LTO account. Encrypting a message is done using the public key of the recipient.

To get the public key from an address, we'll need to resolve the address. This uses the LTO DID resolver internally.

```javascript
import LTO, { Message } from '@ltonetwork/lto';

const lto = new LTO('T');
const account = lto.account();

const recipient = await lto.resolveAccount('3MsAuZ59xHHa5vmoPG45fBGC7PxLCYQZnbM');

const message = new Message('hello')
  .encryptFor(recipient)
  .signWith(account);
```

{% hint style="warning" %}
Only accounts that have submitted at least one transaction on the public chain can be resolved.
{% endhint %}

The recipient can decrypt the message using its private key.

```javascript
import LTO, { Message } from '@ltonetwork/lto';

const lto = new LTO('T');
const account = lto.account({ seed: 'satisfy sustain shiver skill betray mother appear pupil coconut weasel firm top puzzle monkey seek', nonce: 1 });

const data = {
  type: 'message',
  sender: { keyType: 'ed25519', publicKey: '3ct1eeZg1ryzz24VHk4CigJxW6Adxh7Syfm459CmGNv2' },
  recipient: '3MsAuZ59xHHa5vmoPG45fBGC7PxLCYQZnbM',
  timestamp: '2023-06-20T21:40:40.268Z',
  signature: '362PiaufpQotrVjXJNQFF9HQ3cqKnmgwD3LzkX3PCWHRzqjUGAQxrWPfCC2irvFUqrM4YkWq9jpv6QYiPJMHTDCJ',
  hash: '8cb1ab4507c0f0f9cf9742f98805bf2cb69277c3ac7a845958dfbe087285fbbd',
  encryptedData: 'VuQ5544fbeodXVy86g9yk8zVgCjNNXqrMVOAou9d8SQM+2PF/CPuUm/rWEoB5OHSc40H2V3DheEiqkQ9di66NQ==',
};

const message = Message
  .from(data)
  .decryptWith(account);
```

## Relay service

By default, messages are posted through [https://relay.lto.network](https://relay.lto.network). Instead of depending on the public relay service, you can [host your own](https://github.com/ltonetwork/relay).

```javascript
import LTO, { Relay } from '@ltonetwork/lto';

const lto = new LTO('T');
lto.relay = new Relay('https://my-relay.example.com');
```
