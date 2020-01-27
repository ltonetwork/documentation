---
description: 'Run a process with multiple actors, actions and responses.'
---

# A handshake

In the previous tutorial we created our first Live Contract and tested it with `lctest`. The scenario only defined a single actor. In most workflows \(especially decentralized once\), multiple actors participate on the process.

{% hint style="success" %}
Create a new `handshake` subdirectory and copy `scenario.yml` from the `basic` directory.
{% endhint %}

## Adding the recipient actor

Let's expand the scenario for a process where two actors greet each other. We start with our previous scenario and add the _recipient_ actor.

{% tabs %}
{% tab title="YAML" %}
```yaml
$schema: "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#"
title: A handshake

actors:
  initiator:
    title: Initiator
  recipient:
    title: Recipient

actions:
  complete:
    actor: initiator

states:
  initial:
    action: complete
    transition: :success
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "$schema": "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#",
    "title": "A handshake",
    "actors": {
        "initiator": {
            "title": "Initiator"
        },
        "recipient": {
            "title": "Recipient"
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
{% endtab %}
{% endtabs %}

## Greeting each other

We want the two actors to interact in the form of a simple greeting:

> Initiator: Hi, how are you?  
> Recipient: Fine. How about you?  
> Initiator: Fine

The _initiator_ will still _complete_ the process, but from the _initial_ state it will first do a _greeting_, expecting a _reply_. We'll add these 2 actions and subsequently the states where these can be performed. First the _initiator_ waits on the _recipient_ and then visa versa.

{% tabs %}
{% tab title="YAML" %}
```yaml
$schema: "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#"
title: A handshake

actors:
  initiator:
    title: Initiator
  recipient:
    title: Recipient

actions:
  greet:
    actor: initiator
  reply:
    actor: recipient
  complete:
    actor: initiator

states:
  initial:
    action: greet
    transition: wait_on_recipient
  wait_on_recipient:
    action: reply
    transition: wait_on_initiator
  wait_on_initiator:
    action: complete
    transition: :success
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "$schema": "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#",
    "title": "Basic user",
    "actors": {
        "initiator": {
            "title": "Initiator"
        },
        "recipient": {
            "title": "Recipient"
        }
    },
    "actions": {
        "greet": {
            "title": "Greet the recipient",
            "actor": "initiator"
        },
        "reply": {
            "title": "Reply to the greeting",
            "actor": "recipient"
        },
        "complete": {
            "title": "Finish the conversation",
            "actor": "initiator"
        }
    },
    "states": {
        "initial": {
            "action": "greet",
            "transition": "wait_on_recipient"
        },
        "wait_on_recipient": {
            "action": "reply",
            "transition": "wait_on_initiator"
        },
        "wait_on_initiator": {
            "action": "complete",
            "transition": ":success"
        }
    }
}
```
{% endtab %}
{% endtabs %}

We see that we transition from the _initial_ state to _wait\_on\_recipient_, than _wait on initiator_ and finally to a successful completion of the proces.

### Running the test

As before, we want to create a run a test to make sure the process runs as expected.

{% code-tabs %}
{% code-tabs-item title="main.feature" %}
```text
Feature: Two actors greet each other

  Background:
    Given a chain is created by "Joe"
    Given "Joe" creates the "main" process using the "handshake" scenario
    And   "Joe" is the "initiator" actor of the "main" process
    And   "Jane" is the "recipient" actor of the "main" process

  Scenario:
    When "Joe" runs the "greet" action of the "main" process
    Then the "main" process is in the "wait_on_recipient" state
    When "Jane" runs the "reply" action of the "main" process
    Then the "main" process is in the "wait_on_initiator" state
    When "Joe" runs the "complete" action of the "main" process
    Then the "main" process is completed
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Choose to ignore

While it's nice to _reply_, the recipient may also choose to _ignore_ the greeting. In this the process will be cancelled. Let's add an _ignore_ action and make is possible to either _reply_ or _ignore_ in the _wait on recipient_ state.

