# Private layer

## **Create an event chain**

```text
const chain = account.createEventChain(); // Creates an empty event chain with a valid id and last hash
```

## **Create and sign an event and add it to an existing event chain**

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

