---
description: Use response data and data instructions.
---

# A proper introduction

In the [previous tutorial](a-handshake.md) two actors greeted each other and could have a small conversation. In this tutorial we have a similar premise. The `initiator` is now at a conference, where he strikes a conversation introducing himself.

{% hint style="success" %}
Create a new subdirectory named `introduction`.
{% endhint %}

To keep it simple we're not modeling all kind of different responses in this tutorial. For that take a look at the ["A handshake" tutorial](a-handshake.md).

## Introduce yourself

Joe, the initiator, will introduce himself stating his name and organization. Jane will respond by introducing herself also stating her name and organization.

```text
Feature: Two actors meet at a conference and exchange information.

  Background:
    Given a chain is created by "Joe"
    Given "Joe" creates the "main" process using the "handshake" scenario
    And   "Joe" is the "initiator" actor of the "main" process
    And   "Jane" is the "recipient" actor of the "main" process

  Scenario:
    When "Joe" runs the "introduce_initiator" action of the "main" process with:
      | name         | Joe Smith     |
      | organization | LTO Network   |
    Then the "main" process is in the "wait_on_recipient" state
    
    When "Joe" runs the "introduce_recipient" action of the "main" process with:
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
    actor: initiator
  introduce_recipient:
    actor: recipient

states:
  initial:
    action: introduct_initiator
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
            "title": "Initiator",
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
        "introduce_initiator": {
            "actor": "initiator",
            "update": {
                "select": "actors.initiator"
            }
        },
        "introduce_recipient": {
            "actor": "recipient",
            "update": {
                "select": "actors.recipient"
            }
        }
    },
    "states": {
        ":initial": {
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

{% hint style="info" %}
The JSON schema for actors can become quite big. It that case, it's recommended to store it in a separate JSON file and use `$ref` with a URL to the file.
{% endhint %}

### Testing the actor properties

In the test, we want to see that the actors are assigned the correct properties after introducing themselves.

```text
Feature: Two actors meet at a conference and exchange information.

  Background:
    Given a chain is created by "Joe"
    Given "Joe" creates the "main" process using the "handshake" scenario
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
    
    When "Joe" runs the "introduce_recipient" action of the "main" process with:
      | name         | Jane Wong     |
      | organization | Acme Inc      |
    Then the "recipient" actor of the "main" process has:
      | name         | Jane Wong     |
      | organization | Acme Inc      |
    And the "main" process is completed
```

### Update instructions

The process doesn't automatically update the actor information. We need to supply update instructions for each action.

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
  introduce_initiator:
    actor: initiator
    update:
      select: actors.initiator
  introduce_recipient:
    actor: recipient
    update:
      select: actors.recipient

states:
  initial:
    action: introduct_initiator
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
    }
}
```
{% endtab %}
{% endtabs %}

