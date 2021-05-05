# HTTP Authentication

Signing HTTP Messages is described IETF draft [draft-cavage-http-signatures-10](https://tools.ietf.org/id/draft-cavage-http-signatures-10.html).

The [HTTP Authentication library](https://github.com/jasny/http-signature) can be used to sign and verify [PSR-7 requests](https://www.php-fig.org/psr/psr-7/#33-psrhttpmessageresponseinterface).

This library can be used in conjunction with the HTTP authentication library. The `keyId` should be the base58 encoded public key.

For `POST` and `PUT` requests, it's recommended to create an HTTP Digest \([RFC 3230](https://tools.ietf.org/html/rfc3230)\). This is a hash of the body, which manages to indirectly include the body in the signature. See the [HTTP Digest library](https://github.com/jasny/http-digest).

### **Creating the HTTP Authentication service**

```php
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

## **Server middleware**

Create server middleware to verify incoming requests.

The `LTO\Account\ServerMiddleware` can be used to set the `account` attribute for a server request that contains a `signature_key_id` attribute.

```php
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

## **Client middleware**

Create client middleware to sign outgoing requests.

```php
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

