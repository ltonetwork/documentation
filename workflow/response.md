---
description: >-
  Executing an action in a process results a response. This is regardless of
  whether the action is performed manually by the user of automatically by the
  node.
---

# Response

## Sending a response

Sending a response must be done by adding an event with a response resource to the event chain.

{% tabs %}
{% tab title="YAML" %}
```yaml
$schema: https://specs.livecontracts.io/v0.2.0/response/schema.json#
process: 234jashaj3ljk34jkala
action: request_quote
response: ok
data:
  name: John Doe
  email: john@example.com
  product: kettle washed jeans
  quantity: 700

```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "$schema": "https://specs.livecontracts.io/v0.2.0/response/schema.json#",
  "process": "234jashaj3ljk34jkala",
  "action": "request_quote",
  "data": {
    "name": "John Doe",
    "email": "john@example.com",
    "product": "kettle washed jeans",
    "quantity": 700
  }
}
```
{% endtab %}
{% endtabs %}

## Response schema

### $schema

The Live Contracts response [JSON schema](http://json-schema.org) URI that describes the JSON structure of the response.

### process

The process id or process as object. If the process is an object, only the `id` property is used and the rest is ignored.

### action

The key of the action that was performed.

### response

The key of the response defined by the action. This property may be omitted, defaulting to `ok`.

### data

The response data. What is expected depends on the action, specifically on the update instruction.

