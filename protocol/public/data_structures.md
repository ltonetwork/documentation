---
description: Internal data structures of blockchain objects and network messages.
---

# Data Structures

## Blockchain Objects

### Signed transaction

#### Version 1 and 2

|          Field Name          |           Type           | Length |
| :--------------------------: | :----------------------: | ------ |
|       Transaction type       |           Byte           | 1      |
|            Version           |           Byte           | 1      |
| _Transaction specific bytes_ |                          |        |
|        Proofs version        | Byte (constant, value=1) | 1      |
|        Proof size (N)        |           Short          | 2      |
|             Proof            |       Array\[Byte]       | N      |

#### Version 3+

|          Field Name          |           Type           | Length        |
| :--------------------------: | :----------------------: | ------------- |
|       Transaction type       |           Byte           | 1             |
|            Version           |           Byte           | 1             |
|           Timestamp          |           Long           | 8             |
|       Sender's key type      |      KeyType (Byte)      | 1             |
|      Sender's public key     | PublicKey (Array\[Byte]) | 32 \| 33      |
|              Fee             |           Long           | 8             |
| _Transaction specific bytes_ |                          |               |
|      Sponsor's key type      |      KeyType (Byte)      | 1             |
|     Sponsor's public key     | PublicKey (Array\[Byte]) | 0 \| 32 \| 33 |
|        Proofs version        | Byte (constant, value=1) | 1             |
|        Proof size (N)        |           Short          | 2             |
|             Proof            |       Array\[Byte]       | N             |

{% hint style="info" %}
In version 3+ more fields are standardized. These are part of the transaction-specific bytes in versions 1 and 2.
{% endhint %}

### Block

|                        Field Name                       |  Type | Length |
| :-----------------------------------------------------: | :---: | ------ |
| Version (0x02 for Genesis block, 0x03 for common block) |  Byte | 0      |
|                        Timestamp                        |  Long | 1      |
|                  Parent block signature                 | Bytes | 64     |
|         Consensus block length (always 40 bytes)        |  Int  | 4      |
|                       Base target                       |  Long | 8      |
|                  Generation signature\*                 | Bytes | 32     |
|              Transactions block length (N)              |  Int  | 8      |
|                   Transaction #1 bytes                  | Bytes | M1     |
|                           ...                           |  ...  | ...    |
|                   Transaction #K bytes                  | Bytes | MK     |
|                  Generator's public key                 | Bytes | 32     |
|                    Block's signature                    | Bytes | 64     |

Generation signature is calculated as `blake2b256` hash of the following bytes:

|               Field Name              |  Type | Length |
| :-----------------------------------: | :---: | ------ |
| Previous block's generation signature | Bytes | 32     |
|         Generator's public key        | Bytes | 32     |

Block's signature is calculated from the following bytes:

|                        Field Name                       |  Type | Length |
| :-----------------------------------------------------: | :---: | ------ |
| Version (0x02 for Genesis block, 0x03 for common block) |  Byte | 0      |
|                        Timestamp                        |  Long | 1      |
|                  Parent block signature                 | Bytes | 64     |
|         Consensus block length (always 40 bytes)        |  Int  | 4      |
|                       Base target                       |  Long | 8      |
|                  Generation signature\*                 | Bytes | 32     |
|              Transactions block length (N)              |  Int  | 8      |
|                   Transaction #1 bytes                  | Bytes | M1     |
|                           ...                           |  ...  | ...    |
|                   Transaction #K bytes                  | Bytes | MK     |
|                  Generator's public key                 | Bytes | 32     |

## Network messages

### Network message structure

All network messages share the same structure, except the Handshake.

|         Field name        |  Type | Length |
| :-----------------------: | :---: | ------ |
| Packet length (BigEndian) |  Int  | 4      |
|        Magic Bytes        | Bytes | 4      |
|         Content ID        |  Byte | 1      |
|       Payload length      |  Int  | 4      |
|      Payload checksum     | Bytes | 4      |
|          Payload          | Bytes | N      |

Magic Bytes are `0x12 0x34 0x56 0x78`. Payload checksum is the first 4 bytes of _FastHash_ of Payload bytes. _FastHash_ is the hash function `Blake2b256(data)`.

### Handshake message

Handshake is used to start communication between two nodes.

|                            Field name                           |  Type | Length |
| :-------------------------------------------------------------: | :---: | ------ |
|                   Application name length (N)                   |  Byte | 1      |
|              Application name (UTF-8 encoded bytes)             | Bytes | N      |
|                    Application version major                    |  Int  | 4      |
|                    Application version minor                    |  Int  | 4      |
|                    Application version patch                    |  Int  | 4      |
|                       Node name length (M)                      |  Byte | 1      |
|                 Node name (UTF-8 encoded bytes)                 | Bytes | M      |
|                            Node nonce                           |  Long | 8      |
| Declared address length (K) or 0 if no declared address was set |  Int  | 4      |
|           Declared address bytes (if length is not 0)           | Bytes | K      |
|                            Timestamp                            |  Long | 8      |

