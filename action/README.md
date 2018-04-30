# Action

In a process an [action](../scenario/#action-schema) may be performed to trigger a state change or to update the process projection. For each action in the scenario, the `$schema` property MUST be set.

The base schema of an action is `https://specs.livecontracts.io/draft-01/scenario/schema.json#action-schema`. These schemas are specify the type of action. The action type is used to execute an action of to display it correctly in the UI.

_**Basic properties than all actions have, are not described here.**_

### Schemas

* [Choose action](#choose-schema)
* [Form action](#form-schema)
* [Upload action](#upload-schema)
* [View document action](#view-document-schema)
* [Edit action](#edit-schema)
* [Follow link action](#follow-link-schema)
* [NOP action](#nop-schema)
* [HTTP action](#http-schema)

[JSON Schema](https://specs.livecontracts.io/draft-01/action/schema.json) | [changelog](changelog.md)

## Choose schema

`https://specs.livecontracts.io/draft-01/action/schema.json#choose`

This action allows the actor to pick one of the responses.

```json
{
  "$schema": "https://specs.livecontracts.io/draft-01/action/schema.json#choose",
  "actor": "client",
  "responses": {
    "approve": { "label": "Approve" },
    "reject": { "label": "Reject" }
  },
  "default_response": "approve"
}
```

A choose action SHOULD NOT have a `label`, instead the labels of each response is presented to the actor \(as button\).

## Form action schema

`https://specs.livecontracts.io/draft-01/action/schema.json#choose`

Display a [form](../form/) to the actor to fill out information.

```json
{
  "$schema": "https://specs.livecontracts.io/draft-01/action/schema.json#form",
  "actor": "client",
  "form": {
    "id": "lt:/forms/a45f0b2f-21b8-4d09-8295-1665f38e17c7?v=BaM88e44"
  },
  "responses": {
    "ok": {
      "update": {
        "select": "assets.companyInfo"
      }
    }
  }
}
```

Submitting the form will trigger an 'ok' response.

### form schema

A form definition. The form MAY be a complete form, or only the `id` of a form.

If it's only an id, the form SHOULD be fetched client side when the action is activated. It WILL NOT be fetched server side.

## Upload action schema

`https://specs.livecontracts.io/draft-01/action/schema.json#upload`

Ask the actor to upload one or more files. The event body SHOULD be encoded as `multipart/form-data`.

```json
{
  "$schema": "https://specs.livecontracts.io/draft-01/action/schema.json#upload",
  "label": "Upload documents",
  "actor": "client",
  "accept": "image/*, application/pdf",
  "multiple": true,
  "update": {
    "select": "assets.documents"
  }
}
```

### Properties

#### accept

The type of files that are accepted, in MIME format. Use `*` as wildcard.

#### multiple

Boolean flag to allow uploading multiple files at once.

## View document schema

`https://specs.livecontracts.io/draft-01/action/schema.json#view-document`

View or review a [document](../document/). Allow the user to pick one of the responses.

```json
{
  "$schema": "https://specs.livecontracts.io/draft-01/action/schema.json#view-document",
  "actor": "client",
  "document": {
    "id": "lt:/documents/7c403337-a4d6-47a6-89a6-eb989451dc06"
  },
  "responses": {
    "approve": { "label": "Approve" },
    "reject": { "label": "Reject" }
  },
  "default_response": "approve"
}
```

### Properties

#### document

A document definition. The document MAY just be a complete document, or only the `id` of a document.

If it's only an id, the document SHOULD be fetched client side when the action is activated. It WILL NOT be fetched server side.

## Edit document schema

`https://specs.livecontracts.io/draft-01/action/schema.json#edit-document`

Edit a [document](../document/). Also, allow the user to pick one of the responses.

```json
{
  "$schema": "https://specs.livecontracts.io/draft-01/action/schema.json#edit-document",
  "actor": "client",
  "document": {
    "id": "lt:/documents/7c403337-a4d6-47a6-89a6-eb989451dc06"
  },
  "responses": {
    "ok": {
      "label": "Done",
      "update": {
        "set": "assets.document",
        "data": {
          "<merge>": [
            { "<ref>": "assets.document" },
            { "<ref>": "response.data" }
          ]
        }
      }
    }
  }
}
```

The response data MUST be the latest version of document. It MAY only include the `id` with a version number without content, data or metadata.

### Properties

#### document

A document definition. The document MAY just be a complete document, or only the `id` of a document.

If it's only an id, the document SHOULD be fetched client side when the action is activated. It WILL NOT be fetched server side.

## Follow link schema

`https://specs.livecontracts.io/draft-01/action/schema.json#follow-link`

Have the user follow a hyperlink.

### url

The URL to go to.

## NOP schema

`https://specs.livecontracts.io/draft-01/action/schema.json#nop`

No operation \(automated action\).

```json
{
  "$schema": "https://specs.livecontracts.io/draft-01/action/schema.json#nop",
  "actor": "client",
  "responses": {
    "ok": {
      "update": {
        "select": "info.company",
        "data": { "<ref>": "actors.client.organization.name" }
      }
    }
  }
}
```

## HTTP schema

`https://specs.livecontracts.io/draft-01/action/schema.json#http`

Automated action to do a HTTP GET or POST request.

```json
{
  "$schema": "https://specs.livecontracts.io/draft-01/action/schema.json#http",
  "actor": "client",
  "url": "https://api.example.com/",
  "method": "POST",
  "query": {
    "force": true
  },
  "headers": {
    "Content-Type": "application/json"
  },
  "data": {
    "foo": "bar",
    "color": "red"
  },
  "responses": {
    "ok": {
      "update": {
        "select": "assets.foo"
      }
    }
  }
}
```

Multiple parallel HTTP requests

```json
{
  "$schema": "https://specs.livecontracts.io/draft-01/action/schema.json#http",
  "actor": "client",
  "method": "POST",
  "headers": {
    "Content-Type": "application/json"
  },
  "requests": [
    {
      "https://api.example.com/foos/1"
      "data": {
        "color": "red"
      }
    },
    {
      "https://api.example.com/foos/2"
      "data": {
        "color": "blue"
      }
    }
  ]
}
```

### Properties

#### url

HTTP URL of the request.

#### method

HTTP method of the request. Typically `GET` or `POST`.

#### query

HTTP Query as object. This will be URL encoded.

#### headers

HTTP headers as key/value pairs.

#### auth

Either object with `username` and `password` or string for the `Authorization` header.

#### data

HTTP POST body. This is typically a string. If the `Content-Type` is json, the data may be an object which will automatically be encoded.

#### requests

Array with for parallel requests. Each request object can have an `url`, `method`, `query`, `headers`, `auth` and `data` property. For omitted properties, the property of the http action is used instead.
