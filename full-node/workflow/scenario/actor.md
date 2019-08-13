---
description: >-
  An actor is an identity that participates on a process. The actors are defined
  in a scenario as JSON Schema and instantiated for the process.
---

# Actor

## Scenario actor

Items in the `actors` property of the **scenario** aren't actor objects, but JSON schemas defining the properties available for the actor once instantiated in the process.

The custom schema for actors should extend the basic actor schema, which means it must at least have a `title` and `identity` and property.

{% tabs %}
{% tab title="YAML" %}
```yaml
actors:
  employer:
    type: object
    title: Employer
    properties:
      title:
        type: string
      name:
        type: string
      email:
        type: string
        format: email
      identity:
        $ref: "https://specs.livecontracts.io/v0.2.0/identity/schema.json#"
  employee:
    $ref: "https://specs.livecontracts.io/v0.2.0/actor/schema.json#employee"
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "actors": {
    "employer": {
      "type": "object",
      "title": "Employer",
      "properties": {
        "name": {
          "type": "string"
        },
        "identity": {
          "$ref": "https://specs.livecontracts.io/v0.2.0/identity/schema.json#"
        }        
      }
    },
    "employee": {
      "$ref": "https://workflow.example.com/actors/employee/schema.json#"
    }
  }
}
```
{% endtab %}
{% endtabs %}

Properties that aren't defined don't exist and can't be set, unless `additionalProperties` is set in the actor definition.

## Actor schema

`https://specs.livecontracts.io/v0.2.0/actor/schema.json#`

### $schema

The Live Contracts Scenario [JSON schema](http://json-schema.org) URI that describes the JSON structure of the scenario.

### title

The title of the actor. This is copied from the actor definition in the scenario.

### identity

The identity links a person or organization to an actor.

[See Identity specification]()

{% hint style="info" %}
If another party takes over a role, the `identity` property of the actor in the process should change and not the keys of the identity. For instance if there is a change in supplier in the process, don't update the identity keys, but add an identity and switch the identity linked to the actor.
{% endhint %}

