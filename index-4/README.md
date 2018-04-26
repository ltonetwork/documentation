# index

[← back](../)

## Scenario

A Live Contract scenario is a definition of procedure as a Finite State Machine \(FSM\). As an FSM, the scenario can be visualized as flowchart and instantiated as a process.

### Schemas

[JSON Schema](https://github.com/legalthings/livecontracts-specs/tree/1f2cef267dfdf6fb694c3f8e878eb0af9a5cc284/04-scenario/schema.json) - [http://specs.livecontracts.io/draft-01/04-scenario/schema.json\#](http://specs.livecontracts.io/draft-01/04-scenario/schema.json#)

* [Scenario](./#scenario-schema)
* [Action](./#action-schema)
* [Response](./#response-schema)
* [State](./#state-schema)
* [Transition](./#transition-schema)
* [Update instruction](./#update-instruction-schema)

### Workflow

A workflow consists of an orchestrated and repeatable pattern of business activity.

A scenario is composed of a set of states and actions. An action can be executed by an actors which may trigger a state change.

The scenario can be instantiated to create a process. A scenario is stateless whereas a process is stateful. Stateful means that the computer or program keeps track of the state of interaction. Stateless means that there is no record of previous interactions.

#### Golden flow

Each state MAY define a default action and each action MUST define a default response. Following these defaults results in the golden flow.

Omit a default action to wait for the timeout by default.

#### Implicit states

Scenarios have a number of implicit states. Except for `:initial`, these don't need to be defined in the scenario. You may define these not the less to set things like descriptions.

**Initial state**

The state with key `:initial` is the initial state of the FSM.

The initial state should not have a default action. Instead the initial action is automatically determined, by finding an action that the actor that instantiated the process can perform. This actions are checked in order.

**Success state**

The `:success` state is an end state indicating that the process has been completed successfully.

**Failed state**

The `:failed` state is an end state indicating that the process has not been completed successfully.

### Example

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

Both the client and the supplier can cancel the process at any time.

```javascript
{
  "$schema": "http://specs.livecontracts.io/draft-01/04-scenario/schema.json#",
  "id": "lt:/scenarios/fe659ffa-537d-461a-abd7-aa0f3643d5ee",
  "title": "Accept quotation",
  "description": "Accept or reject a quotation",
  "actors": {
    "supplier": {
      "$ref": "http://specs.livecontracts.io/draft-01/06-actor/schema.json#organization"
    },
    "client": {
      "$ref": "http://specs.livecontracts.io/draft-01/06-actor/schema.json#individual"
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
          "enum": [ "normal", "high", "critical" ]
        }
      }
    }
    "quotation": { "$ref": "http://specs.livecontracts.io/draft-01/10-document/schema.json#" }
  },
  "definitions": {
    "request_form": {
      "title": "Quotation request form",
      "definition": [
        {
          "fields": [
            {
              "$schema": "http://specs.legalthings.one/draft-01/08-form/schema.json#external-select",
              "label": "Supplier",
              "name": "supplier",
              "url": "https://jsonplaceholder.typicode.com/users",
              "optionText": "name",
              "optionValue": "{ name, email, phone }",
              "required": true
            },
            {
              "$schema": "http://specs.legalthings.one/draft-01/08-form/schema.json#textarea",
              "label": "Description",
              "name": "description",
              "helptip": "Which service would you like a quotation for?"
            },
            {
              "$schema": "http://specs.legalthings.one/draft-01/08-form/schema.json#select",
              "label": "Urgency",
              "name": "urgency",
              "options": [
                { "value": "normal", "label": "Normal" },
                { "value": "high", "label": "High" },
                { "value": "critical", "label": "Critical" }
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
              "$schema": "http://specs.legalthings.one/draft-01/08-form/schema.json#text",
              "label": "Name",
              "name": "name",
              "required": true
            },
            {
              "$schema": "http://specs.legalthings.one/draft-01/08-form/schema.json#email",
              "label": "E-mail",
              "name": "email",
              "required": true
            }
          ]
        }
      ]
    }
  }
  "actions": {
    "request_quotation": {
      "$schema": "http://specs.legalthings.one/draft-01/05-action/schema.json#form-action",
      "actor": "client",
      "form": { "<ref>": "definitions.request_form" },
      "responses": {
        "ok": {
          "title": "Quote quested by client"
        }
      }
    },
    "invite_supplier": {
      "$schema": "http://specs.legalthings.one/draft-01/05-action/schema.json#invite-action",
      "actor": "client",
      "invite": "supplier",
      "responses": {
        "ok": {
          "display": "never"
        },
        "error": {
          "display": "always",
          "title": "Failed to invite the supplier",
          "description": { "response.data.message" }
        }
      }
    },
    "enter_client": {
      "$schema": "http://specs.legalthings.one/draft-01/04-scenario/schema.json#form-action",
      "actor": "supplier",
      "form": { "<ref>": "definitions.request_form" },
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
      "$schema": "http://specs.legalthings.one/draft-01/04-scenario/schema.json#invite-action",
      "actor": "supplier",
      "invite": "client",
      "responses": {
        "ok": {
          "display": "never"
        },
        "error": {
          "display": "always",
          "title": "Failed to invite the supplier",
          "description": { "response.data.message" }
        }
      }
    },
    "upload": {
      "$schema": "http://specs.legalthings.one/draft-01/04-scenario/schema.json#upload-action",
      "actor": "supplier",
      "label": "Upload",
      "accept": "application/pdf",
      "responses": {
        "ok": {
          "title": "Uploaded quotation",
          "update": {
            "select": "assets.quotation",
            "data": {
              "$schema": "http://specs.livecontracts.io/draft-01/10-document/schema.json#",
              "name": { "<tpl>": "Quotation {{ actors.client.name }} {{ response.date }}" },
              "date": { "<ref>": "response.date" },
              "content_media_type": { "<ref>": "response.data.media_type" },
              "content_encoding": { "<ref>": "response.data.encoding" },
              "content": { "<ref>": "response.data.content" }
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
      "$schema": "http://specs.legalthings.one/draft-01/05-action/schema.json#review",
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
      "$schema": "http://specs.legalthings.one/draft-01/05-action/schema.json#choose",
      "actor": [ "client", "supplier" ],
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
      "actions": [ "request_quotation", "enter_client" ],
      "transitions": [
        {
          "action": "request_quotation",
          "transition": "invite_supplier"
        },
        {
          "action": "enter_client",
          "transition": "provide_quote"
        },
      ]
    },
    "invite_supplier": {
      "title": "Waiting on the supplier to participate in this process",
      "actions": [ "invite_supplier", "cancel" ],
      "default_action": "invite_supplier",
      "transitions": [
        {
          "action": "invite_supplier",
          "response": "ok",
          "transition": "wait_for_quote"
        }
      ]
    },
    "provide_quote": {
      "title": "Prepare quotation",
      "actions": [ "upload", "cancel" ],
      "default_action": "upload",
      "transitions": [
        {
          "action": "upload",
          "response": "ok",
          "transition": "invite_client"
        }
      ],
    },
    "invite_client": {
      "title": "Waiting on the client to participate in this process",
      "actions": [ "invite_client", "cancel" ],
      "default_action": "invite_client",
      "transitions": [
        {
          "action": "invite_client",
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
      "actions": [ "upload", "cancel" ],
      "default_action": "upload",
      "transitions": [
        {
          "action": "upload",
          "response": "ok",
          "transition": "wait_for_review"
        }
      ],
      "timeout": {
        "<switch>": {
          "on": { "<ref>": "assets.request.urgency" },
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
      "actions": [ "review", "cancel" ],
      "default_action": "review",
      "timeout": "7d",
      "transitions": [
        {
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

### Scenario schema

`http://specs.livecontracts.io/draft-01/04-scenario/schema.json#`

#### $schema

The Live Contracts Scenario [JSON schema](http://json-schema.org) URI that describes the JSON structure of the scenario.

#### id

A URI as a unique identifier for the scenario. This is typically an [LTRI](https://github.com/legalthings/livecontracts-specs/tree/1f2cef267dfdf6fb694c3f8e878eb0af9a5cc284/00-ltri/README.md).

The id MUST point to an immutable version of the scenario. Modifying the scenario SHOULD always result in a new id. Previous versions of the scenario SHOULD remain available.

#### name

The name of the scenario. Shown when listing scenarios. The name SHOULD be unique within your set of scenarios.

#### description

A description of the procedure shown as scenario details.

#### keywords

An array with alternative titles or keywords to search on.

#### info

JSON schema of the process information. This information is typically shared when requisting a list of processes, in contrary to the information about actors and assets.

```javascript
{
  "info": {
    "type": "object",
    "properties": {
      "involved": {
        "type": "array",
        "items": { "type": "string" }
      },
      "status": {
        "type": "string",
        "enum": [ "ready", "waiting", "completed" ]
      }
    }
  }
}
```

#### actors

Object with JSON schema, defining the properties for each actor. The keys of the object is used to reference the actor. The actor schema must define an object.

An actor SHOULD at least an `id` property to link it to an identity and a `name` property.

If the actor represents multiple identities, it SHOULD have a `members` property with objects that have an `id` and `name` property.

```javascript
{
  "actors": {
    "employer": {
      "type": "object",
      "title": "Employer",
      "properties": {
        "id": { "type": "string", "format": "uri" },
        "name": { "type": "string" }
      }
    },
    "employee": {
      "$ref": "http://specs.livecontracts.io/draft-01/06-actor/schema.json#employee"
    },
    "team": {
      "type": "object",
      "title": "Team",
      "properties": {
        "members": {
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "id": { "type": "string", "format": "uri" },
              "name": { "type": "string" },
              "email": { "type": "string", "format": "email" }
            }
          }
        }
      }
    }
  }
}
```

You MAY define your own actor types inline. For interoperability it's recommended to refer to a `$schema` instead.

#### assets

Object with JSON schema, defining the properties for each asset. The keys of the object is used to reference the asset. The asset schema must define an object.

```javascript
{
  "assets": {
    "contract": {
      "properties": {
        "id": { "type": "string", "format": "uri" },
        "name": { "type": "string" }
      },
      "readOnly": true
    },
    "book": {
      "properties": {
        "id": { "type": "string", "format": "uri" },
        "title": { "type": "string" },
        "isbn": { "type": "string", "pattern": "^\\d{13}$" }
      }
    }
  }
}
```

#### definitions

An object with constant values and predefined objects. This can be used to define things needed in multiple actions and/or states.

Even if it's only used in a single action or state, using definitions helps to keep actions and states small and readable.

#### actions

Set of the actions of the scenario. The keys of the object are used as reference of the action.

#### states

Set of all states of the scenario. The keys of the object are used as reference of the state.

### Action

`http://specs.livecontracts.io/draft-01/04-scenario/schema.json#action`

An action is something that can be performed by actor or the node of an actor. An action may trigger a state transition and / or may update the process projection.

#### $schema

The action [JSON schema](http://json-schema.org) URI that describes the JSON structure of the action. This is also be used for automation and may be used by the UI.

[10-action specs](https://github.com/legalthings/livecontracts-specs/tree/1f2cef267dfdf6fb694c3f8e878eb0af9a5cc284/10-action/README.md) contains a number of actions that SHOULD be supported on any LegalThings One system.

#### title

A short title for the action.

#### label

Label that is shown when picking this action.

#### description

A long description for the action that is shown when the action has been performed.

#### actor

The reference of the actor that may perform this action. If more than one actor may perform the action, set an array of actor references.

#### responses

A set of possible responses for the action. The keys of the object is used to reference the response.

#### default\_response

The key of the default response. This is used for the golden flow.

#### display

Should the action be displayed in the history? Choose one of the following options "always", "once" or "never". In the case of "once", only the latest instance of the action will be displayed.

### Response schema

`http://specs.livecontracts.io/draft-01/04-scenario/schema.json#response`

Instructions for a response of an action.

#### label

The label that will be displayed in the process if the action is performed.

#### display

Property to determine if the response should be shown in the history \(`prev`\) of the process. Options

* never - Never show the response
* once - If there are multiple successive responses of the same action, only show the latest
* always - Always show all the response

#### transition

Hard-wired transition for an action response to a specific state. This can be used for actions that always transition to a specific state regardless of the state the process is now in.

The value must be the key of an action listed in the actions array.

#### update

[Update instruction](./#update-instruction-schema) or array of update instructions.

### Update instruction schema

`http://specs.livecontracts.io/draft-01/04-scenario/schema.json#update-instruction`

After a response is given, the projection of the process may be updated. Update instructions can update the process information, assets or actors.

### set

A reference to the item in the process that should be updated. This MUST be a property of `info`, `assets` or `actors`.

This can't be a full JMESPath expression, instead it should be it's a simplified notation using the object and array notation \(eg `assets.stock.items[3]`\).

### data

The data to which the items should be updated. By default, this is the `data` of the response.

It's typically useful to use a [data instructions](https://github.com/legalthings/livecontracts-specs/tree/1f2cef267dfdf6fb694c3f8e878eb0af9a5cc284/07-data-instruction/README.md) for data. While processing a response, the process has an additional property `response` which may be referenced.

```javascript
{
  "select": "assets.document",
  "data": {
    "id": { "<tpl>": "lt:/documents/{{ response.data.id }}" },
    "content": { "<ref>": "response.data.body" },
    "content_media_type": "text/html"
  }
}
```

### State schema

`http://specs.livecontracts.io/draft-01/04-scenario/schema.json#state`

The state a process that's instantiated from this scenario can be in.

#### title

A short title for the state.

#### description

A long description for the action that is shown when if the state is current or part of the golden flow.

#### instructions

Instructions that can be specific per actor. This is an object where the keys correspond with the actor keys.

```javascript
{
  "employee": "Fill out this form",
  "employer": "Waiting for employee to fill out the form"
}
```

#### actions

An array with the keys of all actions that may be performed in this state.

#### default\_action

The default action that is followed when determining the golden flow.

If no user interaction is required, the default action MAY be automatically executed by the system of the actor, without the actor explictly choosing this action.

Omit the default action to have the state wait on the timeout by default.

#### transitions

Set of dynamic transitions from this state to the next.

#### timeout

State timeout as date period. The format is a decimal and than

```text
y year
m month
d day
b business day (mon-fri, including holidays)
w week
h hour
i minute
s second
```

These can be combined, for instance `3b12h` means 3 business days and 12 hours.

The timeout counts from transferring into the state until transferring out of the state. Actions that do not trigger a state change does not affect the time out.

By default no state change occurs on a timeout. Add a transition with no action selected and `:timeout` selected as response to specify a state transition in case of a timeout.

### Transition

`http://specs.livecontracts.io/draft-01/04-scenario/schema.json#transition`

A transition defines the change from one state to the next. State transition definitions are more dynamic than the transition you can set in the response object.

Transitions are evaluated in order. If multiple transitions apply, only the first one is used.

#### action

Key of the action that must be performed for this transition to be selected.

#### response

Key of the response for which this transition would be selected.

#### condition

An boolean that must be true for the transition to be selected. This is typically a [data instruction](https://github.com/legalthings/livecontracts-specs/tree/1f2cef267dfdf6fb694c3f8e878eb0af9a5cc284/07-data-instruction/README.md).

#### transition

Key of the state where to transition to.

