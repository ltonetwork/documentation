---
description: PHP client library for interacting with LTO Network
---

# PHP

{% hint style="success" %}
Visit the [project on GitHub](https://github.com/ltonetwork/lto-api.php).
{% endhint %}

_Signing and addresses work both for the \(private\) event chain as for the public chain._

## Installation

```text
composer require lto/api
```

## Accounts

### 

## Public layer

```text

```

## Private layer

### Event chain

#### **Create a new event chain**

```text
$chain = $account->createEventChain(); // Creates an empty event chain with a valid id and last hash
```

_Note: You need to add an identity as first event on the chain. This is **not** done automatically._

#### **Create and sign an event and add it to an existing event chain**

```text
$body = [
  '$schema' => "http://specs.example.com/message#",
  'content' => "Hello world!"
];

$chainId = "JEKNVnkbo3jqSHT8tfiAKK4tQTFK7jbx8t18wEEnygya";
$chainLastHash = "3yMApqCuCjXDWPrbjfR5mjCPTHqFG8Pux1TxQrEM35jj";

$chain = new LTO\EventChain($chainId, $chainLastHash);

$chain->add(new Event($body))->signWith($account);
```

You need the chain id and the hash of the last event to use an existing chain.

### HTTP Authentication



## 

