---
description: JavaScript / TypeScript client library for interacting with LTO Network
---

# JavaScript

{% hint style="success" %}
Visit the [project on GitHub](https://github.com/ltonetwork/lto-api.js).
{% endhint %}

## Installation

```text
npm install lto-api @lto-network/lto-transactions --save
```

## Usage

```javascript
const { LTO, Event } = require('lto-api');
const { transfer, broadcast } = require('@lto-network/lto-transactions');

// Account
const lto = new LTO('T'); // 'T' for testnet, 'L' for mainnet
const account = lto.createAccount();

// Public chain
const nodeUrl = 'https://nodes.lto.network';
 
const transerTx = transfer({ 
  amount: 100e8, // 100 LTO
  recipient: '3JmEPiCpfL4p5WswT21ZpWKND5apPs2hTMB',
}, account.seed);

broadcast(signedTx, nodeUrl).then(resp => console.log(resp))

// Private chain
const chain = account.createEventChain();

const body = {
  "$schema": "http://specs.livecontracts.io/01-draft/12-comment/schema.json#",
  "identity": {
    "$schema": "http://specs.livecontracts.io/01-draft/02-identity/schema.json#",
    "id": "1bb5a451-d496-42b9-97c3-e57404d2984f"
  },
  "content_media_type": "text/plain",
  "content": "Hello world!"
};

chain.addEvent(new Event(body).signWith(account));
```

