---
description: JavaScript / TypeScript client library for interacting with LTO Network
---

# JavaScript

{% hint style="success" %}
Visit the [project on GitHub](https://github.com/ltonetwork/lto-api.js).
{% endhint %}

## Installation

```
npm install @ltonetwork/api --save
```

## Usage

```javascript
import LTO from '@ltonetwork/lto';
import { Transfer } from '@ltonetwork/transactions';
import { Event } from '@ltonetwork/events';
import { Message } from '@ltonetwork/messages';

// Account
const lto = new LTO('T'); // 'T' for testnet, 'L' for mainnet
const account = lto.account();

// Public layer
const amount = 1000;
const recipient = "3JmEPiCpfL4p5WswT21ZpWKND5apPs2hTMB";

const transaction = new Transfer(recipient, amount).signWith(account);
await lto.node.broadcast(transaction);

// Private layer
const chain = new EventChain(account);

const body = {
  '@context': 'instantiate_msg.json',
  ownable_id: '88pDRu52FpsU3kKHwdvPV21RMkBqVqNnthjfdCesTHQhLnUpanw49n6b2PzGnEy',
  package: 'bafybeie4ts4mbcw4pswzh45bj32ulcyztup2dr7zbbjv3y2ym3q3uuejba',
  network_id: 'T',
};

new Event(body).addTo(chain).signWith(account);

const message = new Message(chain).to(recipient).signWith(account);
await lto.anchor(message.hash);
await lto.relay.send(message);
```

## Testnet

To obtain testnet tokens, please join the [@ltotech Telegram group](https://app.gitbook.com/s/-MBYc9qN1f4JIHaaKv_7/hoofdstuk-4-check-de-status-van-de-opheffing-bij-de-kvk) and ask for testnet tokens. Testnet tokens will be provided to you by the community for free.
