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

### Creation

#### **Create an account from seed**

```text
$seedText = "manage manual recall harvest series desert melt police rose hollow moral pledge kitten position add";

$factory = new LTO\AccountFactory('T'); // 'T' for testnet, 'L' for mainnet
$account = $factory->seed($seedText);
```

#### **Create an account from sign key**

```text
$secretKey = 'wJ4WH8dD88fSkNdFQRjaAhjFUZzZhV5yiDLDwNUnp6bYwRXrvWV8MJhQ9HL9uqMDG1n7XpTGZx7PafqaayQV8Rp';

$factory = new LTO\AccountFactory('T'); // 'T' for testnet, 'L' for mainnet
$account = $factory->create($secretKey);
```

#### **Create an account from full info**

```text
$accountInfo = [
  'address' => '3PLSsSDUn3kZdGe8qWEDak9y8oAjLVecXV1',
  'sign' => [
    'secretkey' => 'wJ4WH8dD88fSkNdFQRjaAhjFUZzZhV5yiDLDwNUnp6bYwRXrvWV8MJhQ9HL9uqMDG1n7XpTGZx7PafqaayQV8Rp',
    'publickey' => 'FkU1XyfrCftc4pQKXCrrDyRLSnifX1SMvmx1CYiiyB3Y'
  ],
  'encrypt' => [
    'secretkey' => 'BnjFJJarge15FiqcxrB7Mzt68nseBXXR4LQ54qFBsWJN',
    'publickey' => 'BVv1ZuE3gKFa6krwWJQwEmrLYUESuUabNCXgYTmCoBt6'
  ]
];

$factory = new LTO\AccountFactory('T'); // 'T' for testnet, 'L' for mainnet
$account = $factory->create($accountInfo);
```

Properties that are specified will be verified. Properties that are omitted will be generated where possible.

### Signing \(ED25519\)

#### **Sign a message**

```text
$signature = $account->sign('hello world'); // Base58 encoded signature
```

#### **Verify a signature**

```text
if (!$account->verify($signature, 'hello world')) {
    throw new RuntimeException('invalid signature');
}
```

### Encryption \(X25519\)

#### **Encrypt a message for another account**

```text
$message = 'hello world';

$recipientPublicKey = "HBqhfdFASRQ5eBBpu2y6c6KKi1az6bMx8v1JxX4iW1Q8"; // base58 encoded X25519 public key
$recipient = $factory->createPublic(null, $recipientPublicKey);

$cyphertext = $account->encryptFor($recipient, $message); // Raw binary, not encoded
```

You can use `$account->encryptFor($account, $message);` to encrypt a message for yourself.

#### **Decrypt a message received from another account**

```text
$senderPublicKey = "HBqhfdFASRQ5eBBpu2y6c6KKi1az6bMx8v1JxX4iW1Q8"; // base58 encoded X25519 public key
$sender = $factory->createPublic(null, $senderPublicKey);

$message = $account->decryptFrom($sender, $cyphertext);
```

You can use `$account->decryptFrom($account, $message);` to decrypt a message from yourself.

## Public layer

```text
use LTO\Transaction\Transfer;
use LTO\PublicNode;

$node = new PublicNode('https://nodes.lto.network');

$amount = 1000.0; // Amount of LTO to transfer
$recipient = "3Jo1JCrBvnWCg37VDxMXAjYhsS9rRDLBSze";

$transferTx = (new Transfer($amount, $recipient))
    ->signWith($account)
    ->broadcastTo($node);
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

Signing HTTP Messages is described IETF draft [draft-cavage-http-signatures-10](https://tools.ietf.org/id/draft-cavage-http-signatures-10.html).

The [HTTP Authentication library](https://github.com/jasny/http-signature) can be used to sign and verify [PSR-7 requests](https://www.php-fig.org/psr/psr-7/#33-psrhttpmessageresponseinterface).

This library can be used in conjunction with the HTTP authentication library. The `keyId` should be the base58 encoded public key.

For `POST` and `PUT` requests, it's recommended to create an HTTP Digest \([RFC 3230](https://tools.ietf.org/html/rfc3230)\). This is a hash of the body, which manages to indirectly include the body in the signature. See the [HTTP Digest library](https://github.com/jasny/http-digest).

#### **Creating the HTTP Authentication service**

```text
use Jasny\HttpSignature\HttpSignature;

$secretKey = 'wJ4WH8dD88fSkNdFQRjaAhjFUZzZhV5yiDLDwNUnp6bYwRXrvWV8MJhQ9HL9uqMDG1n7XpTGZx7PafqaayQV8Rp';

$factory = new LTO\AccountFactory('T'); // 'T' for testnet, 'L' for mainnet
$ourAccount = $factory->create($secretKey);

$service = new HttpSignature(
    ['ed25519', 'ed25519-sha256'],
    new SignCallback($ourAccount),
    new VerifyCallback($accountFactory)
);
```

#### **Server middleware**

Create server middleware to verify incoming requests.

The `LTO\Account\ServerMiddleware` can be used to set the `account` attribute for a server request that contains a `signature_key_id` attribute.

```text
use Jasny\HttpDigest\HttpDigest;
use Jasny\HttpDigest\ServerMiddleware as DigestMiddleware;
use Jasny\HttpDigest\Negitiation\DigestNegotiator;
use Jasny\HttpSignature\HttpSignature;
use Jasny\HttpSignature\ServerMiddleware as SignatureMiddleware;
use LTO\Account\ServerMiddleware as AccountMiddleware;
use Relay\RelayBuilder;

$factory = new LTO\AccountFactory('T'); // 'T' for testnet, 'L' for mainnet
$ourAccount = $factory->create($secretKey);

