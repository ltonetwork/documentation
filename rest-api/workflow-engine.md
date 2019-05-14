---
description: REST API of the Workflow Engine
---

# Workflow Engine

{% api-method method="get" host="https://lto.example.com" path="/processes/{id}" %}
{% api-method-summary %}
Get a process
{% endapi-method-summary %}

{% api-method-description %}
Get a running or completed 
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="id" type="string" required=false %}
Process identifier
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% tabs %}
{% tab title="Basic" %}
{% hint style="warning" %}
The workflow engine REST API has more endpoints, however typically you shouldn't interact with those directly, just via the events.
{% endhint %}
{% endtab %}

{% tab title="Advanced" %}

{% endtab %}
{% endtabs %}