{% tabs %}
{% tab title="YAML" %}
```yaml
$schema: "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#"
title: A handshake

actors:
  initiator:
    title: Initiator
  recipient:
    title: Recipient

actions:
  greet:
    actor: initiator
  reply:
    actor: recipient
  ignore:
    actor: recipient
  complete:
    actor: initiator

states:
  initial:
    action: greet
    transition: wait_on_recipient
  wait_on_recipient:
    actions:
      - reply
      - ignore
    transitions:
      - action: reply
        transition: wait_on_initiator
      - action: ignore
        transition: :cancelled
  wait_on_initiator:
    action: complete
    transition: :success
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "$schema": "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#",
    "title": "Basic user",
    "actors": {
        "initiator": {
            "title": "Initiator"
        },
        "recipient": {
            "title": "Recipient"
        }
    },
    "actions": {
        "greet": {
            "actor": "initiator"
        },
        "reply": {
            "actor": "recipient"
        },
        "ignore": {
            "actor": "recipient"
        },
        "complete": {
            "actor": "initiator"
        }
    },
    "states": {
        "initial": {
            "action": "greet",
            "transition": "wait_on_recipient"
        },
        "wait_on_recipient": {
            "actions": [
                "reply",
                "ignore"
            ],
            "transitions": [
                {
                    "action": "reply",
                    "transition": "wait_on_initiator"
                },
                {
                    "action": "ignore",
                    "transition": ":cancelled"
                }
            ]
        },
        "wait_on_initiator": {
            "action": "complete",
            "transition": ":success"
        }
    }
}
```
{% endtab %}
{% endtabs %}

Rather than a single `action` property, the _wait\_on\_recipient_ state now has an `actions` property that contains an array with the actions that be performed. The `transition` property has been replaced with a `transitions` property, defining the transition for each action.

### Testing the scenario

We can add a new `Scenario` section in our test file to test the path in the process where the _recipient_ "Jane" ignores the greeting.

{% code-tabs %}
{% code-tabs-item title="main.feature" %}
```text
Feature: Two actors greet each other

  Background:
    Given a chain is created by "Joe"
    Given "Joe" creates the "main" process using the "handshake" scenario
    And   "Joe" is the "initiator" actor of the "main" process
    And   "Jane" is the "recipient" actor of the "main" process

  Scenario:
    When "Joe" runs the "greet" action of the "main" process
    Then the "main" process is in the "wait_on_recipient" state
    When "Jane" runs the "reply" action of the "main" process
    Then the "main" process is in the "wait_on_initiator" state
    When "Joe" runs the "complete" action of the "main" process
    Then the "main" process is completed

  Scenario:
    When "Joe" runs the "greet" action of the "main" process
    Then the "main" process is in the "wait_on_recipient" state
    When "Jane" runs the "ignore" action of the "main" process
    Then the "main" process is cancelled
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> You can have multiple `Scenario` sections. The `Background` runs once and than make a backup of the databases. After each scenario the data in the database is reverted, to run the next scenario.

## A conversation

In this process Jane \(the _recipient_\) can only ignore the greeting or respond with "fine". But unfortunately it's not always sunshine and rainbows. It's possible to define different responses for an action.

{% hint style="info" %}
Up until now, we've created the scenario first and added a test later. It's recommended to take a [TDD](https://en.wikipedia.org/wiki/Test-driven_development) style approach, where you start with a test and then write or modify your scenario.
{% endhint %}

Jane might answer with "not so good", asking for sympathy from Joe. He might still end the conversation \(_complete_\) by answering with "Sorry to hear that" and move on. A nicer thing to is is to ask "What's the matter?" upon which Jane can give a response.

### Testing the scenario

We could add a new `Scenario` section to our `main.feature` test file, but instead we'll create a new test file `recipient-not-good.feature` specifically for the use case where the greeting turns into a bigger conversation.

We already have two paths we want to test. One where Joe brushes Jane off and one where he's genuinely interested. We'll expand the background to the point where Jane give her response.

{% code-tabs %}
{% code-tabs-item title="recipient-not-good.feature" %}
```text
Feature: Two actors meet. The recipient is not doing well.

  Background:
    Given a chain is created by "Joe"
    Given "Joe" creates the "main" process using the "handshake" scenario
    And   "Joe" is the "initiator" actor of the "main" process
    And   "Jane" is the "recipient" actor of the "main" process
    When "Joe" runs the "greet" action of the "main" process
    Then the "main" process is in the "wait_on_recipient" state

  Scenario:
    When "Jane" runs the "reply" action of the "main" process giving a "not_good" response
    Then the "main" process is in the "expect_sympathy" state
    When "Joe" runs the "complete" action of the "main" process
    Then the "main" process is completed

  Scenario:
    When "Jane" runs the "reply" action of the "main" process giving a "not_good" response
    Then the "main" process is in the "expect_sympathy" state

    When "Joe" runs the "sympathize" action of the "main" process
    Then the "main" process is in the "recipient_can_elaborate" state
    When "Jane" runs the "elaborate" action of the "main" process with:
      | reason | My cat is stealing my boyfriend. She pnly cuddles with him. |
    Then the "main" process is in the "wait_on_initiator" state

    When "Joe" runs the "complete" action of the "main" process
    Then the "main" process is completed
