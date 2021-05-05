---
description: PHP client library for interacting with LTO Network
---

# PHP

{% hint style="success" %}
Visit the [project on GitHub](https://github.com/ltonetwork/lto-api.php).
{% endhint %}

## Installation

```text
composer require lto/api
```

## Usage

```php
use LTO\Transaction\Transfer;
use LTO\PublicNode;

// Create account for signing
$seedText = "manage manual recall harvest series desert melt police rose hollow moral pledge kitten position add";

$factory = new LTO\AccountFactory('T'); // 'T' for testnet, 'L' for mainnet
$account = $factory->seed($seedText);

// Public layer
$node = new PublicNode('https://nodes.lto.network');

$amount = 1000.0; // Amount of LTO to transfer
$recipient = "3Jo1JCrBvnWCg37VDxMXAjYhsS9rRDLBSze";

$transferTx = (new Transfer($amount, $recipient))
    ->signWith($account)
    ->broadcastTo($node);
    
// Private layer
$schema = "http://specs.example.com/message";
$content => "Hello world!";

$chain = $account->createEventChain();
$chain->addIdentity($account->asIdentity())->signWith($account);
$chain->addEvent($schema, $content)->signWith($account);

```

