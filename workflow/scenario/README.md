---
description: >-
  A Live Contract scenario is a definition of procedure as a Finite State
  Machine (FSM). As an FSM, the scenario can be visualized as flowchart and
  instantiated as a process.
---

# Scenario

## Workflow

A workflow consists of an orchestrated and repeatable pattern of business activity.

A scenario is composed of a set of states and actions. An action can be executed by an actors which may trigger a state change.

The scenario can be instantiated to create a process. A scenario is stateless whereas a process is stateful. Stateful means that the computer or program keeps track of the state of interaction. Stateless means that there is no record of previous interactions.

{% hint style="info" %}
The scenario may be defined in YAML, which is better readable. The workflow service only accepts JSON, but the LTO client libraries can convert this.
{% endhint %}

### Golden flow

Each state MAY define a default action and each action MUST define a default response. Following these defaults results in the golden flow.

Omit a default action to wait for the timeout by default.

### Implicit states

Scenarios have a number of implicit states. Except for `:initial`, these don't need to be defined in the scenario. You may define these not the less to set things like descriptions.

**Initial state**

The state with key `:initial` is the initial state of the FSM.

**Success state**

The `:success` state is an end state indicating that the process has been completed successfully.

#### Cancelled state

The `:cancelled` state an end state indicating that the process has been cancelled by one of the actors.

**Failed state**

The `:failed` state is an end state indicating that the process has ended because of an error or other unexpected problem.

## Example

In this scenario a supplier provides a quotation for a client to accept. The process can be started either by the client or by the supplier.

If the client starts the process, the golden flow is

1. Client selects the supplier and describes the service he would like a quotation for
2. Client invites the supplier to participate in the process
3. Supplier uploads a quote \(due date depends on `urgency`\)
4. Client accepts the quote
5. _done_

If the supplier starts the process, the golden flow is

1. Supplier enters the client information
2. Supplier uploads a quote
3. Supplier invites the client to participate in the process
4. Client accepts the quote
5. _done_