```
{% endcode-tabs-item %}
{% endcode-tabs %}

When the recipient elaborates, she'll send the _reason_ as additional response data. In the tutorial ["A proper introduction"](a-proper-introduction.md) we'll see how to use the response data in the process.

### The scenario

When we run this test we can see that it fails. The _not good_ response hasn't been defined. We also need to define the _sympathize_ and _elaborate_ actions and the _expect sympathy_ and _recipient can elaborate_ states.

{% tabs %}
{% tab title="YAML" %}
```yaml
$schema: "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#"
title: A handshake

actors:
  initiator:
    title: Initiator
  recipient:
    title: Recipient

actions:
  greet:
    actor: initiator
  reply:
    actor: recipient
    responses:
      ok:
      not_good:
  ignore:
    actor: recipient
  sympathize:
    actor: initiator
  elaborate:
    actor: recipient
  complete:
    actor: initiator

states:
  initial:
    action: greet
    transition: wait_on_recipient
  wait_on_recipient:
    actions:
      - reply
      - ignore
    transitions:
      - action: reply
        response: ok
        transition: wait_on_initiator
      - action: reply
        response: not_good
        transition: expect_sympathy
      - action: ignore
        transition: :cancelled
  expect_sympathy:
    actions:
      - sympathize
      - complete
    transitions:
      - action: sympathize
        transition: recipient_can_elaborate
      - action: complete
        transition: :success
  recipient_can_elaborate:
    action: elaborate
    transition: wait_on_initiator
  wait_on_initiator:
    action: complete
    transition: :success
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "$schema": "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#",
    "title": "A handshake",
    "actors": {
        "initiator": {
            "title": "Initiator"
        },
        "recipient": {
            "title": "Recipient"
        }
    },
    "actions": {
        "greet": {
            "actor": "initiator"
        },
        "reply": {
            "actor": "recipient",
            "responses": {
                "ok": {},
                "not_good": {}
            }
        },
        "ignore": {
            "actor": "recipient"
        },
        "sympathize": {
            "actor": "initiator"
        },
        "elaborate": {
            "actor": "recipient"
        },
        "complete": {
            "actor": "initiator"
        }
    },
    "states": {
        "initial": {
            "action": "greet",
            "transition": "wait_on_recipient"
        },
        "wait_on_recipient": {
            "actions": [
                "reply",
                "ignore"
            ],
            "transitions": [
                {
                    "action": "reply",
                    "response": "ok",
                    "transition": "wait_on_initiator"
                },
                {
                    "action": "reply",
                    "response": "not_good",
                    "transition": "expect_sympathy"
                },
                {
                    "action": "ignore",
                    "transition": ":cancelled"
                }
            ]
        },
        "expect_sympathy": {
            "actions": [
                "sympathize",
                "complete"
            ],
            "transitions": [
                {
                    "action": "sympathize",
                    "transition": "recipient_can_elaborate"
                },
                {
                    "action": "complete",
                    "transition": ":success"
                }
            ]
        },
        "recipient_can_elaborate": {
            "action": "elaborate",
            "transition": "wait_on_initiator"
        },
        "wait_on_initiator": {
            "action": "complete",
            "transition": ":success"
        }
    }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
The default response has the key "ok" by default. If we define a set of responses without "ok", we also need to set the `default_response` property of the action.
{% endhint %}

## Keeping the conversation going

Instead of asking "What's the matter?", Joe could say "Sorry to hear that. Please tell me more". By repeating the _sympathize_ action, Joe can keep the conversation going.

