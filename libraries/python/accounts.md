# Accounts

## Creation

### Create an account

```
from lto import LTO

account = LTO(chain_id).Account()
```

### Create an account from seed

```
from lto import LTO

account = LTO(chain_id).Account(seed=my_seed)
```

### Create an account from public key

```
from lto import LTO

account = LTO(chain_id).Account(public_key=my_pub_key)
```

### Create an account from private key

```
from lto import LTO

account = LTO(chain_id).Account(provate_key=my_priv_key)
```

## Different encryption algorithms

### If not specified EdDSA is default:&#x20;

**ed25519**

```
account = LTO(chain_id).Account()
```

### For ECDSA we have two available curves:

**secp256k1**

```
account = LTO(chain_id).Account(key_type = "secp256k1")
```

**secp256r1**

```
account = LTO(chain_id).Account(key_type = "secp256r1")
```



## Signing

### Signing a message

We first need to convert the string to bytes using our crypto library

```
from lto import crypto

message = crypto.str2bytes("test")
signature = account.sign(message)
```

### Verifying a message

```
account.verify_signature(message, signature)
```
