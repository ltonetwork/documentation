# Data Structures

## Binary Data Structures

### Blockchain Objects

#### Address

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Version \(0x01\) | Byte | 1 |
| 2 | Address scheme \(0x54 for Testnet 0x57 for Mainnet\) | Byte | 1 |
| 3 | Public key hash | Bytes | 20 |
| 4 | Checksum | Bytes | 4 |

Public key hash is first 20 bytes of\_SecureHash\_of public key bytes. Checksum is first 4 bytes of\_SecureHash\_of version, scheme and hash bytes. SecureHash is hash function sha256\(Blake2b256\(data\)\).

#### Proof

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Proof size \(N\) | Short | 2 |
| 2 | Proof | Bytes | N |

#### Block

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

### Transactions

#### Transaction types

| Tx type \# | Tx type name |
| :--- | :--- |
| 1 | GenesisTransaction |
| 2 | TransferTransaction |
| 3 | LeaseTransaction |
| 4 | LeaseCancelTransaction |
| 5 | MassTransferTransaction |
| 6 | DataTransaction |

#### Genesis Transaction

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction type | Byte \(constant, value=1\) | 1 |
| 2 | Timestamp | Long | 8 |
| 3 | Recipient's address | Address | 26 |
| 4 | Amount | Long | 8 |

#### Transfer Transaction V1

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction type | Byte \(constant, value=4\) | 1 |
| 2 | Signature | ByteStr \(Array\[Byte\]\) | 64 |
| 3 | Transaction type | Byte \(constant, value=4\) | 1 |
| 4 | Sender's public key | PublicKeyAccount \(Array\[Byte\]\) | 32 |
| 5 | Timestamp | Long | 8 |
| 6 | Amount | Long | 8 |
| 7 | Fee | Long | 8 |
| 8 | Recipient | Address | 26 |
| 9 | Attachment length \(N\) | Short | 2 |
| 10 | Attachment | Array\[Byte\] | N |

The transaction's signature is calculated from the following bytes:

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction type | Byte \(constant, value=4\) | 1 |
| 2 | Sender's public key | PublicKeyAccount \(Array\[Byte\]\) | 32 |
| 3 | Timestamp | Long | 8 |
| 4 | Amount | Long | 8 |
| 5 | Fee | Long | 8 |
| 6 | Recipient | Address | 26 |
| 7 | Attachment length \(N\) | Short | 2 |
| 8 | Attachment | Array\[Byte\] | N |

#### Transfer Transaction V2

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction multiple version mark | Byte \(constant, value=0\) | 1 |
| 2 | Transaction type | Byte \(constant, value=4\) | 1 |
| 3 | Signature | ByteStr \(Array\[Byte\]\) | 64 |
| 4 | Transaction type | Byte \(constant, value=4\) | 1 |
| 5 | Sender's public key | PublicKeyAccount \(Array\[Byte\]\) | 32 |
| 6 | Timestamp | Long | 8 |
| 7 | Amount | Long | 8 |
| 8 | Fee | Long | 8 |
| 9 | Recipient | Address | 26 |
| 10 | Attachment length \(N\) | Short | 2 |
| 11 | Attachment | Array\[Byte\] | N |
| 12.1 | Proofs version \(1\) | Byte | 1 |
| 12.2 | Proofs count | Short | 2 |
| 12.3 | Proof 1 length \(P1\) | Short | 2 |
| 12.4 | Proof 1 | ByteStr \(Array\[Byte\]\) | P1 |
| 12.5 | Proof 2 length \(P2\) | Short | 2 |
| 12.6 | Proof 2 | ByteStr \(Array\[Byte\]\) | P2 |
| ... | ... | ... | ... |

* You may sign the transaction your way and place the signature in proofs.

#### Lease Transaction

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction multiple version mark | Byte \(constant, value=0\) | 1 |
| 2 | Transaction type | Byte \(constant, value=8\) | 1 |
| 3 | Sender's public key | PublicKeyAccount \(Array\[Byte\]\) | 32 |
| 4 | Recipient | Address | 26 |
| 5 | Amount | Long | 8 |
| 6 | Fee | Long | 8 |
| 7 | Timestamp | Long | 8 |
| 8.1 | Proofs version \(1\) | Byte | 1 |
| 8.2 | Proofs count | Short | 2 |
| 8.3 | Proof 1 length \(P1\) | Short | 2 |
| 8.4 | Proof 1 | ByteStr \(Array\[Byte\]\) | P1 |
| 8.5 | Proof 2 length \(P2\) | Short | 2 |
| 8.6 | Proof 2 | ByteStr \(Array\[Byte\]\) | P2 |
| ... | ... | ... | ... |

#### Lease Cancel Transaction

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction multiple version mark | Byte \(constant, value=0\) | 1 |
| 2 | Transaction type | Byte \(constant, value=9\) | 1 |
| 3 | Sender's public key | PublicKeyAccount \(Array\[Byte\]\) | 32 |
| 4 | Fee | Long | 8 |
| 5 | Timestamp | Long | 8 |
| 6 | Lease ID | ByteStr \(Array\[Byte\]\) | 32 |
| 7.1 | Proofs version \(1\) | Byte | 1 |
| 7.2 | Proofs count | Short | 2 |
| 7.3 | Proof 1 length \(P1\) | Short | 2 |
| 7.4 | Proof 1 | ByteStr \(Array\[Byte\]\) | P1 |
| 7.5 | Proof 2 length \(P2\) | Short | 2 |
| 7.6 | Proof 2 | ByteStr \(Array\[Byte\]\) | P2 |
| ... | ... | ... | ... |

