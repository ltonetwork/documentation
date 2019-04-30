---
description: Triggers send are used for automated actions.
---

# Trigger

### Configuring custom actions to use trigger

```yaml
triggers:
  payment:
    type: http
    schema: "https://specs.psp.example.com/actions/payment/schema.json#"
    url: http://psp.example.com/payment
    method: POST
    projection: "{body: {name: name, amount: quantity * price }}"
```

### Using a custom action

{% tabs %}
{% tab title="YAML" %}
```yaml
$schema: "https://specs.psp.example.com/actions/payment/schema.json#"
title: Step1
actor: system
name: Payment1
quantity: 1
price: 100
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "$schema": "https://specs.psp.example.com/actions/payment/schema.json#",
  "title": "Step1",
  "actor": "system",
  "name": "Payment1",
  "quantity": 1,
  "price": 100
}
```
{% endtab %}
{% endtabs %}

