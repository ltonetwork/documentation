# Live Contracts - Scenario

A Live Contract scenario is a definition of procedure as a Finite State Machine (FSM). As an FSM, the scenario can be
visualized as flowchart and instantiated as a process.

## Schemas

* [Scenario](#scenario-schema)
* [Action](#action-schema)
* [Response](#response-schema)
* [State](#state-schema)
* [Transition](#transition-schema)
* [Update instruction](#update-instruction-schema)

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

### Implicit states

Scenarios have a number of implicit states. Except for `:initial`, these don't need to be defined in the scenario. You
may define these not the less to set things like descriptions.

#### Initial state

The state with key `:initial` is the initial state of the FSM. This state is required, except for subscenarios.

The initial state should not have a default action. Instead the initial action is automatically determined, by finding
an action that the actor that instantiated the process can perform. This actions are checked in order.

#### Success state

The `:success` state is an end state indicating that the process has been completed successfully.

#### Failed state

The `:failed` state is an end state indicating that the process has not been completed successfully.

## Scenario schema

[JSON Schema](schema.json#)

### $schema

The Live Contracts Scenario [JSON schema](http://json-schema.org) URI that describes the JSON structure of the scenario.
To point to this version of the specification use `"$schema": "http://specs.livecontracts.io/draft-01/04-scenario/schema.json#"`.

### id

A URI as a unique identifier for the scenario. This is typically an [LTRI](../00-ltri/).

The id MUST point to an immutable version of the scenario. Modifying the scenario SHOULD always result in a new id.
Previous versions of the scenario SHOULD remain available.

### name

The name of the scenario. Shown when listing scenarios. The title SHOULD be unique within your set of scenarios.

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
      "title": "Employer",
      "properties": {
        "id": { "type": "string", "format": "uri" },
        "name": { "type": "string" }
      }
    },
    "employee": {
      "type": "object",
      "title": "Employee",
      "properties": {
        "id": { "type": "string", "format": "uri" },
        "name": { "type": "string" },
        "email": { "type": "string", "format": "email" }
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

## Action

An action is something that can be performed by actor or the node of an actor. An action may trigger a state transition
and / or may update the process projection.

### $schema

The action [JSON schema](http://json-schema.org) URI that describes the JSON structure of the action. This is also be
used for automation and may be used by the UI.

[10-action specs](../10-action/) contains a number of actions that SHOULD be
supported on any LegalThings One system.

### title

A short title for the action.

### label

Label that is shown when picking this action.

### description

A long description for the action that is shown when the action has been performed.

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

## response

Instructions for a response of an action.

### transition

Hard-wired transition for an action response to a specific state. This can be used for actions that always transition to
a specific state regardless of the state the process is now in.

The value must be the key of an action listed in the actions array.

### update

[Update instruction](#update-instruction-schema) or array of update instructions.

## state

The state a process that's instantiated from this scenario can be in.

### title

A short title for the state.

### description

A long description for the action that is shown when if the state is current or part of the golden flow.

### alt_descriptions

An alternative description that can be specific per actor. This is an object where the keys correspond with the actor
keys.

```json
{
  "employee": "Fill out this form",
  "employer": "Waiting for employee to fill out the form"
}
```

### actions

An array with the keys of all actions that may be performed in this state.

### default_action

The default action that is followed when determining the golden flow.

If no user interaction is required, the default action MAY be automatically executed by the system of the actor, without
the actor explictly choosing this action.

### transitions

Set of dynamic transitions from this state to the next.

### timeout

State timeout as date period. The format is a decimal and than

```
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

The timeout counts from transferring into the state until transferring out of the state. Actions that do not trigger a
state change does not affect the time out.

## State schema

## title

Short title of the state. This may be displayed in an overview of the process.

## description

A long description of the state.

## 

An object with instructions for the state per actor. These instructions may be show when this state is current. The
key of this object is an actor reference.

```json
{
  "employee": "Please accept the procedure",
  "employer": "Wait for the employee to accept the procedure or cancel"
}
```

## actions

A list of references of allowed [actions](../10-action/). Actions that are not allowed in the state SHOULD NOT be
executed. If such an action is executed, the result MUST NOT trigger a state change, nor change the process projection.

## default_action

The default action is used to determine the golden flow. Additionally, a system MAY execute this action without it being
explictly being invoked by the user, given it's a system action (may be signed using the system signkey).

## transitions

Specific transitions if an action is executed in this state. Transitions defined here overrule the transition defined in
the action response.

The [transition object](#transition-schema) can specify conditions 

## Update instruction schema