{% code-tabs %}
{% code-tabs-item title="recipient-not-good.feature" %}
```text
Feature: Two actors meet. The recipient is not doing well.

  Background:
    Given a chain is created by "Joe"
    Given "Joe" creates the "main" process using the "handshake" scenario
    And   "Joe" is the "initiator" actor of the "main" process
    And   "Jane" is the "recipient" actor of the "main" process
    When "Joe" runs the "greet" action of the "main" process
    Then the "main" process is in the "wait_on_recipient" state

  Scenario:
    When "Jane" runs the "reply" action of the "main" process giving a "not_good" response
    Then the "main" process is in the "expect_sympathy" state
    When "Joe" runs the "complete" action of the "main" process
    Then the "main" process is completed

  Scenario:
    When "Jane" runs the "reply" action of the "main" process giving a "not_good" response
    Then the "main" process is in the "expect_sympathy" state

    When "Joe" runs the "sympathize" action of the "main" process
    Then the "main" process is in the "recipient_can_elaborate" state
    When "Jane" runs the "elaborate" action of the "main" process with:
      | reason | My cat is stealing my boyfriend. |
    Then the "main" process is in the "expect_sympathy" state

    When "Joe" runs the "complete" action of the "main" process
    Then the "main" process is completed

  Scenario:
    When "Jane" runs the "reply" action of the "main" process giving a "not_good" response
    Then the "main" process is in the "expect_sympathy" state

    When "Joe" runs the "sympathize" action of the "main" process
    Then the "main" process is in the "recipient_can_elaborate" state
    When "Jane" runs the "elaborate" action of the "main" process with:
      | reason | My cat is stealing my boyfriend. |
    Then the "main" process is in the "expect_sympathy" state

    When "Joe" runs the "sympathize" action of the "main" process
    Then the "main" process is in the "recipient_can_elaborate" state
    When "Jane" runs the "elaborate" action of the "main" process with:
      | reason | She always comes in to cuddle with him. |
    Then the "main" process is in the "expect_sympathy" state

    When "Joe" runs the "sympathize" action of the "main" process
    Then the "main" process is in the "recipient_can_elaborate" state
    When "Jane" runs the "elaborate" action of the "main" process with:
      | reason | Misty has a mean purr. I know it's to taunt me. |
    Then the "main" process is in the "expect_sympathy" state

    When "Joe" runs the "complete" action of the "main" process
    Then the "main" process is completed
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Modifying the scenario

To do so, we just need to modify the _recipient can elaborate_ state to transition to _expect sympathy_. Since the initiator can also _complete_ the process from that state, it doesn't affect our existing use cases.

Previous tests should be, of course, also modified.

{% tabs %}
{% tab title="YAML" %}
```yaml
  recipient_can_elaborate:
    action: elaborate
    transition: expect_sympathy
```
{% endtab %}

{% tab title="JSON" %}
```javascript
        "recipient_can_elaborate": {
            "action": "elaborate",
            "transistion": "expect_sympathy"
        },
```
{% endtab %}
{% endtabs %}

## Instructions and titles

As briefly show in the previous tutorial, actors and actions can have a title. We can also provide a title for states and responses. These titles won't affect the process, but can be used in a UI to display to the end users. Additionally it will make the scenario a bit less cryptic.

The title should tell what we're waiting on. The title of the action describes what an actor will do, while the title of the response describes what the actor has done \(or has said in this example\).

Instructions for a specific actor can be defined for each state. Again, this doesn't influence the process, but can be used in a UI.

{% tabs %}
{% tab title="YAML" %}
```yaml
$schema: "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#"
title: A handshake

actors:
  initiator:
    title: Initiator
  recipient:
    title: Recipient

actions:
  greet:
    actor: initiator
    title: Greet the person you're meeting
    responses:
      ok:
        title: Hi, how are you?
  reply:
    actor: recipient
    title: Respond to the greeting
    responses:
      ok:
        title: Fine. How about you?
      not_good:
        title: Not so good.
  ignore:
    actor: recipient
    title: Ignore the greeting
  sympathize:
    actor: initiator
    title: Ask further
    responses:
      ok:
        title: Sorry to hear that. Please tell me more.
  elaborate:
    actor: recipient
    title: Tell what's the matter.
  complete:
    actor: initiator
    title: End the conversation

