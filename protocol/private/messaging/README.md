# Messaging

To share information on the private layer, it should be wrapped as a message.

## Message

&#x20;A message has the following structure

<table><thead><tr><th width="183">Field</th><th></th></tr></thead><tbody><tr><td><code>type</code></td><td>Message type, used for routing</td></tr><tr><td><code>mediaType</code></td><td>MIME type of the data</td></tr><tr><td><code>data</code></td><td>Binary</td></tr><tr><td><code>timestamp</code></td><td>Unix timestamp in milliseconds. Time of when the event was signed</td></tr><tr><td><code>sender</code></td><td>Public key with key type</td></tr><tr><td><code>signature</code></td><td>Signature of the binary representation of the event</td></tr><tr><td><code>recipient</code></td><td>Address of an LTO account</td></tr><tr><td><code>hash</code></td><td>Hash of the binary representation of the event</td></tr></tbody></table>

{% hint style="danger" %}
This page is a stub, more information needs to be added
{% endhint %}

