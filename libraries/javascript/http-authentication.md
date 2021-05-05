# HTTP Authentication

Signing HTTP Messages is described IETF draft [draft-cavage-http-signatures-10](https://tools.ietf.org/id/draft-cavage-http-signatures-10.html).

### **Create a signature for an HTTP request**

```javascript
const headers = {
  date: (new Date("April 1, 2018 12:00:00")).toISOString()
};

const request = new Request('http://example.com', 'get', headers);

const httpSign = new HTTPSignature(request, ['(request-target)', 'date']);
const signatureHeader = httpSign.signWith(account); // keyId="FkU1XyfrCftc4pQKXCrrDyRLSnifX1SMvmx1CYiiyB3Y",algorithm="ed25519-sha256",headers="(request-target) date",signature="tMAxot4iWb8gB4FQ2zqIMfH2Fd8kA9DwSoW3UZPj9f8QlpLX5VvWf314vFnM8MsDo5kqtGzk7XOOy0TL4zVWAg=="
```

