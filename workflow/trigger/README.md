---
description: Triggers send are used for automated actions.
---

# Trigger

### Configuring custom actions to use trigger

```text
triggers:
  payment:
    type: http
    schema: "https://specs.psp.example.com/actions/payment/schema.json#"
    url: http://psp.example.com/payment
    method: POST
    projection: "{body: {name: name, amount: quantity * price }}"
```