### GetPeers message

GetPeers message is sent when sending node wants to know of other nodes on network.

|         Field name        |  Type | Length |
| :-----------------------: | :---: | ------ |
| Packet length (BigEndian) |  Int  | 4      |
|        Magic Bytes        | Bytes | 4      |
|     Content ID (0x01)     |  Byte | 1      |
|       Payload length      |  Int  | 4      |
|      Payload checksum     | Bytes | 4      |

### Peers message

Peers message is a reply on GetPeers message.

|         Field name        |  Type | Length |
| :-----------------------: | :---: | ------ |
| Packet length (BigEndian) |  Int  | 4      |
|        Magic Bytes        | Bytes | 4      |
|     Content ID (0x02)     |  Byte | 1      |
|       Payload length      |  Int  | 4      |
|      Payload checksum     | Bytes | 4      |
|      Peers count (N)      |  Int  | 4      |
|     Peer #1 IP address    | Bytes | 4      |
|        Peer #1 port       |  Int  | 4      |
|            ...            |  ...  | ...    |
|     Peer #N IP address    | Bytes | 4      |
|        Peer #N port       |  Int  | 4      |

### GetSignatures message

|         Field name        |  Type | Length |
| :-----------------------: | :---: | ------ |
| Packet length (BigEndian) |  Int  | 4      |
|        Magic Bytes        | Bytes | 4      |
|     Content ID (0x14)     |  Byte | 1      |
|       Payload length      |  Int  | 4      |
|      Payload checksum     | Bytes | 4      |
|    Block IDs count (N)    |  Int  | 4      |
|        Block #1 ID        | Bytes | 64     |
|            ...            |  ...  | ...    |
|        Block #N ID        | Bytes | 64     |

### Signatures message

|         Field name         |  Type | Length |
| :------------------------: | :---: | ------ |
|  Packet length (BigEndian) |  Int  | 4      |
|         Magic Bytes        | Bytes | 4      |
|      Content ID (0x15)     |  Byte | 1      |
|       Payload length       |  Int  | 4      |
|      Payload checksum      | Bytes | 4      |
| Block signatures count (N) |  Int  | 4      |
|     Block #1 signature     | Bytes | 64     |
|             ...            |  ...  | ...    |
|     Block #N signature     | Bytes | 64     |

### GetBlock message

|         Field name        |  Type | Length |
| :-----------------------: | :---: | ------ |
| Packet length (BigEndian) |  Int  | 4      |
|        Magic Bytes        | Bytes | 4      |
|     Content ID (0x16)     |  Byte | 1      |
|       Payload length      |  Int  | 4      |
|      Payload checksum     | Bytes | 4      |
|          Block ID         | Bytes | 64     |

### Block message

|         Field name        |  Type | Length |
| :-----------------------: | :---: | ------ |
| Packet length (BigEndian) |  Int  | 4      |
|        Magic Bytes        | Bytes | 4      |
|     Content ID (0x17)     |  Byte | 1      |
|       Payload length      |  Int  | 4      |
|      Payload checksum     | Bytes | 4      |
|      Block bytes (N)      | Bytes | N      |

### Score message

|         Field name        |  Type  | Length |
| :-----------------------: | :----: | ------ |
| Packet length (BigEndian) |   Int  | 4      |
|        Magic Bytes        |  Bytes | 4      |
|     Content ID (0x18)     |  Byte  | 1      |
|       Payload length      |   Int  | 4      |
|      Payload checksum     |  Bytes | 4      |
|      Score (N bytes)      | BigInt | N      |

### Transaction message

|         Field name        |  Type | Length |
| :-----------------------: | :---: | ------ |
| Packet length (BigEndian) |  Int  | 4      |
|        Magic Bytes        | Bytes | 4      |
|     Content ID (0x19)     |  Byte | 1      |
|       Payload length      |  Int  | 4      |
|      Payload checksum     | Bytes | 4      |
|   Transaction (N bytes)   | Bytes | N      |

### Checkpoint message

|         Field name         |  Type | Length |
| :------------------------: | :---: | ------ |
|  Packet length (BigEndian) |  Int  | 4      |
|         Magic Bytes        | Bytes | 4      |
|      Content ID (0x64)     |  Byte | 1      |
|       Payload length       |  Int  | 4      |
|      Payload checksum      | Bytes | 4      |
| Checkpoint items count (N) |  Int  | 4      |
|    Checkpoint #1 height    |  Long | 8      |
|   Checkpoint #1 signature  | Bytes | 64     |
|             ...            |  ...  | ...    |
|    Checkpoint #N height    |  Long | 8      |
|   Checkpoint #N signature  | Bytes | 64     |
