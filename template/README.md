# Template

[← back](../)

## Template

The Live Contract template can be instantiated to create a contract. It holds a templated version of the natural language text, a form definition to collect the data and links to associated scenarios of the digitized procedures.

### Schemas

[JSON Schema](../template/schema.json) - [http://specs.livecontracts.io/01-draft/template/schema.json\#](http://specs.livecontracts.io/01-draft/template/schema.json#)

* [Template](./#template-schema)
* [Linked content](./#linked-content-schema)
* [Related scenario](./#related-scenario-schema)

### Example

```json
{
  "$schema": "http://specs.livecontracts.io/01-draft/02-template/schema.json#",
  "id": "lt:/templates/ac19b51f-4cd2-413e-b283-a51c533580ad?v=GKot5hBs",
  "name": "NDA US",
  "description": "A general non-disclosure agreement legally enforcable in the United States of America",
  "form": "lt:/forms/3c7c628f-e586-4362-9162-8a0e089a9d06?v=d81kMupN",
  "content": {
    "url": "http://s3.amazonaws.com/exmpale-docs/AZ9Xug4RctZgXnDtTgEPnAg2wSYCs53dsRcVXvPP8234.html",
    "hash: "ba7816bf8f01cfea414140de5dae2223b00361a396177a9cb410ff61f20015ad",
    "encryptkey": "8MeRTc26xZqPmQ3Q29RJBwtgtXDPwR7P9QNArymjPLVQ"
  },
  "content_type": "text/html",
  "locale": "en_US",
  "scenarios": [
    {
      "id": "lt:/scenarios/d2e147fb-cb62-4070-9575-8a672d35724a?v=HECPrZDs",
      "name": "Sign document"
    },
    {
      "id": "lt:/scenarios/0f379b89-1a41-4b97-a9f8-30433fe3c30d?v=8cWTaKQL",
      "name": "NDA violated",
      "paragraph": "#2"
    }
  ]
}
```

### Template schema

`http://specs.livecontracts.io/01-draft/02-template/schema.json#`

#### $schema

The Live Contracts Scenario [JSON schema](http://json-schema.org) URI that describes the JSON structure of the scenario.

#### id

A URI as a unique identifier for the template. This is typically an [LTRI](../ltri/README.md).

The id MUST point to an immutable version of the template. Modifying the template SHOULD always result in a new id. Previous versions of the template SHOULD remain available.

#### name

The name of the template. Shown when listing scenarios. The title SHOULD be unique within your set of scenarios.

#### description

A description of the document shown as template details.

#### form

A form definition or the id of a form definition. When creating a contract, the user is asked to fill out this form in order to gather the contract information.

#### content

The template content in natural language. Typically the template content contains fields that are filled in using the information collected from filling out the form.

Alternatively the content may not be embedded, but linked. In this case `content` is an object following the [linked content schema](./#linked-content-schema).

#### content\_type

The MIME type of the content. It the template has content, the content type must be set.

#### content\_encoding

If specified the string SHOULD be interpreted as binary data and decoded using the encoding named by this property.

#### locale

The language of the template as ISO-639 locale.

#### scenarios

A list of [scenarios that are related](./#related-scenario-schema) to this template.

### Linked content schema

`http://specs.livecontracts.io/01-draft/02-template/schema.json#linked-content`

Linking the content rather embedding it reduces the size of the event.

```json
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

### Related scenario schema

`http://specs.livecontracts.io/01-draft/02-template/schema.json#related-scenarios`

A link to a scenario that relates to this schema. This is either a procedure that is described in the contract or a procedure that applies to the contract.

#### id

The scenario id as URI, typically an LTRI.

#### name

The scenario name.

#### paragraph

The paragraph anchor, if scenario references a particular paragraph of the contract.

