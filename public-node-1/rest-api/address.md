# Address

{% api-method method="get" host="https://nodes.lto.network" path="/addresses/balance/:address" %}
{% api-method-summary %}
Balance
{% endapi-method-summary %}

{% api-method-description %}
Get account balance
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="address" required=true %}
Account's address in Base58 format
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "address": "3N3keodUiS8WLEw9W4BKDNxgNdUpwSnpb3K",
  "confirmations": 0,
  "balance": 100945889661986
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://nodes.lto.network" path="/addresses/balance/:address/:confirmations" %}
{% api-method-summary %}
Balance after confirmations
{% endapi-method-summary %}

{% api-method-description %}
Get account balance after X confirmations from now
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="address" required=true %}
Account's address in Base58 format
{% endapi-method-parameter %}

{% api-method-parameter name="confirmations" required=true %}
Number of confirmations
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "address": "3N3keodUiS8WLEw9W4BKDNxgNdUpwSnpb3K",
  "confirmations": 500,
  "balance": 100945388397565
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://nodes.lto.network" path="/addresses/balance/details/:address" %}
{% api-method-summary %}
Balance details
{% endapi-method-summary %}

{% api-method-description %}
Get balance details
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="address" required=true %}
Account's address in Base58 format
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "address": "3P2HNUd5VUPLMQkJmctTPEeeHumiPN2GkTb",
  "regular": 1498883844,
  "generating": 1066926675599895,
  "available": 1498883844,
  "effective": 1067913688974251
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

| Balance | Description |
| :--- | :--- |
| Regular | The amount LTO owned by the account, including LTO leased |
| Available | The amount LTO owned by the account, excluding LTO leased |
| Effective | The available amount + the LTO leased to the account |
| Generating | The minimal effective balance over the last 1000 blocks |

### GET /addresses/balance/{address}/{confirmations}

![master](https://img.shields.io/badge/MAINNET-available-4bc51d.svg)

Get account balance in LTO by {address} after {confirmations} from now:

```text
  "address" - account's address in Base58 format
  "confirmations" - N of confirmations
```

**Response JSON example:**

```javascript
{

"address": "3N3keodUiS8WLEw9W4BKDNxgNdUpwSnpb3K",

"confirmations": 500,

"balance": 100945388397565

}
```

