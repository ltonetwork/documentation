---
description: Internal data structures of blockchain objects and network messages.
---

# Data Structures

## Blockchain Objects

### Signed transaction

#### Version 1 and 2

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction type | Byte \(constant, value=4\) | 1 |
| 2 | Version | Byte \(constant, value=2\) | 1 |
| ... | Transaction specific bytes |  |  |
| 3 | Proofs version | Byte \(constant, value=1\) | 1 |
| 5 | Proof size \(N\) | Short | 2 |
| 6 | Proof | Array\[Byte\] | N |
| ... |  |  |  |

#### Version 3

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction type | Byte \(constant, value=4\) | 1 |
| 2 | Version | Byte \(constant, value=2\) | 1 |
| 3 | Timestamp | Long | 8 |
| 4 | Sender's key type | KeyType \(Byte\) | 1 |
| 5 | Sender's public key | PublicKey \(Array\[Byte\]\) | 32 \| 33 |
| 6 | Fee | Long | 8 |
| ... | Transaction specific bytes |  |  |
| 7 | Sponsor's key type | KeyType \(Byte\) | 1 |
| 8 | Sponsor's public key | PublicKey \(Array\[Byte\]\) | 0 \| 32 \| 33 |
| 9 | Proofs version | Byte \(constant, value=1\) | 1 |
| 10 | Proof size \(N\) | Short | 2 |
| 11 | Proof | Array\[Byte\] | N |
| ... |  |  |  |

### Block

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Version \(0x02 for Genesis block, 0x03 for common block\) | Byte | 0 |
| 2 | Timestamp | Long | 1 |
| 3 | Parent block signature | Bytes | 64 |
| 4 | Consensus block length \(always 40 bytes\) | Int | 4 |
| 5 | Base target | Long | 8 |
| 6 | Generation signature\* | Bytes | 32 |
| 7 | Transactions block length \(N\) | Int | 8 |
| 8 | Transaction \#1 bytes | Bytes | M1 |
| ... | ... | ... | ... |
| 8 + \(K - 1\) | Transaction \#K bytes | Bytes | MK |
| 9 + \(K - 1\) | Generator's public key | Bytes | 32 |
| 10 + \(K - 1\) | Block's signature | Bytes | 64 |

Generation signature is calculated as Blake2b256 hash of the following bytes:

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Previous block's generation signature | Bytes | 32 |
| 2 | Generator's public key | Bytes | 32 |

Block's signature is calculated from the following bytes:

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Version \(0x02 for Genesis block, 0x03 for common block\) | Byte | 0 |
| 2 | Timestamp | Long | 1 |
| 3 | Parent block signature | Bytes | 64 |
| 4 | Consensus block length \(always 40 bytes\) | Int | 4 |
| 5 | Base target | Long | 8 |
| 6 | Generation signature\* | Bytes | 32 |
| 7 | Transactions block length \(N\) | Int | 8 |
| 8 | Transaction \#1 bytes | Bytes | M1 |
| ... | ... | ... | ... |
| 8 + \(K - 1\) | Transaction \#K bytes | Bytes | MK |
| 9 + \(K - 1\) | Generator's public key | Bytes | 32 |

## Network messages

### Network message structure

All network messages share the same structure, except the Handshake.

| \# | Field name | Type | Length in Bytes |
| :--- | :--- | :--- | :--- |
| 1 | Packet length \(BigEndian\) | Int | 4 |
| 2 | Magic Bytes | Bytes | 4 |
| 3 | Content ID | Byte | 1 |
| 4 | Payload length | Int | 4 |
| 5 | Payload checksum | Bytes | 4 |
| 6 | Payload | Bytes | N |

Magic Bytes are `0x12 0x34 0x56 0x78`. Payload checksum is the first 4 bytes of _FastHash_ of Payload bytes. _FastHash_ is the hash function `Blake2b256(data)`.

### Handshake message

Handshake is used to start communication between two nodes.

| \# | Field name | Type | Length in Bytes |
| :--- | :--- | :--- | :--- |
| 1 | Application name length \(N\) | Byte | 1 |
| 2 | Application name \(UTF-8 encoded bytes\) | Bytes | N |
| 3 | Application version major | Int | 4 |
| 4 | Application version minor | Int | 4 |
| 5 | Application version patch | Int | 4 |
| 6 | Node name length \(M\) | Byte | 1 |
| 7 | Node name \(UTF-8 encoded bytes\) | Bytes | M |
| 8 | Node nonce | Long | 8 |
| 9 | Declared address length \(K\) or 0 if no declared address was set | Int | 4 |
| 10 | Declared address bytes \(if length is not 0\) | Bytes | K |
| 11 | Timestamp | Long | 8 |

### GetPeers message

GetPeers message is sent when sending node wants to know of other nodes on network.