states:
  initial:
    action: greet
    transition: wait_on_recipient
  wait_on_recipient:
    title: Waiting on the recipient to respond.
    instructions:
      recipient: Respond or, if you're feeling rude, ignore it.
    actions:
      - reply
      - ignore
    transitions:
      - action: reply
        response: ok
        transition: wait_on_initiator
      - action: reply
        response: not_good
        transition: expect_sympathy
      - action: ignore
        transition: :cancelled
  expect_sympathy:
    title: Waiting on the initiator to respond.
    instructions:
      initiator: Ask further or end the conversation politely.
    actions:
      - sympathize
      - complete
    transitions:
      - action: sympathize
        transition: recipient_can_elaborate
      - action: complete
        transition: :success
  recipient_can_elaborate:
    title: Waiting on the recipient to elaborate.
    instructions:
      recipient: Please explain why it's not going well.
    action: elaborate
    transition: expect_sympathy
  wait_on_initiator:
    title: Waiting on the initiator to respond.
    action: complete
    transition: :success
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "$schema": "https://specs.livecontracts.io/v0.2.0/scenario/schema.json#",
    "title": "A handshake",
    "actors": {
        "initiator": {
            "title": "Initiator"
        },
        "recipient": {
            "title": "Recipient"
        }
    },
    "actions": {
        "greet": {
            "actor": "initiator",
            "title": "Greet the person you're meeting",
            "responses": {
                "ok": {
                    "title": "Hi, how are you?"
                }
            }
        },
        "reply": {
            "actor": "recipient",
            "title": "Respond to the greeting",
            "responses": {
                "ok": {
                    "title": "Fine. How about you?"
                },
                "not_good": {
                    "title": "Not so good."
                }
            }
        },
        "ignore": {
            "actor": "recipient",
            "title": "Ignore the greeting"
        },
        "sympathize": {
            "actor": "initiator",
            "title": "Ask further",
            "responses": {
                "ok": {
                    "title": "Sorry to hear that. Please tell me more."
                }
            }
        },
        "elaborate": {
            "actor": "recipient",
            "title": "Tell what's the matter."
        },
        "complete": {
            "actor": "initiator",
            "title": "End the conversation"
        }
    },
    "states": {
        "initial": {
            "action": "greet",
            "transition": "wait_on_recipient"
        },
        "wait_on_recipient": {
            "title": "Waiting on the recipient to respond.",
            "instructions": {
                "recipient": "Respond or, if you're feeling rude, ignore it."
            },
            "actions": [
                "reply",
                "ignore"
            ],
            "transitions": [
                {
                    "action": "reply",
                    "response": "ok",
                    "transition": "wait_on_initiator"
                },
                {
                    "action": "reply",
                    "response": "not_good",
                    "transition": "expect_sympathy"
                },
                {
                    "action": "ignore",
                    "transition": ":cancelled"
                }
            ]
        },
        "expect_sympathy": {
            "title": "Waiting on the initiator to respond.",
            "instructions": {
                "initiator": "Ask further or end the conversation politely."
            },
            "actions": [
                "sympathize",
                "complete"
            ],
            "transitions": [
                {
                    "action": "sympathize",
                    "transition": "recipient_can_elaborate"
                },
                {
                    "action": "complete",
                    "transition": ":success"
                }
            ]
        },
        "recipient_can_elaborate": {
            "title": "Waiting on the recipient to elaborate.",
            "instructions": {
                "recipient": "Please explain why it's not going well."
            },
            "action": "elaborate",
            "transition": "expect_sympathy"
        },
        "wait_on_initiator": {
            "title": "Waiting on the initiator to respond.",
            "action": "complete",
            "transition": ":success"
        }
    }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Adding titles and instructions is optional. While it makes the scenario less cryptic. It also substantially increase lines of code of the scenario.
{% endhint %}

We didn't specify a `title` for the _complete_ action. That's because the response that would giving in the _wait on initiator_ state would would "Fine". In the state where we _expect sympathy from initiator_ would the response for the same action would be "Sorry, to hear that".

We could define a new action, but there are better ways to handle this. We'll discuss that in the next tutorial.

## Now you!

The _initiator_ can still only reply with "Fine" when the recipient ask "How about you?". We should change that, so the _initiator_ could also reply with "Could be better". To keep it simple, this will not resort in a conversation.

Keep the structure so that it's still up to the _initiator_ to _complete_ the process. The recipient can only respond with "Sorry to hear that", after which we transition to the state where we _wait on the initiator_ to end the process.

In other words; The _initiator_ can respond with _no good_, after witch the process transitions to a new state where we _expect an acknowledgement_ from the _recipient_.

{% hint style="success" %}
1. Create a new test file named `initiator-not-good.feature`
2. The `Before` section should go through the process all the way to the state where we _wait on the the initiator_.
3. The initiator should then respond with _no good_, after witch the process transitions to a new state where we _expect an acknowledgement_ from the recipient.
4. Create a new test _Scenario_ section for this use case.
5. Modify the scenario so the new test succeeds.
{% endhint %}

