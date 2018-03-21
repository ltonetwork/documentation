[← back](../)

## Action

In a process an [action](../04-scenario/#action-schema) may be performed to trigger a state change or to update the
process projection. For each action in the scenario, the `$schema` property MUST be set.

The base schema of an action is `http://specs.livecontracts.io/draft-01/04-scenario/schema.json#action-schema`. These
schemas are specify the type of action. The action type is used to execute an action of to display it correctly in the
UI.

_Basic properties of all actions are not described here._

## Schemas

[JSON Schema](schema.json) - http://specs.livecontracts.io/draft-01/06-action/schema.json#

* [Choose action](#choose-schema)
* [Form action](#form-schema)
* [Upload action](#upload-schema)
* [View document action](#view-document-schema)
* [Edit action](#edit-schema)
* [Follow link action](#follow-link-schema)
* [NOP action](#nop-schema)
* [HTTP action](#http-schema)

## Choose schema

`http://specs.livecontracts.io/draft-01/05-action/schema.json#choose`

This action allows the actor to pick one of the responses.

```json
{
  "$schema": "http://specs.livecontracts.io/draft-01/05-action/schema.json#choose",
  "actor": "client",
  "responses": {
    "approve": { "title": "Approve" },
    "reject": { "title": "Reject" }
  },
  "default_response": "approve"
}
```

## Form action

`http://specs.livecontracts.io/draft-01/05-action/schema.json#choose`

Display [a form](../08-form/) to the actor to fill out information.

```json
{
  "$schema": "http://specs.livecontracts.io/draft-01/05-action/schema.json#form",
  "actor": "client",
  "form": {
    "$schema": "http://specs.livecontracts.io/draft-01/08-form/schema.json#",
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

### form

A form definition.

## Upload action

`http://specs.livecontracts.io/draft-01/05-action/schema.json#upload`

Ask the actor to upload one or more files. The event body SHOULD be encoded as `multipart/form-data`.

```json
{
  "$schema": "http://specs.livecontracts.io/draft-01/05-action/schema.json#upload",
  "label": "Upload documents",
  "actor": "client",
  "accept": "image/*, application/pdf",
  "multiple": true,
  "update": {
    "select": "assets.documents"
  }
}
```

### accept

The type of files that are accepted, in MIME format. Use `*` as wildcard.

### multiple

Boolean flag to allow uploading multiple files at once.

## View document schema

`http://specs.livecontracts.io/draft-01/05-action/schema.json#view-document`

View or review a [document](../10-document/). Allow the user to pick one of the responses.

```json
{
  "$schema": "http://specs.livecontracts.io/draft-01/05-action/schema.json#view-document",
  "actor": "client",
  "document": {
    "$schema": "http://specs.livecontracts.io/draft-01/10-document/schema.json#",
    "id": "lt:/documents/7c403337-a4d6-47a6-89a6-eb989451dc06?v=8TffQS6U"
  },
  "responses": {
    "approve": { "title": "Approve" },
    "reject": { "title": "Reject" }
  },
  "default_response": "approve"
}
```

### document

A document definition.

## Edit document schema

`http://specs.livecontracts.io/draft-01/05-action/schema.json#edit-document`

Edit a [document](../10-document/). Also, allows the user to pick one of the responses.

```json
{
  "$schema": "http://specs.livecontracts.io/draft-01/05-action/schema.json#edit-document",
  "actor": "client",
  "document": {
    "$schema": "http://specs.livecontracts.io/draft-01/10-document/schema.json#",
    "id": "lt:/documents/7c403337-a4d6-47a6-89a6-eb989451dc06?v=8TffQS6U"
  }
}
```

### document

A document definition.

## Follow link schema

`http://specs.livecontracts.io/draft-01/05-action/schema.json#follow-link`

Have the user follow a hyperlink.

### url

The URL to go to.

## NOP schema

`http://specs.livecontracts.io/draft-01/05-action/schema.json#nop`

No operation (automated action).

```json
{
  "$schema": "http://specs.livecontracts.io/draft-01/05-action/schema.json#nop",
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

`http://specs.livecontracts.io/draft-01/05-action/schema.json#http`

Automated action to do a HTTP GET or POST request.

```json
{
  "$schema": "http://specs.livecontracts.io/draft-01/05-action/schema.json#http",
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
  "$schema": "http://specs.livecontracts.io/draft-01/05-action/schema.json#http",
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

### url

HTTP URL of the request.

### method

HTTP method of the request. Typically `GET` or `POST`.

### query

HTTP Query as object. This will be URL encoded.

### headers

HTTP headers as key/value pairs.

### auth

Either object with `username` and `password` or string for the `Authorization` header.

### data

HTTP POST body. This is typically a string. If the `Content-Type` is json, the data may be an object which will
automatically be encoded.

### requests

Array with for parallel requests. Each request object can have an `url`, `method`, `query`, `headers`, `auth` and `data`
property. For omitted properties, the property of the http action is used instead.
