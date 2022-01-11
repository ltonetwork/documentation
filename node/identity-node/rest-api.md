# REST API

{% hint style="info" %}
In the following examples, replace`https://lto.example.com` with the domain or IP address of **your** node.&#x20;
{% endhint %}

### Authorization

A node can be configured with an authorization token. This can be done in case the api of the node is exposed publicly. Once the token is configured the anchoring of hash on the chain requires an authorization header

```
Authorization: bearer <token>
```

## Decentralized identifiers (DID)

{% swagger baseUrl="https://lto.example.com" path="/identities/:address" method="get" summary="Resolve DID" %}
{% swagger-description %}
Resolve a DID into a DID document
{% endswagger-description %}

{% swagger-parameter in="path" name="address" type="string" %}
DID or LTO address
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://lto.example.com" path="/identities/:address/derived/:secret" method="get" summary="Resolve derived DID" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="address" type="string" %}
DID or LTO address
{% endswagger-parameter %}

{% swagger-parameter in="path" name="secret" type="string" %}
Base58 encoded random secret
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
```
{% endswagger-response %}
{% endswagger %}

## Trust network

{% swagger baseUrl="https://lto.example.com" path="/trust/:address" method="get" summary="Get all the roles of an identity" %}
{% swagger-description %}
Resolves the roles from an identity
{% endswagger-description %}

{% swagger-parameter in="query" name="address" type="string" %}
Account's address in Base58 format
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
  "roles": [
    "authority",
    "notary"
  ],
  "issues_roles": [
    { "type": 100, "role": "notary" }
  ],
  "issues_authorization": [
    "https://www.w3.org/2018/credentials/examples/v1"
  ]
}
```
{% endswagger-response %}

{% swagger-response status="400" description="" %}
```
{
  "error": "invalid address"
}
```
{% endswagger-response %}
{% endswagger %}