#### Mass Transfer Transaction

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction type | Byte \(constant, value=11\) | 1 |
| 2 | Version | Byte \(constant, value=1\) | 1 |
| 3 | Sender's public key | PublicKeyAccount \(Array\[Byte\]\) | 32 |
| 4.1 | Number of transfers | Short | 2 |
| 4.2 | Address for transfer 1 | Address | 26 |
| 4.3 | Amount of transfer 1 | Long | 8 |
| 4.4 | Address for transfer 2 | Address | 26 |
| 4.5 | Amount of transfer 2 | Long | 8 |
| ... | ... | ... | ... |
| 5 | Timestamp | Long | 8 |
| 6 | Fee | Long | 8 |
| 7.1 | Attachments length \(N\) | Short | 2 |
| 7.2 | Attachments | Array\[Byte\] | N |
| 8.1 | Proofs version \(1\) | Byte | 1 |
| 8.2 | Proofs count | Short | 2 |
| 8.3 | Proof 1 length \(P1\) | Short | 2 |
| 8.4 | Proof 1 | ByteStr \(Array\[Byte\]\) | P1 |
| 8.5 | Proof 2 length \(P2\) | Short | 2 |
| 8.6 | Proof 2 | ByteStr \(Array\[Byte\]\) | P2 |
| ... | ... | ... | ... |

#### Data Transaction

| \` \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction multiple version mark | Byte \(constant, value = 0\) | 1 |
| 2 | Transaction type | Byte \(constant, value = 12\) | 1 |
| 3 | Version | Byte | 1 |
| 4 | Sender's public key | PublicKeyAccount \(Array\[Byte\]\) | 32 |
| 5.1 | Data entries count | Short | 2 |
| 5.2 | Key 1 length \(K1\) | Short | 2 |
| 5.3 | Key 1 bytes | UTF-8 encoded | K1 |
| 5.4 | Value 1 type \(0 = integer, 1 = boolean, 2 = binary array, 3 = string\) | Byte | 1 |
| 5.5 | Value 1 bytes | Value 1 type | depends on value type |
| ... | ... | ... | ... |
| 6 | Timestamp | Long | 8 |
| 7 | Fee | Long | 8 |
| 8.1 | Proofs version \(1\) | Byte | 1 |
| 8.2 | Proofs count | Short | 2 |
| 8.3 | Proof 1 length \(P1\) | Short | 2 |
| 8.4 | Proof 1 | ByteStr \(Array\[Byte\]\) | P1 |
| 8.5 | Proof 2 length \(P2\) | Short | 2 |
| 8.6 | Proof 2 | ByteStr \(Array\[Byte\]\) | P2 |
| ... | ... | ... | ... |

### Network messages

#### Network message structure

All network messages shares the same structure except the Handshake.

| \# | Field name | Type | Length in Bytes |
| :--- | :--- | :--- | :--- |
| 1 | Packet length \(BigEndian\) | Int | 4 |
| 2 | Magic Bytes | Bytes | 4 |
| 3 | Content ID | Byte | 1 |
| 4 | Payload length | Int | 4 |
| 5 | Payload checksum | Bytes | 4 |
| 6 | Payload | Bytes | N |

Magic Bytes are 0x12, 0x34, 0x56, 0x78. Payload checksum is first 4 bytes of\_FastHash\_of Payload bytes. FastHash is hash function Blake2b256\(data\).

#### Handshake message

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

#### GetPeers message

GetPeers message is sent when sending node wants to know of other nodes on network.

| \# | Field name | Type | Length in Bytes |
| :--- | :--- | :--- | :--- |
| 1 | Packet length \(BigEndian\) | Int | 4 |
| 2 | Magic Bytes | Bytes | 4 |
| 3 | Content ID \(0x01\) | Byte | 1 |
| 4 | Payload length | Int | 4 |
| 5 | Payload checksum | Bytes | 4 |

#### Peers message

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

#### GetSignatures message

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

#### Signatures message

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

#### GetBlock message

| \# | Field name | Type | Length in Bytes |
| :--- | :--- | :--- | :--- |
| 1 | Packet length \(BigEndian\) | Int | 4 |
| 2 | Magic Bytes | Bytes | 4 |
| 3 | Content ID \(0x16\) | Byte | 1 |
| 4 | Payload length | Int | 4 |
| 5 | Payload checksum | Bytes | 4 |
| 6 | Block ID | Bytes | 64 |

#### Block message

| \# | Field name | Type | Length in Bytes |
| :--- | :--- | :--- | :--- |
| 1 | Packet length \(BigEndian\) | Int | 4 |
| 2 | Magic Bytes | Bytes | 4 |
| 3 | Content ID \(0x17\) | Byte | 1 |
| 4 | Payload length | Int | 4 |
| 5 | Payload checksum | Bytes | 4 |
| 6 | Block bytes \(N\) | Bytes | N |

#### Score message

| \# | Field name | Type | Length in Bytes |
| :--- | :--- | :--- | :--- |
| 1 | Packet length \(BigEndian\) | Int | 4 |
| 2 | Magic Bytes | Bytes | 4 |
| 3 | Content ID \(0x18\) | Byte | 1 |
| 4 | Payload length | Int | 4 |
| 5 | Payload checksum | Bytes | 4 |
| 6 | Score \(N bytes\) | BigInt | N |

#### Transaction message

| \# | Field name | Type | Length in Bytes |
| :--- | :--- | :--- | :--- |
| 1 | Packet length \(BigEndian\) | Int | 4 |
| 2 | Magic Bytes | Bytes | 4 |
| 3 | Content ID \(0x19\) | Byte | 1 |
| 4 | Payload length | Int | 4 |
| 5 | Payload checksum | Bytes | 4 |
| 6 | Transaction \(N bytes\) | Bytes | N |

#### Checkpoint message

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

