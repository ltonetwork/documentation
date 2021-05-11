# Untitled

{% hint style="info" %}
In the following examples, replace`https://lto.example.com` with the domain or IP address of **your** node. 
{% endhint %}

### Authorization

A node can be configured with an authorization token. This can be done in case the api of the node is exposed publicly. Once the token is configured the anchoring of hash on the chain requires an authorization header

```text
Authorization: bearer <token>
```

