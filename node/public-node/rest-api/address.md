# Address

{% swagger baseUrl="https://nodes.lto.network" path="/addresses/balance/:address" method="get" summary="Balance" %}
{% swagger-description %}
Get account balance
{% endswagger-description %}

{% swagger-parameter in="path" name="address" %}
Account's address in Base58 format
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```javascript
{
  "address": "3N3keodUiS8WLEw9W4BKDNxgNdUpwSnpb3K",
  "confirmations": 0,
  "balance": 100945889661986
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://nodes.lto.network" path="/addresses/balance/:address/:confirmations" method="get" summary="Balance after confirmations" %}
{% swagger-description %}
Get account balance after X confirmations from now
{% endswagger-description %}

{% swagger-parameter in="path" name="address" %}
Account's address in Base58 format
{% endswagger-parameter %}

{% swagger-parameter in="path" name="confirmations" %}
Number of confirmations
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```javascript
{
  "address": "3N3keodUiS8WLEw9W4BKDNxgNdUpwSnpb3K",
  "confirmations": 500,
  "balance": 100945388397565
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://nodes.lto.network" path="/addresses/balance/details/:address" method="get" summary="Balance details" %}
{% swagger-description %}
Get balance details
{% endswagger-description %}

{% swagger-parameter in="path" name="address" %}
Account's address in Base58 format
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```javascript
{
  "address": "3P2HNUd5VUPLMQkJmctTPEeeHumiPN2GkTb",
  "regular": 1498883844,
  "generating": 1066926675599895,
  "available": 1498883844,
  "effective": 1067913688974251
}
```
{% endswagger-response %}
{% endswagger %}

| Balance    | Description                                               |
| ---------- | --------------------------------------------------------- |
| Regular    | The amount LTO owned by the account, including LTO leased |
| Available  | The amount LTO owned by the account, excluding LTO leased |
| Effective  | The available amount + the LTO leased to the account      |
| Generating | The minimal effective balance over the last 1000 blocks   |
