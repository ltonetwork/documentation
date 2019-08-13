# Action

## Scenario action

An action is something that can be performed by actor or the node of an actor. An action may trigger a state transition and / or may update the process projection.

{% tabs %}
{% tab title="YAML" %}
```yaml
issue:
  "$schema": https://specs.livecontracts.io/v0.2.0/action/schema.json
  actor: issuer
  responses:
    ok:
      title: Issued new license
      update:
      - select: assets.license
      - select: assets.available
        projection: "{shipments: shipments, quantity: quantity}"
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "issue": {
    "$schema": "https://specs.livecontracts.io/v0.2.0/action/schema.json",
    "actor": "issuer",
    "responses": {
      "ok": {
        "title": "Issued new license",
        "update": [
          {
            "select": "assets.license"
          },
          {
            "select": "assets.available",
            "projection": "{shipments: shipments, quantity: quantity}"
          }
        ]
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Action schema

`https://specs.livecontracts.io/v0.2.0/action/schema.json`

### $schema

The action [JSON schema](http://json-schema.org) URI that describes the JSON structure of the action. This is also be used for automation and may be used by the UI.

[Action specifications](action.md) contains a number of actions that SHOULD be supported on any LegalThings One system.

### title

A short title for the action.

### label

Label that is shown when picking this action.

### description

A long description for the action that is shown when the action has been performed.

### actor

The reference of the actor that may perform this action. If more than one actor may perform the action, set an array of actor references.

### responses

A set of possible responses for the action. The keys of the object is used to reference the response.

### default\_response

The key of the default response. This is used for the golden flow.

### display

Should the action be displayed in the history? Choose one of the following options "always", "once" or "never". In the case of "once", only the latest instance of the action will be displayed.

## Response schema

`https://specs.livecontracts.io/v0.1.0/scenario/schema.json#response`

Instructions for a response of an action.

### label

The label that will be displayed in the process if the action is performed.

### display

Property to determine if the response should be shown in the history \(`prev`\) of the process. Options

* never - Never show the response
* once - If there are multiple successive responses of the same action, only show the latest
* always - Always show all the response

### transition

Hard-wired transition for an action response to a specific state. This can be used for actions that always transition to a specific state regardless of the state the process is now in.

The value must be the key of an action listed in the actions array.

### update

[Update instruction](action.md#update-instruction-schema) or array of update instructions.

## Update instruction schema

`https://specs.livecontracts.io/v0.1.0/scenario/schema.json#update-instruction`

After a response is given, the projection of the process may be updated. Update instructions can update the process information, assets or actors.

## select

A reference to the item in the process that should be updated. This MUST be a property of `info`, `assets` or `actors`.

This can't be a full JMESPath expression, instead it should be it's a simplified notation using the object and array notation \(eg `assets.stock.items[3]`\).

## data

The data to which the items should be updated. By default, this is the `data` of the response.

It's typically useful to use a [data instructions](data-instruction.md) for data. While processing a response, the process has an additional property `response` which may be referenced.

{% tabs %}
{% tab title="YAML" %}
```yaml
select: assets.document
data:
  url: !tpl "https://docs.example.com/documents/{{ response.data.id }}"
  content: !ref response.data.body
  content_media_type: text/html
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "select": "assets.document",
  "data": {
    "url": { "<tpl>": "https://docs.example.com/documents/{{ response.data.id }}" },
    "content": { "<ref>": "response.data.body" },
    "content_media_type": "text/html"
  }
}
```
{% endtab %}
{% endtabs %}



