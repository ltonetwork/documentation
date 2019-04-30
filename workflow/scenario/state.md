---
description: The state a process that's instantiated from this scenario can be in.
---

# State

## State scenario

All the possible states are described in the `states` section. Each state describes through which `action` it can transition to a new `state`

{% tabs %}
{% tab title="YAML" %}
```yaml
:initial:
    action: complete
    transitions:
      - response: ok
        transition: :success
      - response: cancel
        transition: :failed
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    ":initial": {
        "action": "complete"
        "transitions": [
            {
                "response": "ok",
                "transition": ":success"
            },
            {
                "response": "cancel",
                "transition": ":failed"
            }
        ]
    }
}
```
{% endtab %}
{% endtabs %}

or

{% tabs %}
{% tab title="YAML" %}
```yaml
:initial:
    actions: 
      - complete
      - next
    transitions:
      - action: complete
        transition: :success
      - action: next
        transition: next-state
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "initial": {
    "actions": [
      "complete",
      "next"
    ],
    "transitions": [
      {
        "action": "complete",
        "transition": "success"
      },
      {
        "action": "next",
        "transition": "next-state"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

## State Schema

`https://specs.livecontracts.io/v0.2.0/scenario/schema.json#state`

### title

A short title for the state.

### description

A long description for the action that is shown when if the state is current or part of the golden flow.

### instructions

Instructions that can be specific per actor. This is an object where the keys correspond with the actor keys.

{% tabs %}
{% tab title="YAML" %}
```yaml
instructions:
  employee: Fill out this form
  employer: Waiting for employee to fill out the form
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "instructions": {
    "employee": "Fill out this form",
    "employer": "Waiting for employee to fill out the form"
  }
}
```
{% endtab %}
{% endtabs %}

### actions

An array with the keys of all actions that may be performed in this state.

The order of the actions matter, especially for automated actions. The first available action is considered to be the default action. This action is followed when determining the golden flow.

If there is only one action available action, you may replace the `actions` property with an `action` property with a single action key as value.

### transitions

Dynamic [transitions](state.md#transition-schema) from this state to the next.

## Transition schema

`https://specs.livecontracts.io/v0.2.0/scenario/schema.json#transition`

A transition defines the change from one state to the next. State transition definitions are more dynamic than the transition you can set in the response object.

Transitions are evaluated in order. If multiple transitions apply, only the first one is used.

### action

Key of the action that must be performed for this transition to be selected.

### response

Key of the response for which this transition would be selected.

### condition

A boolean that must be true for the transition to be selected. This is typically a [data instruction](data-instruction.md).

### transition

Key of the state where to transition to.

