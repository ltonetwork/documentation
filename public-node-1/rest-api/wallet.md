---
description: API endpoints for the node's wallet
---

# Wallet

{% api-method method="get" host="https://nodes.lto.network" path="/wallet/addresses" %}
{% api-method-summary %}
Addresses
{% endapi-method-summary %}

{% api-method-description %}
Get list of all accounts addresses in the node's wallet.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
[
  "3NBVqYXrapgJP9atQccdBPAgJPwHDKkh6A8",
  "3Mx2afTZ2KbRrLNbytyzTtXukZvqEB8SkW7"
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://nodes.lto.network" path="/wallet/addresses/seq/:from/:to" %}
{% api-method-summary %}
Address range
{% endapi-method-summary %}

{% api-method-description %}
Get list of accounts addresses with indexes at this range in the node's wallet.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="from" type="number" required=true %}

{% endapi-method-parameter %}

{% api-method-parameter name="to" type="number" required=true %}

{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
[
  "3NBVqYXrapgJP9atQccdBPAgJPwHDKkh6A8",  
  "3Mx2afTZ2KbRrLNbytyzTtXukZvqEB8SkW7"
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://nodes.lto.network" path="/wallet/addresses" %}
{% api-method-summary %}
Add address
{% endapi-method-summary %}

{% api-method-description %}
Generate a new account address in the wallet.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="" type="string" required=true %}

{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
"Bearer " + API\_KEY
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "address": "3Mx2afTZ2KbRrLNbytyzTtXukZvqEB8SkW7"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### 

