# Live Contracts - Comment

### Abstract

A comment is an arbitrary message that can be added to the event chain. It MAY be used for a chat-like feature, where
messages are stored as part of the event chain. Everybody who has access to the event chain can read the comments.

### Note to Readers

The issues list for this draft can be found at <https://github.com/legalthings/livecontracts-specs/issues>.
For additional information, see <http://livecontracts.io/>.

To provide feedback, use this issue tracker, the communication methods listed on the homepage, or email the document
editors.

### Copyright Notice

Copyright (c) 2017 LegalThings One. All rights reserved.

You may use this specification under the [Creative Commons Attribution 4.0 International Public License](https://raw.githubusercontent.com/legalthings/livecontracts-specifications/master/LICENSE).

## Comment

### $schema

The Live Contracts Comment [JSON schema](http://json-schema.org) URI that describes the JSON structure of the comment.
To point to this version of the specification use `"$schema": "http://specs.livecontracts.io/draft-01/11-comment/schema.json#"`.

### creator

The information about who wrote this comment, following the [identity specs](http://specs.livecontracts.io/draft-01/05-identity).

### date

The date when the comment was created.

### content_media_type

The media type (MIME) of the content. This defaults to `text/plain`.

### content_encoding

Method that was used to encode the content (typically none or `base64`). I case of binary content, it must be encoded.

### content

The message of the comment.

### hash

A base58 encoded sha256 hash of the event. It can be used as identifier to find the corresponding event on the event
chain.

### receipt

The receipt for anchoring the comment to a global blockchain. See the [event chain specs](http://specs.livecontracts.io/draft-01/01-event-chain).
