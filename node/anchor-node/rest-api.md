# REST API

{% hint style="info" %}
In the following examples, replace`https://lto.example.com` with the domain or IP address of **your** node.&#x20;
{% endhint %}

### Authorization

A node can be configured with an authorization token. This can be done in case the api of the node is exposed publicly. Once the token is configured the anchoring of hash on the chain requires an authorization header

```
Authorization: bearer <token>
```

## Anchor a hash on the blockchain

<mark style="color:green;">`POST`</mark> `https://lto.example.com/hash`

#### Request Body

| Name     | Type   | Description |
| -------- | ------ | ----------- |
| hash     | string |             |
| encoding | string |             |

{% tabs %}
{% tab title="200 " %}
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
{% endtab %}

{% tab title="400 " %}
```
invalid body given
no hash given
invalid hash given
invalid encoding given
```
{% endtab %}

{% tab title="500 " %}
```
failed to anchor '[reason]'
```
{% endtab %}
{% endtabs %}

## Verify if a hash was anchored

<mark style="color:blue;">`GET`</mark> `https://lto.example.com/hash/:hash`

Get chainpoint info of an anchored hash.

#### Path Parameters

| Name | Type   | Description                       |
| ---- | ------ | --------------------------------- |
| hash | string | anchor hash in hexadecimal format |

{% tabs %}
{% tab title="200 " %}
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
{% endtab %}

{% tab title="404 " %}
```
```
{% endtab %}

{% tab title="500 " %}
```
Failed to get transaction by hash and encoding '[reason]'
```
{% endtab %}
{% endtabs %}

## Verify if a hash was anchored

<mark style="color:blue;">`GET`</mark> `https://lto.example.com/hash/:hash/encoding/:encoding`

Get chainpoint info of an anchored hash in given encoding.

#### Path Parameters

| Name     | Type   | Description                                                                          |
| -------- | ------ | ------------------------------------------------------------------------------------ |
| hash     | string | anchored hash                                                                        |
| encoding | string | <p>The encoding in which the hash is given. Options are<br>(hex, base58, base64)</p> |

{% tabs %}
{% tab title="200 " %}
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
{% endtab %}

{% tab title="400 " %}
```
invalid encoding given
```
{% endtab %}

{% tab title="404 " %}
```
```
{% endtab %}

{% tab title="500 " %}
```
Failed to get transaction by hash and encoding '[reason]'
```
{% endtab %}
{% endtabs %}
