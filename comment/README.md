# Comment

A comment is an arbitrary message that can be added to the event chain. It MAY be used for a chat-like feature, where
messages are stored as part of the event chain. Everybody who has access to the event chain can read the comments.

### Schemas

* [Comment](#comment-schema)
* [Linked content](#linked-content-schema)

[JSON Schema](https://specs.livecontracts.io/v0.1.0/comment/schema.json) | [changelog](changelog.md)

### Example

```json
{
  "$schema": "https://specs.livecontracts.io/01-draft/comment/schema.json#",
  "identity": {
    "$schema": "https://specs.livecontracts.io/01-draft/identity/schema.json#",
    "id": "1bb5a451-d496-42b9-97c3-e57404d2984f",
    "info": {
      "name": "John Doe",
      "email": "john.doe@example.com"
    }
  },
  "timestamp": "2018-03-01T00:00:00+00:00",
  "content_media_type": "text/plain",
  "content": "Holding this procedure until we called"
}
```

## Comment schema

`https://specs.livecontracts.io/01-draft/comment/schema.json#`

### Properties

#### $schema

The Live Contracts Comment [JSON schema](http://json-schema.org) URI that describes the JSON structure of the comment.

#### identity

The [identity](../identity/) who wrote this comment. Information like name and e-mail MAY be snapshot.

#### timestamp

The date and time when the comment was created.

#### content\_media\_type

The media type (MIME) of the content. This defaults to `text/plain`.

#### content\_encoding

Method that was used to encode the content (typically `none`, `base58` or `base64`). I case of binary content, it must
be encoded.

#### content

The message of the comment as string.

Alternatively the content may not be embedded, but linked. In this case `content` is an object following the
[linked content schema](#linked-content-schema).

#### receipt

The [receipt](../event-chain/README.md#receipt-schema) for anchoring the comment to a global blockchain.

### Linked content schema

`https://specs.livecontracts.io/01-draft/comment/schema.json#linked-content`

Linking the content rather embedding it reduces the size of the event.

```json
{
  "url": "https://example.com/2ejVRPkvyC9q3s5g1t2HWjN9Cf5KxM1BHyrYahevSJ8f.html",
  "hash": "2ejVRPkvyC9q3s5g1t2HWjN9Cf5KxM1BHyrYahevSJ8f",
  "decryptkey": "A9FN0apAXVgica00XpJmTUOdJVsVE6Y9JukxAWpkGLfN"
}
```

#### url

The URL to the file holding the content of the template. It's recommended to use the hash as filename. All nodes that
are allow to participate in the event chain MUST be able access the file. This might mean that the file is publicly
available.

#### hash

A base58 encoded SHA256 hash of the content. The hash should be of the unencrypted content.

#### encryptkey

The file SHOULD be encrypted. Encryption MUST be done using the [AES256 gcm](../cryptography.md#symmetric-encryption)
algorithm. If the content is encrypted, the `encryptkey` property contains the base58 encoded encryption key which can
be used to decrypt the content.
