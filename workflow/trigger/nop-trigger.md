# NOP trigger

**NOP trigger is able to manipulate data within the process. So it allows you to update asset or actors information.**

{% tabs %}
{% tab title="YAML" %}
```yaml
"$schema": https://specs.livecontracts.io/v0.2.0/action/nop/schema.json#
actor: issuer
default_response: approve
trigger_response:
  "<if>":
    condition:
      "<apply>":
        input:
          available:
            "<ref>": assets.available
        query: available.shipments > `0`
    then: approve
    else: deny
responses:
  approve: {}
  dany: {}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "$schema": "https://specs.livecontracts.io/v0.2.0/action/nop/schema.json#",
  "actor": "issuer",
  "default_response": "approve",
  "trigger_response": {
    "<if>": {
      "condition": {
        "<apply>": {
          "input": {
            "available": {
              "<ref>": "assets.available"
            }
          },
          "query": "available.shipments > `0`"
        }
      },
      "then": "approve",
      "else": "deny"
    }
  },
  "responses": {
    "approve": {},
    "dany": {}
  }
}
```
{% endtab %}
{% endtabs %}

