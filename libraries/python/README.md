---
description: Python client library for interacting with LTO Network
---

# Python

{% hint style="info" %}
Visit the [project on Github](https://github.com/ltonetwork/lto-api.python)
{% endhint %}

## Installation

```shell
pip install lto
```

## Usage

```python
from lto import LTO
from lto.public_node import PublicNode

seed = "manage manual recall harvest series desert melt police rose hollow moral pledge kitten position add"

account = LTO("T").Account(seed='seed')
node = PublicNode("https://testnet.lto.network")

amount = 1000.0 # Amount of LTO to transfer
recipient = "3Jo1JCrBvnWCg37VDxMXAjYhsS9rRDLBSze"

transaction = Transfer(recipient, amount)
transaction.sign_with(account)
transaction.broadcast_to(node)
```

To check the response from the node is easy, just modify the last line and add a print

```python
response = transaction.broadcast_to(node)
print(response.to_json())
```
