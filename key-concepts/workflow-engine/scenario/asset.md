---
description: An asset can be any type of structured data relevant to the process.
---

# Asset

## Scenario asset

Items in the `assets` property of the **scenario** aren't asset objects, but JSON schemas defining the properties available for the asset once instantiated in the process.

Assets don't have any required properties, but it's common for an asset to have a `title` property. If this property exist, it's copied from the JSON schema.

{% tabs %}
{% tab title="YAML" %}
```yaml
assets:
  contract:
    properties:
      id:
        type: string
        format: uri
      name:
        type: string
  book:
    properties:
      id:
        type: string
        format: uri
      title:
        type: string
      isbn:
        type: string
        pattern: "^\\d{13}$"
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "assets": {
    "request": {
      "type": "object",
      "properties": {
        "description": {
          "type": "string"
        },
        "urgency": {
          "type": "string",
          "enum": [
            "normal",
            "high",
            "critical"
          ]
        }
      }
    },
    "quotation": {
      "type": "object",
      "properties": {
        "title": {
          "type": "string"
        },
        "url": {
          "type": "string"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

Properties that aren't defined don't exist and can't be set, unless `additionalProperties` is set in the actor definition.

## Scenario definition

The `definitions` property contains a set of immutable assets. Unlike the `assets` property, the items aren't JSON schemas but \(instantiated\) asset objects. These are simply copied to the process.

{% tabs %}
{% tab title="YAML" %}
```yaml
definitions:
  request_form:
    title: Quotation request form
    definition:
    - fields:
      - type: external-select
        label: Supplier
        name: supplier
        url: https://jsonplaceholder.typicode.com/users
        optionText: name
        optionValue: "{ name, email, phone }"
        required: true
      - type: textarea
        label: Description
        name: description
        helptip: Which service would you like a quotation for?
      - type: select
        label: Urgency
        name: urgency
        options:
        - value: normal
          label: Normal
        - value: high
          label: High
        - value: critical
          label: Critical
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "definitions": {
    "request_form": {
      "title": "Quotation request form",
      "definition": [
        {
          "fields": [
            {
              "type": "external-select",
              "label": "Supplier",
              "name": "supplier",
              "url": "https://jsonplaceholder.typicode.com/users",
              "optionText": "name",
              "optionValue": "{ name, email, phone }",
              "required": true
            },
            {
              "type": "textarea",
              "label": "Description",
              "name": "description",
              "helptip": "Which service would you like a quotation for?"
            },
            {
              "type": "select",
              "label": "Urgency",
              "name": "urgency",
              "options": [
                {
                  "value": "normal",
                  "label": "Normal"
                },
                {
                  "value": "high",
                  "label": "High"
                },
                {
                  "value": "critical",
                  "label": "Critical"
                }
              ]
            }
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Definitions CAN NEVER be modified. If you want an asset that has default values when the process is created, but where the values may still change during the process, define it under `assets` and use the `default` property.
{% endhint %}

