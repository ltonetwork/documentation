# Event trigger

The Node will add an event to the event chain. The event will be signed with the private key of the node.

{% tabs %}
{% tab title="YAML" %}
```yaml
"$schema": https://specs.livecontracts.io/v0.2.0/action/event/schema.json#
actor: issuer
body:
  "$schema": https://specs.livecontracts.io/v0.2.0/identity/schema.json#
  id: !ref actors.license_holder.id
  signkeys: !ref actors.license_holder.signkeys
  node: !ref actors.license_holder.node
responses:
  ok:
    display: never
  error:
    display: always
    title: Failed to add the license holder

```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "$schema": "https://specs.livecontracts.io/v0.2.0/action/event/schema.json#",
  "actor": "issuer",
  "body": {
      "$schema": "https://specs.livecontracts.io/v0.2.0/identity/schema.json#",
      "id": {
        "<ref>": "actors.license_holder.id"
      },
      "signkeys": {
        "<ref>": "actors.license_holder.signkeys"
      },
      "node": {
        "<ref>": "actors.license_holder.node"
      }
  },
  "responses": {
    "ok": {
      "display": "never"
    },
    "error": {
      "display": "always",
      "title": "Failed to add the license holder"
    }
  }
}
```
{% endtab %}
{% endtabs %}

