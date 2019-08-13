# REST API

The event-chain, workflow and dispatcher service all expose REST APIs. These are available through the LTO API gateway service which acts as a proxy server.

{% hint style="info" %}
In the following examples, replace `https://lto.example.com` with the domain or IP address of **your** node. 
{% endhint %}

### LTO API client libraries

It's recommended to use one of the LTO API client libraries if available. These libraries are configured to correctly sign requests.

[Read more](api-client-libraries.md)

### HTTP Signature

All requests need to be signed following the [HTTP Signatures specification](https://tools.ietf.org/html/draft-cavage-http-signatures-10) using your ED25519 key set. Each request must have `Date` header. Request with a body like `POST` and `PUT` requests, must have a `Content-Type` and `Digest` header. The `(request-uri)` must also be part of the signature.

The `KeyId` must be the base58 encoded public ED25519 key. The `algorithm` may be either `ed25519` or `ed25519-sha256`. If you use the the SHA-256 version, the signature string needs to be hashed. The signature must be _**base64**_ encoded as required by the http signatures specifications.

### Digest

The HTTP signature string is only constructed from the HTTP headers, not the body. To make sure the body hasn't been manipulated, requests are required to have a `Digest` header as described in [RFC 3230](https://tools.ietf.org/html/rfc3230). The digest needs to be a SHA-256 hash.

_Note that the Digest header, is a different specification than HTTP Digest Authentication._

## Services

{% page-ref page="event-chain-service.md" %}

