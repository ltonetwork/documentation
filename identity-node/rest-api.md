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

## Trust network

{% api-method method="get" host="https://lto.example.com" path="/trust/:address" %}
{% api-method-summary %}
Get all the roles of an identity
{% endapi-method-summary %}

{% api-method-description %}
Resolves the roles from an identity
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="address" type="string" required=true %}
Account's address in Base58 format
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "roles": [
    "authority",
    "notary"
  ],
  "issue_roles": [
    { "type": 100, "role": "notary" }
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

## Verifiable credentials

{% api-method method="post" host="https://lto.example.com" path="/verify" %}
{% api-method-summary %}
Verify that a verifiable credential is valid
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

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

