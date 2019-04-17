# HTTP trigger

HTTP Trigger sends an http request to a specified url

{% tabs %}
{% tab title="YAML" %}
```yaml
$schema: https://specs.livecontracts.io/v0.2.0/action/http/schema.json#
title: Step1
url: http://example.com
method: "POST"
headers:
  Content-Type: "application/json"
data:
  foo: bar
actor: system
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "$schema": "https://specs.livecontracts.io/v0.2.0/action/http/schema.json#",
  "title": "Step1",
  "url": "http://example.com",
  "method": "POST",
  "headers": {
    "Content-Type": "application/json"
  },
  "data": {
    "foo": "bar"
  },
  "actor": "system"
}
```
{% endtab %}
{% endtabs %}