$digestService = HttpDigest(new DigestNegotiator(), ["SHA-256"]);

$signatureService = new HttpSignature(
    ['ed25519', 'ed25519-sha256'],
    function() { throw new \LogicException('sign not supported'); },
    new VerifyCallback($accountFactory)
);

$relayBuilder = new RelayBuilder($resolver);
$relay = $relayBuilder->newInstance([
    (new DigestMiddleware($digestService))->asDoublePass(),
    (new SignatureMiddleware($signatureService))->asDoublePass(),
    (new AccountMiddleware($factory))->asDoublePass(),
]);
```

The server middleware implements the PSR-15 `MiddlewareInterface` for single pass support and returns a callback for double pass with the `asDoublePass()` method.

#### **Client middleware**

Create client middleware to sign outgoing requests.

```text
use GuzzleHttp\HandlerStack;
use GuzzleHttp\Client;
use Jasny\HttpDigest\HttpDigest;
use Jasny\HttpDigest\ClientMiddleware as DigestMiddleware;
use Jasny\HttpDigest\Negitiation\DigestNegotiator;
use Jasny\HttpSignature\HttpSignature;
use Jasny\HttpSignature\ClientMiddleware as SignatureMiddleware;

$secretKey = 'wJ4WH8dD88fSkNdFQRjaAhjFUZzZhV5yiDLDwNUnp6bYwRXrvWV8MJhQ9HL9uqMDG1n7XpTGZx7PafqaayQV8Rp';

$factory = new LTO\AccountFactory('T'); // 'T' for testnet, 'L' for mainnet
$ourAccount = $factory->create($secretKey);

$digestService = HttpDigest(new DigestNegotiator(), ["SHA-256"]);

$signatureService = new HttpSignature(
    ['ed25519', 'ed25519-sha256'],
    new SignCallback($ourAccount),
    function() { throw new \LogicException('verify not supported'); }
);

$signatureMiddleware = new SignatureMiddleware(
    $service->withAlgorithm('ed25519-sha256'),
    $ourAccount->getPublicKey()
);

$stack = new HandlerStack();
$stack->push((new DigestMiddleware($digestService))->forGuzzle());
$stack->push($signatureMiddleware->forGuzzle());

$client = new Client(['handler' => $stack]);
```

## Commandline scripts

### Generate account from seed

Get the seed from `stdin` to generate account info.

```text
$ vendor/bin/lto-seed <<< "manage manual recall harvest series desert melt police rose hollow moral pledge kitten position add"

address: 3JmCa4jLVv7Yn2XkCnBUGsa7WNFVEMxAfWe
sign:
  secretkey: 4zsR9xoFpxfnNwLcY4hdRUarwf5xWtLj6FpKGDFBgscPxecPj2qgRNx4kJsFCpe9YDxBRNoeBWTh2SDAdwTySomS
  publickey: GjSacB6a5DFNEHjDSmn724QsrRStKYzkahPH67wyrhAY
encrypt:
  secretkey: 4q7HKMbwbLcG58iFV3pz4vkRnPTwbrY9Q5JrwnwLEZCC
  publickey: 6fDod1xcVj4Zezwyy3tdPGHkuDyMq8bDHQouyp5BjXsX
```

By default a mainnet \(`L`\) address is generated. Pass `T` as argument to create a testnet address.

```text
$ vendor/bin/lto-seed T <<< "manage manual recall harvest series desert melt police rose hollow moral pledge kitten position add"
```

### Account info

Show account info from the secret key. The secret key should be base58 encoded.

```text
$ vendor/bin/lto-account 4zsR9xoFpxfnNwLcY4hdRUarwf5xWtLj6FpKGDFBgscPxecPj2qgRNx4kJsFCpe9YDxBRNoeBWTh2SDAdwTySomS

address: 3JmCa4jLVv7Yn2XkCnBUGsa7WNFVEMxAfWe
sign:
  secretkey: 4zsR9xoFpxfnNwLcY4hdRUarwf5xWtLj6FpKGDFBgscPxecPj2qgRNx4kJsFCpe9YDxBRNoeBWTh2SDAdwTySomS
  publickey: GjSacB6a5DFNEHjDSmn724QsrRStKYzkahPH67wyrhAY
encrypt:
  secretkey: 4q7HKMbwbLcG58iFV3pz4vkRnPTwbrY9Q5JrwnwLEZCC
  publickey: 6fDod1xcVj4Zezwyy3tdPGHkuDyMq8bDHQouyp5BjXsX
```

By default a mainnet \(`L`\) address is generated. Pass `T` as third argument to create a testnet address.

```text
$ vendor/bin/lto-seed 4zsR9xoFpxfnNwLcY4hdRUarwf5xWtLj6FpKGDFBgscPxecPj2qgRNx4kJsFCpe9YDxBRNoeBWTh2SDAdwTySomS T
```

### Sign message

Sign a message \(from `stdin`\). The secret key should be base58 encoded. Outputs the signature and hash \(`base58` encoded\).

```text
$ cat message.json | vendor/bin/lto-sign 4zsR9xoFpxfnNwLcY4hdRUarwf5xWtLj6FpKGDFBgscPxecPj2qgRNx4kJsFCpe9YDxBRNoeBWTh2SDAdwTySomS

signature: 5i9gBaHwg9UFPuwU63LBdBR29yZdRDstWM9z7oo8GzevWhBdAAWwCSRUQbPLaCT3nFgjbQuuWxVQckzCd3CoFig4
hash:      42TEXg1vFAbcJ65y7qdYG9iCPvYfy3NDdVLd75akX2P5
```

