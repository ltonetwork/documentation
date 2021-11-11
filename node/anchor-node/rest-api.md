# REST API

{% hint style="info" %}
In the following examples, replace`https://lto.example.com` with the domain or IP address of **your** node.&#x20;
{% endhint %}

### Authorization

A node can be configured with an authorization token. This can be done in case the api of the node is exposed publicly. Once the token is configured the anchoring of hash on the chain requires an authorization header

```
Authorization: bearer <token>
```

{% swagger baseUrl="https://lto.example.com" path="/hash" method="post" summary="Anchor a hash on the blockchain" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="hash" type="string" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="encoding" type="string" %}

{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
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
{% endswagger-response %}

{% swagger-response status="400" description="" %}
```
invalid body given
no hash given
invalid hash given
invalid encoding given
```
{% endswagger-response %}

{% swagger-response status="500" description="" %}
```
failed to anchor '[reason]'
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://lto.example.com" path="/hash/:hash" method="get" summary="Verify if a hash was anchored" %}
{% swagger-description %}
Get chainpoint info of an anchored hash.
{% endswagger-description %}

{% swagger-parameter in="path" name="hash" type="string" %}
anchor hash in hexadecimal format
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
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
{% endswagger-response %}

{% swagger-response status="404" description="" %}
```
```
{% endswagger-response %}

{% swagger-response status="500" description="" %}
```
Failed to get transaction by hash and encoding '[reason]'
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://lto.example.com" path="/hash/:hash/encoding/:encoding" method="get" summary="Verify if a hash was anchored" %}
{% swagger-description %}
Get chainpoint info of an anchored hash in given encoding.
{% endswagger-description %}

{% swagger-parameter in="path" name="hash" type="string" %}
anchored hash
{% endswagger-parameter %}

{% swagger-parameter in="path" name="encoding" type="string" %}
The encoding in which the hash is given. Options are

\


(hex, base58, base64)
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
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
{% endswagger-response %}

{% swagger-response status="400" description="" %}
```
invalid encoding given
```
{% endswagger-response %}

{% swagger-response status="404" description="" %}
```
```
{% endswagger-response %}

{% swagger-response status="500" description="" %}
```
Failed to get transaction by hash and encoding '[reason]'
```
{% endswagger-response %}
{% endswagger %}