{% tabs %}
{% tab title="YAML" %}
```yaml
$schema: "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#"
id: JKdf3jddfklsr834s312
title: Product quotation
description: Send a quotation requested by the client

actors:
  supplier:
    $ref: "https://workflow.example.com/actors/organization/schema.json#"
  client:
    $ref: "https://workflow.example.com/actors/individual/schema.json#"

assets:
  request:
    type: object
    properties:
      description:
        type: string
      urgency:
        type: string
        enum:
        - normal
        - high
        - critical
  quotation:
    type: object
    properties:
      title:
        type: string
      url:
        type: string

definitions:
  request_form:
    title: Quotation request form
    definition:
    - fields:
      - type: external-select
        label: Supplier
        name: supplier
        url: https://jsonplaceholder.typicode.com/users
        optionText: name
        optionValue: "{ name, email, phone }"
        required: true
      - type: textarea
        label: Description
        name: description
        helptip: Which service would you like a quotation for?
      - type: select
        label: Urgency
        name: urgency
        options:
        - value: normal
          label: Normal
        - value: high
          label: High
        - value: critical
          label: Critical
  client_form:
    title: Enter client information
    definition:
    - fields:
      - type: text
        label: Name
        name: name
        required: true
      - type: email
        label: E-mail
        name: email
        required: true

actions:
  request_quotation:
    $schema: "http://workflow.example.com/actions/form/schema.json#"
    actor: client
    title: Quote requested by client
    form: !ref definitions.request_form
    update:
      - select: assets.request
      - select: assets.supplier
        transform: supplier
  invite_supplier:
    $schema: "http://workflow.example.com/actions/invite/schema.json#"
    actor: client
    invite: supplier
    responses:
      ok:
        display: never
      error:
        display: always
        title: Failed to invite the supplier
        data: !ref response.data.message  
  enter_client:
    $schema: "http://workflow.example.com/actions/form/schema.json#"
    actor: supplier
    form: !ref definitions.client_form
    display: never
    update:
      select: actors.client
  invite_client:
    $schema: "http://workflow.example.com/actions/invite/schema.json#"
    actor: supplier
    invite: client
    responses:
      ok:
        display: never
      error:
        display: always
        title: Failed to invite the supplier
        description:
          "<ref>": response.data.message
  upload:
    $schema: "http://workflow.example.com/actions/upload/schema.json#"
    actor: supplier
    accept: application/pdf
    responses:
      ok:
        title: Provided a quotation
        update:
          select: assets.quotation
      error:
        label: An error occurred when uploading the quotation
        display: once
  review:
    $schema: "http://workflow.example.com/actions/review/schema.json#"
    actor: client
    document: !ref assets.quotation
    responses:
      accept:
        label: Accept
        title: Accepted quotation
      reject:
        label: Reject
        title: Rejected quotation
    default_response: accept

states:
  ":initial":
    actions:
    - request_quotation
    - enter_client
    transitions:
    - action: request_quotation
      transition: invite_supplier
    - action: enter_client
      transition: provide_quote
  invite_supplier:
    action: invite_supplier
    transitions:
    - response: ok
      transition: wait_for_quote
    - response: error
      transition: ":failed"
  invite_client:
    action: invite_client
    transitions:
    - response: ok
      transition: wait_for_review
  wait_for_quote:
    title: Prepare quotation
    instructions:
      supplier: !tpl "{{ assets.request.description }} ({{ assets.request.urgency }} urgency)"
    action: upload
    transitions:
    - response: ok
      transition: wait_for_review
  provide_quote:
    title: Provide quotation
    action: upload
    transitions:
    - response: ok
      transition: invite_client          
  wait_for_review:
    title: Review quotation
    instructions:
      client: Please review and accept the quotation
      supplier: Please wait for the client to review the quotation.
    action: review
    transitions:
    - response: accept
      transition: ":success"
    - response: reject
      transition: ":cancelled"

```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "$schema": "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#",
  "id": "JKdf3jddfklsr834s312",
  "title": "Product quotation",
  "description": "Send a quotation requested by the client",
  "actors": {
    "supplier": {
      "$ref": "https://specs.livecontracts.io/v0.2.0/actor/schema.json#organization"
    },
    "client": {
      "$ref": "https://specs.livecontracts.io/v0.2.0/actor/schema.json#individual"
    }
  },
  "assets": {
    "request": {
      "type": "object",
      "properties": {
        "description": {
          "type": "string"
        },
        "urgency": {
          "type": "string",
          "enum": [
            "normal",
            "high",
            "critical"
          ]
        }
      }
    },
    "quotation": {
      "type": "object",
      "properties": {
        "title": {
          "type": "string"
        },
        "url": {
          "type": "string"
        }
      }
    }
  },
  "definitions": {
    "request_form": {
      "title": "Quotation request form",
      "definition": [
        {
          "fields": [
            {
              "type": "external-select",
              "label": "Supplier",
              "name": "supplier",
              "url": "https://jsonplaceholder.typicode.com/users",
              "optionText": "name",
              "optionValue": "{ name, email, phone }",
              "required": true
            },
            {
              "type": "textarea",
              "label": "Description",
              "name": "description",
              "helptip": "Which service would you like a quotation for?"
            },
            {
              "type": "select",
              "label": "Urgency",
              "name": "urgency",
              "options": [
                {
                  "value": "normal",
                  "label": "Normal"
                },
                {
                  "value": "high",
                  "label": "High"
                },
                {
                  "value": "critical",
                  "label": "Critical"
                }
              ]
            }
          ]
        }
      ]
    },
    "client_form": {
      "title": "Enter client information",
      "definition": [
        {
          "fields": [
            {
              "type": "text",
              "label": "Name",
              "name": "name",
              "required": true
            },
            {
              "type": "email",
              "label": "E-mail",
              "name": "email",
              "required": true
            }
          ]
        }
      ]
    }
  },
  "actions": {
    "request_quotation": {
      "$schema": "http://workflow.example.com/actions/form/schema.json",
      "actor": "client",
      "title": "Quote requested by client",
      "form": { "<ref>": "definitions.request_form" },
      "update": [
        {
          "select": "assets.request"
        },
        {
          "select": "assets.supplier",
          "transform": "supplier"
        }
      ]
    },
    "invite_supplier": {
      "$schema": "http://workflow.example.com/actions/invite/schema.json",
      "actor": "client",
      "invite": "supplier",
      "responses": {
        "ok": {
          "display": "never"
        },
        "error": {
          "display": "always",
          "title": "Failed to invite the supplier",
          "data": "response.data.message"
        }
      }
    },
    "enter_client": {
      "$schema": "http://workflow.example.com/actions/form/schema.json",
      "actor": "supplier",
      "form": { "<ref>": "definitions.client_form" },
      "display": "never",
      "update": {
        "select": "actors.client"
      }
    },
    "invite_client": {
      "$schema": "http://workflow.example.com/actions/invite/schema.json",
      "actor": "supplier",
      "invite": "client",
      "responses": {
        "ok": {
          "display": "never"
        },
        "error": {
          "display": "always",
          "title": "Failed to invite the supplier",
          "description": {
            "<ref>": "response.data.message"
          }
        }
      }
    },
    "upload": {
      "$schema": "http://workflow.example.com/actions/upload/schema.json",
      "actor": "supplier",
      "accept": "application/pdf",
      "responses": {
        "ok": {
          "title": "Provided a quotation",
          "update": {
            "select": "assets.quotation"
          }
        },
        "error": {
          "label": "An error occurred when uploading the quotation",
          "display": "once"
        }
      }
    },
    "review": {
      "$schema": "http://workflow.example.com/actions/review/schema.json",
      "actor": "client",
      "document": "assets.quotation",
      "responses": {
        "accept": {
          "label": "Accept",
          "title": "Accepted quotation"
        },
        "reject": {
          "label": "Reject",
          "title": "Rejected quotation"
        }
      },
      "default_response": "accept"
    }
  },
  "states": {
    ":initial": {
      "actions": [
        "request_quotation",
        "enter_client"
      ],
      "transitions": [
        {
          "action": "request_quotation",
          "transition": "invite_supplier"
        },
        {
          "action": "enter_client",
          "transition": "provide_quote"
        }
      ]
    },
    "invite_supplier": {
      "action": "invite_supplier",
      "transitions": [
        {
          "response": "ok",
          "transition": "wait_for_quote"
        },
        {
          "response": "error",
          "transition": ":failed"
        }
      ]
    },
    "invite_client": {
      "action": "invite_client",
      "transitions": [
        {
          "response": "ok",
          "transition": "wait_for_review"
        }
      ]
    },
    "wait_for_quote": {
      "title": "Prepare quotation",
      "instructions": {
        "supplier": { "<tpl>": "{{ assets.request.description }} ({{ assets.request.urgency }} urgency)" }
      },
      "action": "upload",
      "transitions": [
        {
          "response": "ok",
          "transition": "wait_for_review"
        }
      ]
    },
    "provide_quote": {
      "title": "Provide quotation",
      "action": "upload",
      "transitions": [
        {
          "response": "ok",
          "transition": "invite_client"
        }
      ]
    },
    "wait_for_review": {
      "title": "Review quotation",
      "instructions": {
        "client": "Please review and accept the quotation",
        "supplier": "Please wait for the client to review the quotation."
      },
      "action": "review",
      "transitions": [
        {
          "response": "accept",
          "transition": ":success"
        },
        {
          "response": "reject",
          "transition": ":cancelled"
        }
      ]
    }
  }
}
```
{% endtab %}

{% tab title="JSON \(full\)" %}
```javascript
{
  "$schema": "https://specs.livecontracts.io/v0.1.0/scenario/schema.json#",
  "id": "JKdf3jddfklsr834s312",
  "title": "Product quotation",
  "description": "Send a quotation requested by the client",
  "actors": {
    "supplier": {
      "$ref": "https://specs.livecontracts.io/v0.1.0/actor/schema.json#organization"
    },
    "client": {
      "$ref": "https://specs.livecontracts.io/v0.1.0/actor/schema.json#individual"
    }
  },
  "assets": {
    "request": {
      "type": "object",
      "properties": {
        "description": {
          "type": "string"
        },
        "urgency": {
          "type": "string",
          "enum": ["normal", "high", "critical"]
        }
      }
    },
    "quotation": {
      "$ref": "https://specs.livecontracts.io/v0.1.0/document/schema.json#"
    }
  },
  "definitions": {
    "request_form": {
      "title": "Quotation request form",
      "definition": [{
        "fields": [{
            "$schema": "http://specs.legalthings.one/draft-01/form/schema.json#external-select",
            "label": "Supplier",
            "name": "supplier",
            "url": "https://jsonplaceholder.typicode.com/users",
            "optionText": "name",
            "optionValue": "{ name, email, phone }",
            "required": true
          },
          {
            "$schema": "http://specs.legalthings.one/draft-01/form/schema.json#textarea",
            "label": "Description",
            "name": "description",
            "helptip": "Which service would you like a quotation for?"
          },
          {
            "$schema": "http://specs.legalthings.one/draft-01/form/schema.json#select",
            "label": "Urgency",
            "name": "urgency",
            "options": [{
                "value": "normal",
                "label": "Normal"
              },
              {
                "value": "high",
                "label": "High"
              },
              {
                "value": "critical",
                "label": "Critical"
              }
            ]
          }
        ]
      }]
    },
    "client_form": {
      "title": "Enter client information",
      "definition": [{
        "fields": [{
            "$schema": "http://specs.legalthings.one/draft-01/form/schema.json#text",
            "label": "Name",
            "name": "name",
            "required": true
          },
          {
            "$schema": "http://specs.legalthings.one/draft-01/form/schema.json#email",
            "label": "E-mail",
            "name": "email",
            "required": true
          }
        ]
      }]
    }
  },
  "actions": {
    "request_quotation": {
      "$schema": "http://specs.legalthings.one/draft-01/action/schema.json#form-action",
      "actor": "client",
      "form": {
        "<ref>": "definitions.request_form"
      },
      "responses": {
        "ok": {
          "title": "Quote quested by client"
        }
      }
    },
    "invite_supplier": {
      "$schema": "http://specs.legalthings.one/draft-01/action/schema.json#invite-action",
      "actor": "client",
      "invite": "supplier",
      "responses": {
        "ok": {
          "display": "never"
        },
        "error": {
          "display": "always",
          "title": "Failed to invite the supplier",
          "description": {
            "<ref>": "response.data.message"
          }
        }
      }
    },
    "enter_client": {
      "$schema": "http://specs.legalthings.one/draft-01/scenario/schema.json#form-action",
      "actor": "supplier",
      "form": {
        "<ref>": "definitions.request_form"
      },
      "responses": {
        "ok": {
          "display": "never",
          "update": {
            "select": "actors.client"
          }
        }
      }
    },
    "invite_client": {
      "$schema": "http://specs.legalthings.one/draft-01/scenario/schema.json#invite-action",
      "actor": "supplier",
      "invite": "client",
      "responses": {
        "ok": {
          "display": "never"
        },
        "error": {
          "display": "always",
          "title": "Failed to invite the supplier",
          "description": {
            "<ref>": "response.data.message"
          }
        }
      }
    },
    "upload": {
      "$schema": "http://specs.legalthings.one/draft-01/scenario/schema.json#upload-action",
      "actor": "supplier",
      "label": "Upload",
      "accept": "application/pdf",
      "responses": {
        "ok": {
          "title": "Uploaded quotation",
          "update": {
            "select": "assets.quotation",
            "data": {
              "$schema": "https://specs.livecontracts.io/v0.1.0/document/schema.json#",
              "name": {
                "<tpl>": "Quotation {{ actors.client.name }} {{ response.date }}"
              },
              "date": {
                "<ref>": "response.date"
              },
              "content_media_type": {
                "<ref>": "response.data.media_type"
              },
              "content_encoding": {
                "<ref>": "response.data.encoding"
              },
              "content": {
                "<ref>": "response.data.content"
              }
            }
          }
        },
        "error": {
          "label": "An error occurred when uploading the quotation",
          "display": "once"
        }
      }
    },
    "review": {
      "$schema": "http://specs.legalthings.one/draft-01/action/schema.json#review",
      "actor": "client",
      "document": {
        "<ref>": "assets.quotation"
      },
      "responses": {
        "accept": {
          "label": "Accept",
          "title": "Accepted quotation"
        },
        "reject": {
          "label": "Reject",
          "title": "Rejected quotation"
        }
      },
      "default_response": "approve"
    },
    "cancel": {
      "$schema": "http://specs.legalthings.one/draft-01/action/schema.json#choose",
      "actor": ["client", "supplier"],
      "responses": {
        "ok": {
          "label": "Cancel",
          "title": "Quotation withdrawn",
          "transition": ":failed"
        }
      }
    }
  },
  "states": {
    ":initial": {
      "actions": ["request_quotation", "enter_client"],
      "transitions": [{
          "action": "request_quotation",
          "transition": "invite_supplier"
        },
        {
          "action": "enter_client",
          "transition": "provide_quote"
        }
      ]
    },
    "invite_supplier": {
      "title": "Waiting on the supplier to participate in this process",
      "actions": ["invite_supplier", "cancel"],
      "default_action": "invite_supplier",
      "transitions": [{
        "action": "invite_supplier",
        "response": "ok",
        "transition": "wait_for_quote"
      }]
    },
    "provide_quote": {
      "title": "Prepare quotation",
      "actions": ["upload", "cancel"],
      "default_action": "upload",
      "transitions": [{
        "action": "upload",
        "response": "ok",
        "transition": "invite_client"
      }]
    },
    "invite_client": {
      "title": "Waiting on the client to participate in this process",
      "actions": ["invite_client", "cancel"],
      "default_action": "invite_client",
      "transitions": [{
        "action": "invite_client",
        "response": "ok",
        "transition": "wait_for_review"
      }]
    },
    "wait_for_quote": {
      "title": "Prepare quotation",
      "instructions": {
        "supplier": {
          "<tpl>": "{{ assets.request.description }} ({{ assets.request.urgency }} urgency)"
        }
      },
      "actions": ["upload", "cancel"],
      "default_action": "upload",
      "transitions": [{
        "action": "upload",
        "response": "ok",
        "transition": "wait_for_review"
      }],
      "timeout": {
        "<switch>": {
          "on": {
            "<ref>": "assets.request.urgency"
          },
          "options": {
            "normal": "3b",
            "high": "1b",
            "critical": "6h"
          }
        }
      }
    },
    "wait_for_review": {
      "title": "Review quotation",
      "instructions": {
        "client": "Please review and accept the quotation",
        "supplier": "Please wait for the client to review the quotation."
      },
      "actions": ["review", "cancel"],
      "default_action": "review",
      "timeout": "7d",
      "transitions": [{
          "action": "review",
          "response": "accept",
          "transition": ":success"
        },
        {
          "action": "review",
          "response": "deny",
          "transition": ":failed"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Scenario schema

`https://specs.livecontracts.io/v0.2.0/scenario/schema.json#`

### $schema

The Live Contracts Scenario [JSON schema](http://json-schema.org) URI that describes the JSON structure of the scenario.

### id

A unique identifier. If the scenario is part of a Live Contract, this must be resource id that's valid for the event chain.

The id MUST point to an immutable version of the scenario. Modifying the scenario SHOULD always result in a new id. Previous versions of the scenario SHOULD remain available.

### title

The title of the scenario.

### description

A description of the procedure shown as scenario details.

### actors

Object with JSON Schema, defining the properties for each actor. The keys of the object is used to reference the actor. The actor schema must define an object.

[Read more](actor.md)

### assets

Object with JSON schema, defining the properties for each asset. The keys of the object is used to reference the asset. The asset schema must define an object.

[Read more](asset.md)

### definitions

An object with constant values and predefined objects. This can be used to define things needed in multiple actions and/or states.

Even if it's only used in a single action or state, using definitions helps to keep actions and states small and readable.

[Read more](asset.md#scenario-definition)

### actions

Set of the actions of the scenario. The keys of the object are used as reference of the action.

[Read more](action.md)

### states

Set of all states of the scenario. The keys of the object are used as reference of the state.

[Read more](state.md)

