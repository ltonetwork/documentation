---
description: Setup a node and run your first Live Contract.
---

# Live Contracts

## Installation

Running Live Contracts requires using the LTO Network workflow node.

In these tutorials, we're always using the developer build of the workflow node, which is available as a Docker container.

```
curl "https://raw.githubusercontent.com/ltonetwork/lto-workflow-node/docker-compose.yml" -o docker-compose.yml
docker-compose up
```

{% hint style="info" %}
If you need more help to install Docker and the workflow node, please follow the [step-by-step installation guide](broken-reference).
{% endhint %}

{% tabs %}
{% tab title="..." %}
To check that the node is correctly up visit [https://localhost:3000](https://localhost:3000). You'll see a JSON response containing information about the LTO services.
{% endtab %}

{% tab title="JSON screenshot" %}
![](../.gitbook/assets/screenshot-localhost-3000-2019.05.06-15-19-12.png)


{% endtab %}
{% endtabs %}

### Project directory structure

{% hint style="success" %}
Create a directory named `livecontracts-tutorial`.
{% endhint %}

```
mkdir -p livecontracts-tutorial
cd livecontracts-tutorial
```

## Creating a scenario

A Live Contract must contain one or more scenarios that describe the process(es) we're automating. We can write a scenario in either JSON or YAML.

{% hint style="success" %}
Create file `scenario.yml` (or `scenario.json`) in the `basic` directory.
{% endhint %}

{% tabs %}
{% tab title="YAML" %}
```yaml
$schema: "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#"
title: The basics
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "$schema": "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#",
    "title": "The basics"
}
```
{% endtab %}
{% endtabs %}

### Defining actors

Actors are organizations or individuals that play a role in the process. The scenario needs to define which actors (may) exist.

{% tabs %}
{% tab title="YAML" %}
```yaml
$schema: "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#"
title: The basics

actors:
  initiator:
    title: Initiator
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "$schema": "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#",
    "title": "The basics",
    "actors": {
        "initiator": {
            "title": "Initiator"
        }
    }
}
```
{% endtab %}
{% endtabs %}

We've defined a single actor for the process; the _initiator_. Normally a process contains 2 or more actors.

{% hint style="info" %}
The key `initiator` is used to reference the actor. The title must be defined, but only exists for displaying purposes.
{% endhint %}

### Defining actions

All actions that any actor can perform within the process must be defined at forehand in the scenario.

{% tabs %}
{% tab title="YAML" %}
```yaml
$schema: "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#"
title: My first Live Contract

actors:
  initiator:
    title: Initiator
    
actions:
  complete:
    title: Complete the process
    actor: initiator
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "$schema": "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#",
    "title": "The basics",
    "actors": {
        "initiator": {
            "title": "Initiator"
        }
    },
    "actions": {
        "complete": {
            "title": "Complete the process",
            "actor": "initiator"
        }
    }
}
```
{% endtab %}
{% endtabs %}

The _complete_ action can be performed by the _initiator_ actor. The intend of this action is to complete the process. However, that transition needs to be defined in the state.

### The initial state

When a process is started, it's in the _initial_ state. From this state it can transition to other states, until the process is completed.

{% tabs %}
{% tab title="YAML" %}
```yaml
$schema: "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#"
title: My first Live Contract

actors:
  initiator:
    title: Initiator
    
actions:
  complete:
    title: Complete the process
    actor: initiator
    
states:
  initial:
    action: complete
    transition: :success
```
{% endtab %}

{% tab title="JSON" %}
{% code title="" %}
```javascript
{
    "$schema": "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#",
    "title": "Basic user",
    "actors": {
        "initiator": {
            "title": "Initiator"
        }
    },
    "actions": {
        "complete": {
            "title": "Complete the process",
            "actor": "initiator"
        }
    },
    "states": {
        "initial": {
            "action": "complete",
            "transition": ":success"
        }
    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

While in the _initial_ state, only the _complete_ action can be performed. Once the action is performed, the process will transition to the _:success_ end state, meaning the process has been completed successfully.

## Running a test

To ensure the Live Contract behaves as expected, we need to create and run tests. Tests must be defined using the [Gherkin syntax](https://cucumber.io/docs/gherkin/reference/). The Live Contracts test suite that comes with the full node, defines the steps that can be used in the test.

{% hint style="success" %}
Create file `main.feature` in the `basic` directory.
{% endhint %}

{% code title="main.feature" %}
```
Feature: Run a simple process that is completed in one step

  Background:
    Given a chain is created by "Joe"
    Given "Joe" creates the "main" process using the "basic" scenario
    And   "Joe" is the "initiator" actor of the "main" process

  Scenario:
    When "Joe" runs the "complete" action of the "main" process
    Then the "main" process is completed
```
{% endcode %}

We have a single identity named "Joe". The name is arbitrary but used to keep the identities apart.

Joe initializes the Live Contract by creating a new chain and using our _basic_ scenario. The process name "main" is also arbitrary and used to reference a specific process. It's possible to run multiple processes for a Live Contract, but in our case, only a single process exists.

We need to specify which role Joe is going to have in the process. In this case, he's the _initiator_ of the process.

In the `Scenario` section _(this is unrelated to the workflow scenario)_, we state that will Joe performs the complete action. As defined in our workflow scenario, the process should transition from _initial_ to _:success_, which means the process has been completed successfully.

### Running the Live contract tester

`lctest` is a command-line tool to test workflows described in a Live Contract. It can be run from the `ltonetwork/letsflow-test` docker container.&#x20;

```
docker run ltonetwork/letsflow-test basic
```

The test should succeed, giving the following output:

![](<../.gitbook/assets/screenshot-www.ostechnix.com-2019.05.07-00-34-49 (1).png>)

## Congratulations!

You've successfully created and tested your first Live Contract.

![](../.gitbook/assets/sheep-clap.gif)

### Continue

For more workflow tutorials visit [LetsFlow](https://www.letsflow.io).