| \# | Field name | Type | Length in Bytes |
| :--- | :--- | :--- | :--- |
| 1 | Packet length \(BigEndian\) | Int | 4 |
| 2 | Magic Bytes | Bytes | 4 |
| 3 | Content ID \(0x01\) | Byte | 1 |
| 4 | Payload length | Int | 4 |
| 5 | Payload checksum | Bytes | 4 |

### Peers message

Peers message is a reply on GetPeers message.

| \# | Field name | Type | Length in Bytes |
| :--- | :--- | :--- | :--- |
| 1 | Packet length \(BigEndian\) | Int | 4 |
| 2 | Magic Bytes | Bytes | 4 |
| 3 | Content ID \(0x02\) | Byte | 1 |
| 4 | Payload length | Int | 4 |
| 5 | Payload checksum | Bytes | 4 |
| 6 | Peers count \(N\) | Int | 4 |
| 7 | Peer \#1 IP address | Bytes | 4 |
| 8 | Peer \#1 port | Int | 4 |
| ... | ... | ... | ... |
| 6 + 2 \* N - 1 | Peer \#N IP address | Bytes | 4 |
| 6 + 2 \* N | Peer \#N port | Int | 4 |

### GetSignatures message

| \# | Field name | Type | Length in Bytes |
| :--- | :--- | :--- | :--- |
| 1 | Packet length \(BigEndian\) | Int | 4 |
| 2 | Magic Bytes | Bytes | 4 |
| 3 | Content ID \(0x14\) | Byte | 1 |
| 4 | Payload length | Int | 4 |
| 5 | Payload checksum | Bytes | 4 |
| 6 | Block IDs count \(N\) | Int | 4 |
| 7 | Block \#1 ID | Bytes | 64 |
| ... | ... | ... | ... |
| 6 + N | Block \#N ID | Bytes | 64 |

### Signatures message

| \# | Field name | Type | Length in Bytes |
| :--- | :--- | :--- | :--- |
| 1 | Packet length \(BigEndian\) | Int | 4 |
| 2 | Magic Bytes | Bytes | 4 |
| 3 | Content ID \(0x15\) | Byte | 1 |
| 4 | Payload length | Int | 4 |
| 5 | Payload checksum | Bytes | 4 |
| 6 | Block signatures count \(N\) | Int | 4 |
| 7 | Block \#1 signature | Bytes | 64 |
| ... | ... | ... | ... |
| 6 + N | Block \#N signature | Bytes | 64 |

### GetBlock message

| \# | Field name | Type | Length in Bytes |
| :--- | :--- | :--- | :--- |
| 1 | Packet length \(BigEndian\) | Int | 4 |
| 2 | Magic Bytes | Bytes | 4 |
| 3 | Content ID \(0x16\) | Byte | 1 |
| 4 | Payload length | Int | 4 |
| 5 | Payload checksum | Bytes | 4 |
| 6 | Block ID | Bytes | 64 |

### Block message

| \# | Field name | Type | Length in Bytes |
| :--- | :--- | :--- | :--- |
| 1 | Packet length \(BigEndian\) | Int | 4 |
| 2 | Magic Bytes | Bytes | 4 |
| 3 | Content ID \(0x17\) | Byte | 1 |
| 4 | Payload length | Int | 4 |
| 5 | Payload checksum | Bytes | 4 |
| 6 | Block bytes \(N\) | Bytes | N |

### Score message

| \# | Field name | Type | Length in Bytes |
| :--- | :--- | :--- | :--- |
| 1 | Packet length \(BigEndian\) | Int | 4 |
| 2 | Magic Bytes | Bytes | 4 |
| 3 | Content ID \(0x18\) | Byte | 1 |
| 4 | Payload length | Int | 4 |
| 5 | Payload checksum | Bytes | 4 |
| 6 | Score \(N bytes\) | BigInt | N |

### Transaction message

| \# | Field name | Type | Length in Bytes |
| :--- | :--- | :--- | :--- |
| 1 | Packet length \(BigEndian\) | Int | 4 |
| 2 | Magic Bytes | Bytes | 4 |
| 3 | Content ID \(0x19\) | Byte | 1 |
| 4 | Payload length | Int | 4 |
| 5 | Payload checksum | Bytes | 4 |
| 6 | Transaction \(N bytes\) | Bytes | N |

### Checkpoint message

| \# | Field name | Type | Length in Bytes |
| :--- | :--- | :--- | :--- |
| 1 | Packet length \(BigEndian\) | Int | 4 |
| 2 | Magic Bytes | Bytes | 4 |
| 3 | Content ID \(0x64\) | Byte | 1 |
| 4 | Payload length | Int | 4 |
| 5 | Payload checksum | Bytes | 4 |
| 6 | Checkpoint items count \(N\) | Int | 4 |
| 7 | Checkpoint \#1 height | Long | 8 |
| 8 | Checkpoint \#1 signature | Bytes | 64 |
| ... | ... | ... | ... |
| 6 + 2 \* N - 1 | Checkpoint \#N height | Long | 8 |
| 6 + 2 \* N | Checkpoint \#N signature | Bytes | 64 |

