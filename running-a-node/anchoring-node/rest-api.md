---
description: REST API of the Anchor node
---

# REST API

{% hint style="info" %}
In the following examples, replace`https://lto.example.com` with the domain or IP address of **your** node. 
{% endhint %}

### Authorization

A node can be configured with an authorization token. This can be done in case the api of the node is exposed publicly. Once the token is configured the anchoring of hash on the chain requires an authorization header

```text
Authorization: bearer <token>
```

{% api-method method="post" host="https://lto.example.com" path="/hash" %}
{% api-method-summary %}
Anchor a hash on the blockchain
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="hash" type="string" required=true %}

{% endapi-method-parameter %}

{% api-method-parameter name="encoding" type="string" required=true %}

{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "chainpoint": {
    "@context": "https://w3id.org/chainpoint/v2",
    "type": "ChainpointSHA256v2",
    "targetHash": "8c4b53f85243892edcbb3ccec947269f95ba3da2e84fee84fcc277b19fb68044",
    "anchors": [
      {
        "type": "LTODataTransaction",
        "sourceId": "6KVLV6zRSVR8tqCZZ9cqsbUJkP8fNDiY12CmXrGnrwTd"
      }
    ],
    "block": {
      "height": "483025"
    },
    "transaction": {
      "position": "0"
    }
  }
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
invalid body given
no hash given
invalid hash given
invalid encoding given
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
failed to anchor '[reason]'
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://lto.example.com" path="/hash/:hash/encoding/:encoding" %}
{% api-method-summary %}
Verify if a hash was anchored
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="hash" type="string" required=true %}
hash you are looking
{% endapi-method-parameter %}

{% api-method-parameter name="encoding" type="string" required=false %}
The encoding in which the hash is given. Options are  
\(hex, base58, base64\)
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "chainpoint": {
    "@context": "https://w3id.org/chainpoint/v2",
    "type": "ChainpointSHA256v2",
    "targetHash": "8c4b53f85243892edcbb3ccec947269f95ba3da2e84fee84fcc277b19fb68044",
    "anchors": [
      {
        "type": "LTODataTransaction",
        "sourceId": "6KVLV6zRSVR8tqCZZ9cqsbUJkP8fNDiY12CmXrGnrwTd"
      }
    ],
    "block": {
      "height": "483025"
    },
    "transaction": {
      "position": "0"
    }
  }
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
no hash given
invalid encoding given
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
Failed to get transaction by hash and encoding '[reason]'
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://lto.example.com" path="/hash/:hash" %}
{% api-method-summary %}
Verify if a hash was anchored
{% endapi-method-summary %}

{% api-method-description %}
Similar to the previous method however the encoding is now set to hex by default
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="hash" type="string" required=true %}
hash you are looking
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

