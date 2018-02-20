# Live Contracts - Scenario

### Abstract

A Live Contract scenario is a definition of procedure as a Finite State Machine. As an FSM the scenario can be
visualized as flowchart and instantiated as a process.

### Note to Readers

The issues list for this draft can be found at <https://github.com/legalthings/livecontracts-specifications/issues>. For additional information, see <http://livecontracts.io/>.

To provide feedback, use this issue tracker, the communication methods listed on the homepage, or email the document
editors.

### Copyright Notice

Copyright (c) 2017 LegalThings One. All rights reserved.

You may use this specification under the [Creative Commons Attribution 4.0 International Public License](https://raw.githubusercontent.com/legalthings/livecontracts-specifications/master/LICENSE).

## Workflow

A workflow consists of an orchestrated and repeatable pattern of business activity.

A scenario is composed of a set of states and actions. An action can be executed by an actors which may trigger a
state change.

The scenario can be instantiated to create a process. A scenario is stateless whereas a process is stateful. Stateful
means that the computer or program keeps track of the state of interaction. Stateless means that there is no record of
previous interactions.

### Golden flow

Each state defines a default action and each action a default response. Following these defaults results in the golden
flow.

## Scenario

### $schema

The Live Contract Scenario [JSON schema](http://json-schema.org) URI that describes the JSON structure of the scenario.
To point to this version of the specification use `"$schema": "http://specs.livecontracts.io/draft-01/02-scenario/schema.json#"`.

### id

A URI as a unique identifier for the scenario. This is typically and [LTRI](http://specs.livecontracts.io/draft-01/01-/).

### title

A short description shown when listing scenarios. The title SHOULD be unique within your set of scenarios.

### description

A description of the procedure shown as scenario details.

### keywords

An array with alternative titles or keywords to search on.

### info

JSON schema of the process information. This information is typically shared when requisting a list of processes,
in contrary to the information about actors and assets.

```json
{
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
```

### actors

Object with JSON schema, defining the properties for each actor. The keys of the object is used to reference the actor.
The actor schema must define an object.

```json
{
  "actors": {
    "employer": {
      "type": "object",
      "properties": {
        "title": { "type": "string", "const": "Employer" },
        "company": { "type": "string" }
      }
    },
    "employee": {
      "type": "object",
      "properties": {
        "id": { "type": "string", "format": "uri" },
        "title": { "type": "string", "const": "Employee" },
        "first_name": { "type": "string" },
        "last_name": { "type": "string" }
      }
    }
  }
}
```

### assets

Object with JSON schema, defining the properties for each asset. The keys of the object is used to reference the asset.
The actor schema must define an object.

```json
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

### actions

Set of the actions of the scenario. The keys of the object is used to reference the action.

### states

Set of all states of the scenario. The keys of the object is used to reference the state.

#### Initial state

The state with key `:initial` is the initial state of the FSM. This state is required, except for subscenarios.

The initial state should not have a default action. Instead the initial action is automatically determined, by finding
an action that the actor that instantiated the process can perform. This actions are checked in order.

## Action

An action is something that can be performed by actor or the node of an actor. An action may trigger a state transition
and / or may update the process projection.


```json
{
  "actions": {
    "fill_out_form": {
        "$schema": ""
    }
  }
}
```

### $schema

The action [JSON schema](http://json-schema.org) URI that describes the JSON structure of the action. This is also be
used for automation and may be used by the UI.

### title

A short title for the action.

### label

Label that is shown when picking this action

### description

A long description for the action that is shown when the action is performed.

### actor

The reference of the actor that may perform this action. If more than one actor may perform the action, set an array of
actor references.

### responses

A set of possible responses for the action. The keys of the object is used to reference the response.

### default_response

The key of the default response. This is used for the golden flow.

### display

Should the action be displayed in the history? Choose one of the following options "always", "once" or "never". In the
case of "once", only the latest instance of the action will be displayed.
