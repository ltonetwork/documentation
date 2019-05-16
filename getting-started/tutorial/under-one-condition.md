---
description: Use conditions for actions and transitions.
---

# Under one condition

The [previous tutorial](a-proper-introduction.md) had our two actors introducing themselves. In that scenario we defined an action and a state for each actor. When scenarios becomes more complex and the number of actors increase, having to define actions and states per actor quickly adds up.

{% hint style="success" %}
Create a new subdirectory named `condition` and copy the files from `introduction`.
{% endhint %}

Since both actors need to perform the same action, we can reduce the scenario by creating a single _introduce_ action that can be performed by both actors.

```text
Feature: Two actors meet at a conference and exchange information.

  Background:
    Given a chain is created by "Joe"
    Given "Joe" creates the "main" process using the "handshake" scenario
    And   "Joe" is the "initiator" actor of the "main" process
    And   "Jane" is the "recipient" actor of the "main" process

  Scenario:
    When "Joe" runs the "introduce" action of the "main" process with:
      | name         | Joe Smith     |
      | organization | LTO Network   |
    Then the "initiator" actor of the "main" process has:
      | name         | Joe Smith     |
      | organization | LTO Network   |
    And the "main" process is in the "wait_on_recipient" state
    
    When "Jane" runs the "introduce" action of the "main" process with:
      | name         | Jane Wong     |
      | organization | Acme Inc      |
    Then the "recipient" actor of the "main" process has:
      | name         | Jane Wong     |
      | organization | Acme Inc      |
    And the "main" process is completed
```

## Multiple actors for an action

Allowing 2 actors to perform an action can be done by replacing the `actor` with a list of `actors`.

The update instruction needs to update the actor that performed the action. This actor can be selected using `current.actor`.

{% tabs %}
{% tab title="YAML" %}
```yaml
actions:
  introduce:
    actors:
      - initiator
      - recipient
    update: current.actor

states:
  initial:
    action: introduce
    transition: wait_on_recipient
  wait_on_recipient:
    action: introduce
    transition: :success
```
{% endtab %}

{% tab title="JSON" %}
```javascript
"actions": {
    "introduce": {
        "actors": [
            "initiator",
            "recipient"
        ],
        "update": "current.actor"
    }
},
"states": {
    "initial": {
        "action": "introduce",
        "transition": "wait_on_recipient"
    },
    "wait_on_recipient": {
        "action": "introduce",
        "transition": ":success"
    }
}
```
{% endtab %}
{% endtabs %}

This doesn't quite cut it though, because the initiator could now introduce himself twice to complete the process. That's not what we want.

## Conditional action

We'll add a second `Scenario` section to make sure a second introduction is ignored.

```text
Feature: Two actors meet at a conference and exchange information.

  Background:
    Given a chain is created by "Joe"
    Given "Joe" creates the "main" process using the "handshake" scenario
    And   "Joe" is the "initiator" actor of the "main" process
    And   "Jane" is the "recipient" actor of the "main" process

  Scenario:
    ...

  Scenario:
    When "Joe" runs the "introduce" action of the "main" process with:
      | name         | Joe Smith     |
      | organization | LTO Network   |
    Then the "initiator" actor of the "main" process has:
      | name         | Joe Smith     |
      | organization | LTO Network   |
    And the "main" process is in the "wait_on_recipient" state
    
    When "Joe" runs the "introduce" action of the "main" process with:
      | name         | Joe Smith     |
      | organization | LTO Network   |
    Then the "main" process is in the "wait_on_recipient" state
```

Running this test will fail, because the process is completed and not in the _wait on recipient_ state.

We'll change the scenario, so an actor can only introduce himself/herself if the actor's name is unknown.

We can add a `condition` to an action. The condition is always interpreted as boolean. With [data instructions](../../key-concepts/workflow-engine/scenario/data-instruction.md) like `<eval>`, the condition can be determined based on process data.

{% tabs %}
{% tab title="YAML" %}
```yaml
actions:
  introduce:
    actors:
      - initiator
      - recipient
    condition: !eval current.actor.name == null
    update: current.actor

states:
  initial:
    action: introduce
    transition: wait_on_recipient
  wait_on_recipient:
    action: introduce
    transition: :success
```
{% endtab %}

{% tab title="JSON" %}
```javascript
"actions": {
    "introduce": {
        "actors": [
            "initiator",
            "recipient"
        ],
        "condition": {
            "<eval>": "current.actor.name == null"
        },
        "update": "current.actor"
    }
},
"states": {
    "initial": {
        "action": "introduce",
        "transition": "wait_on_recipient"
    },
    "wait_on_recipient": {
        "action": "introduce",
        "transition": ":success"
    }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Data instructions can only use data that's available in the process, as they must be deterministic for all parties to have the same result.
{% endhint %}

{% hint style="warning" %}
`current.actor` is not available for the properties like the `title` \(action, response and state\) and state `instructions`. The actor isn't determined yet when these are calculated.
{% endhint %}

## Transition validation

An ill intended _Joe_ could still get around this by not specifying his name in he introduces himself. Since the name isn't set he can perform _introduce_ while in the _wait on recipient_ state.

We don't want the process to fail if _Joe_ doesn't give his name. Instead the process will stay in the _initial_ state until he does.

```text
Feature: Two actors meet at a conference and exchange information.

  Background:
    Given a chain is created by "Joe"
    Given "Joe" creates the "main" process using the "handshake" scenario
    And   "Joe" is the "initiator" actor of the "main" process
    And   "Jane" is the "recipient" actor of the "main" process

  Scenario:
    ...

  Scenario:
    ...

  Scenario:
    When "Joe" runs the "introduce" action of the "main" process with:
      | organization | LTO Network   |
    Then the "initiator" actor of the "main" process has:
      | name         |               |
      | organization | LTO Network   |
    And the "main" process is in the "initial" state
    
    When "Joe" runs the "introduce" action of the "main" process with:
      | nonsense     | chatter       |
    Then the "main" process is in the "initial" state
    
    When "Joe" runs the "introduce" action of the "main" process
    Then the "main" process is in the "initial" state
    
    When "Joe" runs the "introduce" action of the "main" process with:
      | name         | Joe Smith     |
    Then the "initiator" actor of the "main" process has:
      | name         | Joe Smith     |
      | organization | LTO Network   |
    And the "main" process is in the "wait_on_recipient" state
