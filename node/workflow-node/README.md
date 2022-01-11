# Workflow node

![](../../.gitbook/assets/letsflow.png)

[LetsFlow is a workflow engine](https://letsflow.io) for running processes, described in YAML or JSON.

{% tabs %}
{% tab title="YAML" %}
```yaml
schema: "https://specs.letsflow.io/v0.3.0/scenario#"
title: My first scenario

actors:
  user:
    title: The user

actions:
  complete:
    title: Complete the process

states:
  initial:
    on: complete
    goto: (success)
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "schema": "https://specs.letsflow.io/v0.3.0/scenario#",
    "title": "My first scenario",
    "actors": {
        "user": {
            "title": "user"
        }
    },
    "actions": {
        "complete": {
            "title": "Complete the process"
        }
    },
    "states": {
        "initial": {
            "action": "complete",
            "transition": "(success)"
        }
    }
}
```
{% endtab %}

{% tab title="JSON (full)" %}
```javascript
{
    "$schema": "https://specs.letsflow.io/v0.3.0/scenario#",
    "title": "My first scenario",
    "actors": {
        "user": {
            "$schema": "https://specs.letsflow.io/v0.3.0/actor#",
            "title": "user"
        }
    },
    "actions": {
        "complete": {
            "title": "Complete the process",
            "responses": {
                "ok": {
                    "title": null,
                    "display": "always"
                    "update": [ ]
                }
            }
        }
    },
    "states": {
        "initial": {
            "actions": [
                "complete"
            ],
            "transitions": [
                {
                    "on": "*.*"
                    "goto": "(success)"
                }
            ]
        }
    }
}
```
{% endtab %}
{% endtabs %}
