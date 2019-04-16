# Resource

### id

A unique identifier for the resource.

The resource id MUST be calculated from event chain id. The `id` is a base58 encoded value from the following data structure:

| \# | Field name | Type | Position | Length |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Type \(0x60\) | Byte | 0 | 1 |
| 2 | Event chain id | Long | 1 | 20 |
| 3 | Public key hash | Bytes | 21 | 20 |
| 4 | Checksum | Bytes | 41 | 4 |

Public key hash is first 20 bytes of _SecureHash_ of public key bytes. Checksum is first 4 bytes of _SecureHash_ of version, random and hash bytes. SecureHash is hash function `sha256(Blake2b(data))`. The random number can be random number or generated from a nonce. This nonce might be the first 20 bytes of a `sha256` hash of a string.

