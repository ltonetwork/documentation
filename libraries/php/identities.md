# Identities

The LTO identity library supports resolving DIDs to DID documents. It's DID method independent.

## Resolver

```php
use LTO\Identity\Resolver\LTONetwork as LTOResolver;

$resolver = new LTOResolver();

$identity = $resolver->resolve("lto:did:3JkihPXS5iPs8xDKcFQmhR8TVcAmDRUZpyx");
```

The Resolver class can be used to combine multiple resolvers.

```php
use LTO\Identity\Resolver;
use LTO\Identity\Resolver\LTONetwork as LTOResolver;
use LTO\Identity\Resolver\WebNetwork as WebResolver;

$resolver = new Resolver(
   new LTOResolver(),
   new WebResolver(),
);

$identity = $resolver->resolve("lto:did:3JkihPXS5iPs8xDKcFQmhR8TVcAmDRUZpyx");
```

## Identity

The identity is a representation of a DID document.

After a signature or JWT is verified, use `hasVerificationMethod()` to check if the public key used in the verification belongs to the identity.

```php
$method = [
    "type": "Ed25519VerificationKey2018", 
    "publicKeyBase58": "H3C2AVvLMv6gmMNam3uVAjZpfkcJCwDwnZn6z3wXmqPV",
];

$identity->hasVerificationMethod($method);
```

Optionally check if the verification method can be used for a specific purpose

```php
use LTO\Identity\VerificationMethod;

$identity->hasVerificationMethod($method, VerificationMethod\Authentication);
```

