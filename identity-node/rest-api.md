# REST API

{% hint style="info" %}
In the following examples, replace`https://lto.example.com` with the domain or IP address of **your** node. 
{% endhint %}

### Authorization

A node can be configured with an authorization token. This can be done in case the api of the node is exposed publicly. Once the token is configured the anchoring of hash on the chain requires an authorization header

```text
Authorization: bearer <token>
```

## Decentralized identifiers \(DID\)

{% api-method method="get" host="https://lto.example.com" path="/identities/:address" %}
{% api-method-summary %}
Resolve DID
{% endapi-method-summary %}

{% api-method-description %}
Resolve a DID into a DID document
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="address" type="string" required=true %}
DID or LTO address
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

{% api-method method="get" host="https://lto.example.com" path="/identities/:address/derived/:secret" %}
{% api-method-summary %}
Resolve derived DID
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="address" type="string" required=true %}
DID or LTO address
{% endapi-method-parameter %}

{% api-method-parameter name="secret" type="string" required=true %}
Base58 encoded random secret
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

