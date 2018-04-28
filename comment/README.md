# index

[← back](../)

## Comment

A comment is an arbitrary message that can be added to the event chain. It MAY be used for a chat-like feature, where messages are stored as part of the event chain. Everybody who has access to the event chain can read the comments.

### Schemas

[JSON Schema](../13-comment/schema.json) - [http://specs.livecontracts.io/01-draft/12-comment/schema.json](http://specs.livecontracts.io/01-draft/12-comment/schema.json)

* [Comment](index.md#comment-schema)
* [Linked content](index.md#linked-content-schema)

### Example

```javascript
{
  "$schema": "http://specs.livecontracts.io/01-draft/12-comment/schema.json#",
  "identity": {
    "$schema": "http://specs.livecontracts.io/01-draft/02-identity/schema.json#",
    "id": "1bb5a451-d496-42b9-97c3-e57404d2984f",
    "name": "John Doe",
    "email": "john.doe@example.com"
  },
  "timestamp": "2018-03-01T00:00:00+00:00",
  "content_media_type": "text/plain",
  "content": "Holding this procedure until we called"
}
```

### Comment schema

`http://specs.livecontracts.io/01-draft/12-comment/schema.json#`

#### $schema

The Live Contracts Comment [JSON schema](http://json-schema.org) URI that describes the JSON structure of the comment.

#### identity

The [identity](../02-identity/README.md) who wrote this comment. Information like name and e-mail MAY be snapshot.

#### timestamp

The date and time when the comment was created.

#### content\_media\_type

The media type \(MIME\) of the content. This defaults to `text/plain`.

#### content\_encoding

Method that was used to encode the content \(typically none, `base58` or `base64`\). I case of binary content, it must be encoded.

#### content

The message of the comment as string.

Alternatively the content may not be embedded, but linked. In this case `content` is an object following the [linked content schema](index.md#linked-content-schema).

#### receipt

The [receipt](../01-event-chain/README.md#receipt-schema) for anchoring the comment to a global blockchain.

### Linked content schema

`http://specs.livecontracts.io/01-draft/12-comment/schema.json#linked-content`

Linking the content rather embedding it reduces the size of the event.

```javascript
{
  "url": "https://example.com/2ejVRPkvyC9q3s5g1t2HWjN9Cf5KxM1BHyrYahevSJ8f.html",
  "hash": "2ejVRPkvyC9q3s5g1t2HWjN9Cf5KxM1BHyrYahevSJ8f",
  "decryptkey": "A9FN0apAXVgica00XpJmTUOdJVsVE6Y9JukxAWpkGLfN"
}
```

#### url

The URL to the file holding the content of the template. It's recommended to use the hash as filename. All nodes that are allow to participate in the event chain MUST be able access the file. This might mean that the file is publicly available.

#### hash

A base58 encoded SHA256 hash of the content. The hash should be of the unencrypted content.

#### encryptkey

The file SHOULD be encrypted. Encryption MUST be done using the [AES256 gcm](../cryptography.md#symmetric-encryption) algorithm. If the content is encrypted, the `encryptkey` property contains the base58 encoded encryption key which can be used to decrypt the content.

