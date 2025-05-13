---
description: Send messages on the private layer
---

# Messages

To share information on the private layer, it should be wrapped as a message.

```javascript
import LTO from '@ltonetwork/lto';
import { Message } from '@ltonetwork/lto/messages';

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

{% hint style="success" %}
The requirement to anchor prevents spam by associating a small cost for each message.
{% endhint %}

### Encrypted messages

Messages can be encrypted so the data can only be read or processed by a specific LTO account. Encrypting a message is done using the public key of the recipient.

To get the public key from an address, we'll need to resolve the address. This uses the LTO DID resolver internally.

```javascript
import LTO from '@ltonetwork/lto';
import { Message } from '@ltonetwork/lto/messages';

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

<pre class="language-javascript"><code class="lang-javascript">import LTO from '@ltonetwork/lto';
import { Message } from '@ltonetwork/lto/messages';

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
<strong>  .from(data)
</strong>  .decryptWith(account);
</code></pre>

### Adding Metadata to Messages

To enhance client-side rendering and improve UX, you can attach metadata to a message before sending it. This metadata allows the receiving wallet to preview the content with a title, description, and thumbnail.

**Supported Metadata Fields**

* `type`: A string identifier (e.g., `"ownable"`) if type is left undefined the library defaults to "basic", generally advised to explictly use "ownable" if its an ownable.
* `title`: A short, human-readable title
* `description`: A brief explanation of the message contents
* `thumbnail`: A binary image preview (max size: **256KB**)

```typescript
const thumbnail = Binary.from(new Uint8Array(buffer)); // ensure <= 256KB

const meta = {
  type: "ownable",
  title: "Cool Ownable",
  description: "Ownable created by @user123",
  thumbnail,
};

const message = new Message(
  messageContent,
  "application/octet-stream",
  meta
)
  .to(recipient)
  .signWith(sender);

await this.relay.send(message);
```

{% hint style="info" %}
Metadata is optional, but including it enables:&#x20;

* Rich previews in wallet interfaces
* Early validation before import
{% endhint %}

## Ownables messages

By default, messages are assigned the `basic` type, which provides minimal descriptive context. While this is sufficient for most simple message exchanges, it lacks the flexibility needed for richer, more descriptive content.

Starting from versions **v0.15.17** and **v0.16.9+**, support for **message metadata** and **message versioning** was introduced. This enhancement allows developers to embed additional context such as a **title**, **description**, and a **thumbnail** directly into the message metadata.

This is particularly valuable for **Ownables**, where visual and descriptive cues are essential for user comprehension. With metadata support, client-side applications can render messages with meaningful context, without decrypting and parsing message bodies just to extract basic display information. This not only improves performance but also significantly enhances the developer and user experience.

```javascript
import {Message, Binary} from '@ltonetwork/lto/messages'


const imageBuffer = await some-image-file.arrayBuffer()

const meta = {
      type: "ownable", //defaults to basic if not provided
      title: "ownableRobot",
      description: "A simple robot ownables",
      thumbnail: Binary.from(new Uint8Array(image-file)),
}

//application-octet sets the mediaType arbitrarily
const message = new Message('hello', "application/octet-stream", meta)
  .encryptFor(recipient)
  .signWith(account);
```

{% hint style="info" %}
Messages with a metadata are marked version=1, Messages without a metadata have a version=0
{% endhint %}

{% hint style="info" %}
Thumbnails are optional, but if you must include it, then it should be in a Binary format
{% endhint %}



## Relay service

By default, messages are posted through [https://relay.lto.network](https://relay.lto.network). Instead of depending on the public relay service, you can [host your own](https://github.com/ltonetwork/relay).

```javascript
import LTO from '@ltonetwork/lto';
import { Relay } from '@ltonetwork/lto/messages';

const lto = new LTO('T');
lto.relay = new Relay('https://my-relay.example.com');
```
