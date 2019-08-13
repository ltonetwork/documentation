---
description: Use conditions for actions and transitions.
---

# Under one condition

The [previous tutorial](a-proper-introduction.md) had our two actors introducing themselves. In that scenario we defined an action and a state for each actor. When scenarios becomes more complex and the number of actors increase, having to define actions and states per actor quickly adds up.

{% hint style="success" %}
Create a new subdirectory named `condition` .
{% endhint %}

Since both actors need to perform the same action, we can reduce the scenario by creating a single _introduce_ action that can be performed by both actors.

```text
Feature: Two actors meet at a conference and exchange information.

  Background:
    Given a chain is created by "Joe"
    Given "Joe" creates the "main" process using the "condition" scenario
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

### Multiple actors for an action

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

We can add a `condition` to an action. The condition is always interpreted as boolean. With [data instructions](../../full-node/workflow/scenario/data-instruction.md) like `<eval>`, the condition can be determined based on process data.

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

An ill intended _Joe_ could still get around this by not specifying his name when he introduces himself. Since the name isn't set he can perform _introduce_ while in the _wait on recipient_ state.

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
    And the "main" process is completed
```

The _introduce_ action can already be performed by both actors in the initial state and only by actor of whom the name is not know yet. We only need to combine the two states.

{% tabs %}
{% tab title="YAML" %}
```yaml
$schema: "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#"
title: Under one condition

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
            "title": "Recipient",
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

## Now you!

In the ["A proper introduction" tutorial](a-proper-introduction.md), you modified the scenario adding steps for asking the e-mail address from the _recipient_.

In that scenario, we only want to only consider the process to successfully completed if the _recipient_ has given her e-mail address. If the ini_t\_iator ends the conversation or if the \_recipient_ doesn't give her e-mail address, the process is _cancelled_.

Rather than making an separate action or response for the recipient to skip not give the e-mail address, use a condition to check to transition to ":success" or ":cancelled".

{% hint style="success" %}
1. Copy the introduction scenario and test file you modified as the previous assignment.
2. Edit main.feature, specifying that the process is _cancelled_ if it's completed by the _initiator_.
3. Add a third `Scenario` section, copying the second. Change the last action so the _reply_ of _recipient_ on the request doesn't have any data. In this case the process should also be cancelled.
4. Modify the scenario so the new test succeeds.
{% endhint %}

