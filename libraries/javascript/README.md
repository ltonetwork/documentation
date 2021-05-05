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

```text
const LTO = require('lto-api').LTO;
const lto = new LTO('T'); // 'T' for testnet, 'L' for mainnet
```

### Account

#### \*\*\*\*

### EventChain

#### **Create an event chain**

```text
const chain = account.createEventChain(); // Creates an empty event chain with a valid id and last hash
```

#### **Create and sign an event and add it to an existing event chain**

```text
const EventChain = require('lto-api').EventChain;
const Event = require('lto-api').Event;

const body = {
  "$schema": "http://specs.livecontracts.io/01-draft/12-comment/schema.json#",
  "identity": {
    "$schema": "http://specs.livecontracts.io/01-draft/02-identity/schema.json#",
    "id": "1bb5a451-d496-42b9-97c3-e57404d2984f"
  },
  "content_media_type": "text/plain",
  "content": "Hello world!"
};

const chain = new EventChain('JEKNVnkbo3jqSHT8tfiAKK4tQTFK7jbx8t18wEEnygya');

chain.addEvent(new Event(body).signWith(account));
```

### HTTPSignature

#### **Create a signature for an http request**

```text
const headers = {
  date: (new Date("April 1, 2018 12:00:00")).toISOString()
};

const request = new Request('http://example.com', 'get', headers);

const httpSign = new HTTPSignature(request, ['(request-target)', 'date']);
const signatureHeader = httpSign.signWith(account); // keyId="FkU1XyfrCftc4pQKXCrrDyRLSnifX1SMvmx1CYiiyB3Y",algorithm="ed25519-sha256",headers="(request-target) date",signature="tMAxot4iWb8gB4FQ2zqIMfH2Fd8kA9DwSoW3UZPj9f8QlpLX5VvWf314vFnM8MsDo5kqtGzk7XOOy0TL4zVWAg=="
```

