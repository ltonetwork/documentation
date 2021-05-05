---
description: JavaScript / TypeScript client library for interacting with LTO Network
---

# JavaScript

{% hint style="success" %}
Visit the [project on GitHub](https://github.com/ltonetwork/lto-api.js).
{% endhint %}

## Installation

```text
npm install lto-api --save
```

## Usage

```javascript
const LTO = require('lto-api').LTO;
const lto = new LTO('T'); // 'T' for testnet, 'L' for mainnet

const account = lto.createAccount();
```

#### \*\*\*\*

