# Live Contracts Scenario: A stateless definition of a procedure

## Abstract

A Live Contract scenario is a definition of procedure as a Finite State Machine. As an FSM the scenario can be
visualized as flowchart and instantiated as a process.

## Note to Readers

The issues list for this draft can be found at <https://github.com/legalthings/livecontracts-specifications/issues>.

For additional information, see <http://livecontracts.io/>.

To provide feedback, use this issue tracker, the communication methods listed on the homepage, or email the document
editors.

## Copyright Notice

Copyright (c) 2017 LegalThings One. All rights reserved.

You may use this specification under the [Creative Commons Attribution 4.0 International Public License](https://raw.githubusercontent.com/legalthings/livecontracts-specifications/master/LICENSE).

## Schema definition

### $schema

The Live Contract Scenario [JSON schema](http://json-schema.org) that describes the JSON structure of the scenario.
To point to this version of the specification use `"$schema": "https://livecontracts.io/draft-01/02-scenario/schema#"`.

### id

A URI as a unique identifier for the scenario.

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

### states

Set of all states of the scenario.

### actions

Set of all possible actions of the scenario.

