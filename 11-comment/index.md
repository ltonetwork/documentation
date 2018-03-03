# Live Contracts - Comment

A comment is an arbitrary message that can be added to the event chain. It MAY be used for a chat-like feature, where
messages are stored as part of the event chain. Everybody who has access to the event chain can read the comments.

## Schemas

* [Comment](#comment-schema)

## Comment schema

[JSON Schema](http://specs.livecontracts.io/draft-01/12-comment/schema.json)

### $schema

The Live Contracts Comment [JSON schema](http://json-schema.org) URI that describes the JSON structure of the comment.
To point to this version of the specification use `"$schema": "http://specs.livecontracts.io/draft-01/11-comment/schema.json#"`.

### creator

The information about who wrote this comment, following the [identity specs](http://specs.livecontracts.io/draft-01/02-identity).

### date

The date when the comment was created.

### content_media_type

The media type (MIME) of the content. This defaults to `text/plain`.

### content_encoding

Method that was used to encode the content (typically none or `base64`). I case of binary content, it must be encoded.

### content

The message of the comment.

### hash

A base58 encoded SHA256 hash of the event. It can be used as identifier to find the corresponding event on the event
chain.

### receipt

The [receipt](http://specs.livecontracts.io/draft-01/01-event-chain/#receipt-schema) for anchoring the comment to a
global blockchain.
