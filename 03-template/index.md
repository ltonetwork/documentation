[[← back](../)]

# Template

The Live Contract template can be instantiated to create a contract. It holds a templated version of the natural
language text, a form definition to collect the data and links to associated scenarios of the digitized procedures.

## Schemas

* [Template](#template-schema)
* [Linked content](#linked-content-schema)
* [Related scenario](#related-scenario-schema)

## Template schema

[JSON Schema](schema.json#)

### $schema

The Live Contracts Scenario [JSON schema](http://json-schema.org) URI that describes the JSON structure of the scenario.
To point to this version of the specification use `"$schema": "http://specs.livecontracts.io/draft-01/04-scenario/schema.json#"`.

### id

A URI as a unique identifier for the template. This is typically an [LTRI](http://specs.livecontracts.io/draft-01/00-ltri/).

The id MUST point to an immutable version of the template. Modifying the template SHOULD always result in a new id.
Previous versions of the template SHOULD remain available.

### name

The name of the scenario. Shown when listing scenarios. The title SHOULD be unique within your set of scenarios.

### description

A description of the procedure shown as scenario details.

### form

A form definition or the id of a form definition. When creating a contract, the user is asked to fill out this form in
order to gather the contract information.

### content

The template content in natural language. Typically the template content contains fields that are filled in using the
information collected from filling out the form.

Alternatively the content may not be embedded, but linked. In this case `content` is an object following the
[linked content schema](#linked-content-schema).

### content_type

The MIME type of the content. It the template has content, the content type must be set.

### content_encoding

If specified the string SHOULD be interpreted as binary data and decoded using the encoding named by this property.

### locale

The language of the template as ISO-639 locale.

### scenarios

A list of scenarios that are related to this template. These are either procedures that are described in the contract
or procedures that apply to the contract.

## Linked content schema

[JSON Schema](schema.json#linked_content)

Linking the content rather embedding it reduces the size of the event.

```json
{
  "url": "https://example.com/2ejVRPkvyC9q3s5g1t2HWjN9Cf5KxM1BHyrYahevSJ8f.html",
  "hash": "2ejVRPkvyC9q3s5g1t2HWjN9Cf5KxM1BHyrYahevSJ8f",
  "decryptkey": "A9FN0apAXVgica00XpJmTUOdJVsVE6Y9JukxAWpkGLfN"
}
```

### url

The URL to the file holding the content of the template. It's recommended to use the hash as filename. All nodes that
are allow to participate in the event chain MUST be able access the file. This might mean that the file is publicly
available.

### hash

A base58 encoded SHA256 hash of the content. The hash should be of the unencrypted content.

### decryptkey

The file SHOULD be encrypted. Encryption MUST be done using the [ChaCha20](http://specs.livecontracts.io/draft-01/cryptography.md#Encryption)
algorithm. If the content is encrypted, the `decryptkey` property contains the base58 encoded public key which can be
used to decrypt the content.

## Related scenario schema

[JSON Schema](schema.json#scenario)

### id

The scenario id as URI, typically an LTRI.

### name

The scenario name.

### paragraph

The paragraph anchor, if scenario references a particular paragraph of the contract.
