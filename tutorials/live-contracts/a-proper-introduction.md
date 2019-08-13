---
description: Use response data and data instructions.
---

# A proper introduction

In the [previous tutorial](a-handshake.md) two actors greeted each other and could have a small conversation. In this tutorial we have a similar premise. The _initiator_ is now at a conference, where he strikes a conversation introducing himself.

{% hint style="success" %}
Create a new subdirectory named `introduction`.
{% endhint %}

_To keep it simple we're not modeling all kind of different responses in this tutorial. For that take a look at the_ [_"A handshake" tutorial_](a-handshake.md)_._

## Introduce yourself

Joe, the initiator, will introduce himself stating his name and organization. Jane will respond by introducing herself also stating her name and organization.

```text
Feature: Two actors meet at a conference and exchange information.

  Background:
    Given a chain is created by "Joe"
    Given "Joe" creates the "main" process using the "introduction" scenario
    And   "Joe" is the "initiator" actor of the "main" process
    And   "Jane" is the "recipient" actor of the "main" process

  Scenario:
    When "Joe" runs the "introduce_initiator" action of the "main" process with:
      | name         | Joe Smith     |
      | organization | LTO Network   |
    Then the "main" process is in the "wait_on_recipient" state

    When "Jane" runs the "introduce_recipient" action of the "main" process with:
      | name         | Jane Wong     |
      | organization | Acme Inc      |
    Then the "main" process is completed
```

### The basic scenario

{% tabs %}
{% tab title="YAML" %}
```yaml
$schema: "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#"
title: A proper introduction

actors:
  initiator:
    title: Initiator
  recipient:
    title: Recipient

actions:
  introduce_initiator:
    title: Greet the person you're meeting
    actor: initiator
  introduce_recipient:
    title: Reply to the initiator
    actor: recipient

states:
  initial:
    action: introduce_initiator
    transition: wait_on_recipient
  wait_on_recipient:
    action: introduce_recipient
    transition: :success
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "$schema": "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#",
    "title": "A proper introduction",
    "actors": {
        "initiator": {
            "title": "Initiator"
        },
        "recipient": {
            "title": "Recipient"
        }
    },
    "actions": {
        "introduce_initiator": {
            "actor": "initiator"
        },
        "introduce_recipient": {
            "actor": "recipient"
        }
    },
    "states": {
        "initial": {
            "action": "introduce_initiator",
            "transition": "wait_on_recipient"
        },
        "wait_on_recipient": {
            "action": "introduce_recipient",
            "transition": ":success"
        }
    }
}
```
{% endtab %}
{% endtabs %}

## Using response data

### Defining the actors

The process is gathering information about both actors. We need to define the properties that the actors will have.

We define the actors in the scenario using [JSON schema](https://json-schema.org/). In the instantiated process these will become objects with the specified properties.

{% tabs %}
{% tab title="YAML" %}
```yaml
$schema: "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#"
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
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
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
    }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
The JSON schema for actors can become quite big. It that case, it's recommended to store it in a separate JSON file and use `$ref` with a URL to the file.
{% endhint %}

### Testing the actor properties

In the test, we want to see that the actors are assigned the correct properties after introducing themselves.

```text
Feature: Two actors meet at a conference and exchange information.

  Background:
    Given a chain is created by "Joe"
    Given "Joe" creates the "main" process using the "introduction" scenario
    And   "Joe" is the "initiator" actor of the "main" process
    And   "Jane" is the "recipient" actor of the "main" process

  Scenario:
    When "Joe" runs the "introduce_initiator" action of the "main" process with:
      | name         | Joe Smith     |
      | organization | LTO Network   |
    Then the "initiator" actor of the "main" process has:
      | name         | Joe Smith     |
      | organization | LTO Network   |
    And the "main" process is in the "wait_on_recipient" state

    When "Jane" runs the "introduce_recipient" action of the "main" process with:
      | name         | Jane Wong     |
      | organization | Acme Inc      |
    Then the "recipient" actor of the "main" process has:
      | name         | Jane Wong     |
      | organization | Acme Inc      |
    And the "main" process is completed
```

### Update instructions

The process doesn't automatically update the actor information. We need to supply update instructions for each action.

Adding an `update` property to an action or response will update the selected object with the data that's send by the client.

{% tabs %}
{% tab title="YAML" %}
```yaml
$schema: "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#"
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
  introduce_initiator:
    title: Greet the person you're meeting
    actor: initiator
    update: actors.initiator
  introduce_recipient:
    title: Reply to the initiator
    actor: recipient
    update: actors.recipient

states:
  initial:
    action: introduce_initiator
    transition: wait_on_recipient
  wait_on_recipient:
    action: introduce_recipient
    transition: :success
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
        "introduce_initiator": {
            "title": "Greet the person you're meeting",
            "actor": "initiator",
            "update": "actors.initiator"
        },
        "introduce_recipient": {
            "title": "Reply to the initiator",
            "actor": "recipient",
            "update": "actors.recipient"
        }
    },
    "states": {
        "initial": {
            "action": "introduce_initiator",
            "transition": "wait_on_recipient"
        },
        "wait_on_recipient": {
            "action": "introduce_recipient",
            "transition": ":success"
        }
    }
}
```
{% endtab %}
{% endtabs %}

## Using the actor's name

Once we know the name of the actor, we can use it in the process. Rather than saying "Reply to the initiator", we could say "Reply to Joe Smith".

The [`<tpl>` data instruction](../../full-node/workflow/scenario/data-instruction.md) parses a mustache template, injecting process data.

{% tabs %}
{% tab title="YAML" %}
```yaml
introduce_recipient:
  title: !tpl Reply to {{ actors.initiator }}
  actor: recipient
  update: actors.recipient
```
{% endtab %}

{% tab title="JSON" %}
```javascript
"introduce_recipient": {
    "title": {
        "<tpl>": "Reply to {{ actors.initiator }}"
    },
    "actor": "initiator",
    "update": "actors.initiator"
}
```
{% endtab %}
{% endtabs %}

## Now you!

We only get the name and organization of the recipient. To contact her, we'd also like to know her e-mail address.

In the process, she won't give it while introducing herself. Instead the initiator may ask the information after the introduction. If he doesn't ask, the conversation is over.

_To keep it simple, we'll assume she's always willing to give her e-mail address._

{% hint style="success" %}
1. Edit`Scenario` section of the `main.feature` test file. The process isn't completed after the introduction. Instead it's in the _wait on initiator_ state. The initiator will give run the _complete_ action to finish the process.
2. Add a second `Scenario` section. The first steps are the same as the original `Scenario`. On _wait on initiator_, the _initiator_ will _ask for email_.
3. The _recipient_ will _reply_ on email the request with her e-mail address. Her address is added to the actor information. This will complete the process.
4. Modify the scenario so the new test succeeds.
{% endhint %}

## More data next

Process data can be utilized for much more than just being displayed in titles and instructions. In the upcoming tutorials we'll see how data can alter the flow using conditions and how it can be used when communicating with externals system.

