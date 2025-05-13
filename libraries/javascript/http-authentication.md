# HTTP Authentication

To authenticate to a backend service with an LTO account, you can sign the HTTP request using the [HTTP Message Signatures draft standard](https://www.ietf.org/archive/id/draft-ietf-httpbis-message-signatures-00.html).

## Installation

```
npm install @ltonetwork/http-message-signatures --save
```

## Signing

The `sign()` method accepts an LTO account as the `signer`. The `keyid` will be the public key of the account.

```javascript
import LTO from '@ltonetwork/lto';
import { sign } from '@ltonetwork/http-message-signatures';

const lto = new LTO();
const account = lto.account();

const request = {
  method: 'GET',
  url: 'https://example.com/api/data',
};

const signedRequest = await sign(request, { signer: account });

// ... Send the signed request to the server
```

You can sign a [Fetch API Request](https://developer.mozilla.org/en-US/docs/Web/API/Request) object or a plain object.

## Verification

The `verify()` method accepts an LTO client as verifier. The key type is determined based on the algorithm specified in the `Signature`. The keyid is used as the public key. The `verify()` method uses the LTO Client to create an account from the public key and verify the signature.

```javascript
import LTO from '@ltonetwork/lto';
import { verify } from '@ltonetwork/http-message-signatures';

const lto = new LTO();

const request = {
  method: 'GET',
  url: 'https://example.com/api/data',
  headers: {
    'Signature-Input': 'sig1=("@method" "@path" "@authority");created=1618884475;keyid="2KduZAmAKuXEL463udjCQkVfwJkBQhpciUC4gNiayjSJ";alg=ed25519',
    'Signature': 'sig1=:base64signature:'
  }
};

(async () => {
  try {
    const account = await verify(request, lto);
    console.log('Verification succeeded');
  } catch (err) {
    console.error('Verification failed:', err.message);
  }
})();
```