```

To achieve this, add a `condition` to the transition in the _initial_ state. Transition conditions are evaluated after the action is performed and updated instructions are processed.

_Jane_ is also be required to give her name.

{% tabs %}
{% tab title="YAML" %}
```yaml
actions:
  introduce:
    actors:
      - initiator
      - recipient
    condition: !eval current.actor.name == null
    update: current.actor

states:
  initial:
    action: introduce
    transitions:
      - transition: wait_on_recipient
        condition: !eval actors.initiator.name != null
  wait_on_recipient:
    action: introduce
    transitions:
      - transition: :success
        condition: !eval actors.recipient.name != null
```
{% endtab %}

{% tab title="JSON" %}
```javascript
"actions": {
    "introduce": {
        "actors": [
            "initiator",
            "recipient"
        ],
        "condition": {
            "<eval>": "current.actor.name == null"
        },
        "update": "current.actor"
    }
},
"states": {
    "initial": {
        "action": "introduce",
        "transitions": [
            {
                "transition": "wait_on_recipient",
                "condition": {
                    "<eval>": "actors.initiator.name != null"
                }
            }
        ]
    },
    "wait_on_recipient": {
        "action": "introduce",
        "transitions": [
            {
                "transition": ":success",
                "condition": {
                    "<eval>": "actors.recipient.name != null"
                }
            }
        ]
    }
}
```
{% endtab %}
{% endtabs %}

## Any order will do

According to the scenario, _Joe_ must first give his name and then _Jane_ should give her name. But the order isn't really important. Both actors should introduce themselves with their name. When that's done, the process is complete.

```text
Feature: Two actors meet at a conference and exchange information.

  Background:
    Given a chain is created by "Joe"
    Given "Joe" creates the "main" process using the "handshake" scenario
    And   "Joe" is the "initiator" actor of the "main" process
    And   "Jane" is the "recipient" actor of the "main" process

  Scenario:
    When "Jane" runs the "introduce" action of the "main" process with:
      | name         | Jane Wong     |
      | organization | Acme Inc      |
    Then the "recipient" actor of the "main" process has:
      | name         | Jane Wong     |
      | organization | Acme Inc      |
    And the "main" process is in the "initial" state

    When "Joe" runs the "introduce" action of the "main" process with:
      | name         | Joe Smith     |
      | organization | LTO Network   |
    Then the "initiator" actor of the "main" process has:
      | name         | Joe Smith     |
      | organization | LTO Network   |
    And the "main" process is in completed
```

The _introduce_ action can already be performed by both actors in the initial state and only be actor of whom the name is not know yet. We only need to combine the two states.

{% tabs %}
{% tab title="YAML" %}
```yaml
title: A proper introduction

actors:
  initiator:
    $schema: "http://json-schema.org/schema#"
    title: Initiator
    type: object
    properties:
      name:
        type: string
      organization:
        type: string
  recipient:
    $schema: "http://json-schema.org/schema#"
    title: Recipient
    type: object
    properties:
      name:
        type: string
      organization:
        type: string

actions:
  introduce:
    actors:
      - initiator
      - recipient
    condition: !eval current.actor.name == null
    update: current.actor

states:
  initial:
    action: introduce
    transitions:
      - transition: :success
        condition: !eval actors.initiator.name != null && actors.recipient.name != null
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "$schema": "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#",
    "title": "A proper introduction",
    "actors": {
        "initiator": {
            "$schema": "http://json-schema.org/schema#",
            "title": "Initiator",
            "type": "object",
            "properties": {
                "name": {
                    "type": "string"
                },
                "organization": {
                    "type": "string"
                }
            }
        },
        "recipient": {
            "$schema": "http://json-schema.org/schema#",
            "title": "Recipient"
            "type": "object",
            "properties": {
                "name": {
                    "type": "string"
                },
                "organization": {
                    "type": "string"
                }
            }
        }
    },
    "actions": {
        "introduce": {
            "actors": [
                "initiator",
                "recipient"
            ],
            "condition": {
                "<eval>": "current.actor.name == null"
            },
            "update": "current.actor"
        }
    },
    "states": {
        "initial": {
            "action": "introduce",
            "transitions": [
                {
                    "transition": ":success",
                    "condition": {
                        "<eval>": "actors.initiator.name != null && actors.recipient.name != null"
                    }
                }
            ]
        }
    }
}
```
{% endtab %}
{% endtabs %}

