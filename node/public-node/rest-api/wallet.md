---
description: API endpoints for the node's wallet
---

# Wallet

{% swagger baseUrl="https://nodes.lto.network" path="/wallet/addresses" method="get" summary="Addresses" %}
{% swagger-description %}
Get list of all accounts addresses in the node's wallet.
{% endswagger-description %}

{% swagger-response status="200" description="" %}
```javascript
[
  "3NBVqYXrapgJP9atQccdBPAgJPwHDKkh6A8",
  "3Mx2afTZ2KbRrLNbytyzTtXukZvqEB8SkW7"
]
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://nodes.lto.network" path="/wallet/addresses/seq/:from/:to" method="get" summary="Address range" %}
{% swagger-description %}
Get list of accounts addresses with indexes at this range in the node's wallet.
{% endswagger-description %}

{% swagger-parameter in="path" name="from" type="number" %}

{% endswagger-parameter %}

{% swagger-parameter in="path" name="to" type="number" %}

{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```javascript
[
  "3NBVqYXrapgJP9atQccdBPAgJPwHDKkh6A8",  
  "3Mx2afTZ2KbRrLNbytyzTtXukZvqEB8SkW7"
]
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://nodes.lto.network" path="/wallet/addresses" method="post" summary="Add address" %}
{% swagger-description %}
Generate a new account address in the wallet.
{% endswagger-description %}

{% swagger-parameter in="path" name="" type="string" %}

{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
"Bearer " + API_KEY
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```javascript
{
  "address": "3Mx2afTZ2KbRrLNbytyzTtXukZvqEB8SkW7"
}
```
{% endswagger-response %}
{% endswagger %}

###
