# draft 01

* Added `$schema` property, which can be consulted for the specification version.
* The `id` property is now an LTRI that MUST include the version number.
* Removed `finalized` property, use a tag instead.
* Removed `signatures` property. Signing is a process.
* The `versions` property is an array that only holds IDs to all versions.
* Added `date` property. This the creation date of this version.
* Added `locale` property, which may contain the locale of the contract.
* Added `description` property, which is a long description.
* Added `template` property, which is a URL to the template of this contract.
* Added `data` which is the data used to generate the contract.
* Added `content_media_type` to contain the media type \(no longer dynamic\) and `content_encoding` for \(base64\) encoding.
* Added `content` which can be used to return the content with the contract, though typically `content_url` is used.
* Added `content_url` which is a URL to the textual content of the contract.
* Added `event_chain` property; a link to the event chain of the contract.
* Added `hash`, `signature` and `receipt` which are retrieved from the event of this contract version.

