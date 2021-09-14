# Address

## Address

{% api-method method="get" host="https://node.testnet.ltonetwork.com" path="/addresses" %}
{% api-method-summary %}
/addresses
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

```
[
  "3NBVqYXrapgJP9atQccdBPAgJPwHDKkh6A8",
  "3Mx2afTZ2KbRrLNbytyzTtXukZvqEB8SkW7"
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

\*\*\*\*

### GET /addresses/seq/{from}/{to}

![](https://img.shields.io/badge/MAINNET-available-4bc51d.svg)

Get list of accounts addresses with indexes at this range in the node's wallet.

**Response:**

```javascript
[
"3NBVqYXrapgJP9atQccdBPAgJPwHDKkh6A8",  
"3Mx2afTZ2KbRrLNbytyzTtXukZvqEB8SkW7"
]
```

### POST /addresses

![](https://img.shields.io/badge/MAINNET-available-4bc51d.svg)

Generate a new account address in the wallet._Requires API\_KEY to be provided_

**Request params:**

```text
  "address" - account's address in Base58 format
```

**Response JSON example:**

```javascript
{

"address": "3Mx2afTZ2KbRrLNbytyzTtXukZvqEB8SkW7"

}
```

### GET /addresses/balance/details/{address}

![](https://img.shields.io/badge/MAINNET-available-4bc51d.svg)

Get Balance details:

```text
"address" - account's address in Base58 format
"Regular" — that's how much LTO you have, including those you leased;
"Available" — the same as regular only without LTO you leased;
"Effective" — available plus those LTO which is leased to you;
"Generating" — the minimal effective for last 1000 blocks;
```

**Response JSON example:**

```javascript
{
  "address": "3P2HNUd5VUPLMQkJmctTPEeeHumiPN2GkTb",
  "regular": 1498883844,
  "generating": 1066926675599895,
  "available": 1498883844,
  "effective": 1067913688974251
}
```

### GET /addresses/balance/{address}

![](https://img.shields.io/badge/MAINNET-available-4bc51d.svg)

Get account balance in LTO in {address}:

```text
  "address" - account's address in Base58 format
```

**Response JSON example:**

```javascript
{

  "address": "3N3keodUiS8WLEw9W4BKDNxgNdUpwSnpb3K",
  "confirmations": 0,
  "balance": 100945889661986

}
```

### GET /addresses/balance/{address}/{confirmations}

![](https://img.shields.io/badge/MAINNET-available-4bc51d.svg)

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

