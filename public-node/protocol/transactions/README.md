# Transactions

| Tx type \# | Tx type name |
| :--- | :--- |
| 1 | GenesisTransaction |
| 4 | TransferTransaction |
| 8 | LeaseTransaction |
| 9 | LeaseCancelTransaction |
| 11 | MassTransferTransaction |
| ~~12~~ | ~~DataTransaction~~ |
| 15 | AnchorTransaction |
| 16 | InvokeAssociationTransaction |
| 17 | RevokeAssociationTransaction |
| 18 | SponsorTransaction |
| 19 | CancelSponsorTransaction |

### Genesis Transaction

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction type | Byte \(constant, value=1\) | 1 |
| 2 | Timestamp | Long | 8 |
| 3 | Recipient's address | Address | 26 |
| 4 | Amount | Long | 8 |

### Transfer Transaction V1

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

### Transfer Transaction V2

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

### Lease Transaction

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

### Lease Cancel Transaction

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

### Mass Transfer Transaction

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

### Anchor Transaction

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction multiple version mark | Byte \(constant, value = 0\) | 1 |
| 2 | Transaction type | Byte \(constant, value = 12\) | 1 |
| 3 | Version | Byte | 1 |
| 4 | Sender's public key | PublicKeyAccount \(Array\[Byte\]\) | 32 |
| 5.1 | Anchors count | Short | 2 |
| 5.2 | Anchor 1 | Array\[Byte\] | 64 |
| 5.3 | Anchor 2 | Array\[Byte\] | 64 |
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

### Invoke Association Transaction

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction multiple version mark | Byte \(constant, value=0\) | 1 |
| 2 | Transaction type | Byte \(constant, value=16\) | 1 |
| 3 | Sender's public key | PublicKeyAccount \(Array\[Byte\]\) | 32 |
| 4 | Party | Address | 26 |
| 5 | Association type | Int | 4 |
| 6 | Hash | Array\[Byte\] | 64 |
| 7 | Fee | Long | 8 |
| 8 | Timestamp | Long | 8 |
| 8.1 | Proofs version \(1\) | Byte | 1 |
| 8.2 | Proofs count | Short | 2 |
| 8.3 | Proof 1 length \(P1\) | Short | 2 |
| 8.4 | Proof 1 | ByteStr \(Array\[Byte\]\) | P1 |
| 8.5 | Proof 2 length \(P2\) | Short | 2 |
| 8.6 | Proof 2 | ByteStr \(Array\[Byte\]\) | P2 |
| ... | ... | ... | ... |

### Revoke Association Transaction

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction multiple version mark | Byte \(constant, value=0\) | 1 |
| 2 | Transaction type | Byte \(constant, value=17\) | 1 |
| 3 | Sender's public key | PublicKeyAccount \(Array\[Byte\]\) | 32 |
| 4 | Party | Address | 26 |
| 5 | Association type | Int | 4 |
| 6 | Hash | Array\[Byte\] | 64 |
| 7 | Fee | Long | 8 |
| 8 | Timestamp | Long | 8 |
| 8.1 | Proofs version \(1\) | Byte | 1 |
| 8.2 | Proofs count | Short | 2 |
| 8.3 | Proof 1 length \(P1\) | Short | 2 |
| 8.4 | Proof 1 | ByteStr \(Array\[Byte\]\) | P1 |
| 8.5 | Proof 2 length \(P2\) | Short | 2 |
| 8.6 | Proof 2 | ByteStr \(Array\[Byte\]\) | P2 |
| ... | ... | ... | ... |

### Sponsor Transaction

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction multiple version mark | Byte \(constant, value=0\) | 1 |
| 2 | Transaction type | Byte \(constant, value=18\) | 1 |
| 3 | Sender's public key | PublicKeyAccount \(Array\[Byte\]\) | 32 |
| 4 | Recipient | Address | 26 |
| 5 | Fee | Long | 8 |
| 6 | Timestamp | Long | 8 |
| 7.1 | Proofs version \(1\) | Byte | 1 |
| 7.2 | Proofs count | Short | 2 |
| 7.3 | Proof 1 length \(P1\) | Short | 2 |
| 7.4 | Proof 1 | ByteStr \(Array\[Byte\]\) | P1 |
| 7.5 | Proof 2 length \(P2\) | Short | 2 |
| 7.6 | Proof 2 | ByteStr \(Array\[Byte\]\) | P2 |
| ... | ... | ... | ... |

### Sponsor Cancel Transaction

| \# | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction multiple version mark | Byte \(constant, value=0\) | 1 |
| 2 | Transaction type | Byte \(constant, value=19\) | 1 |
| 3 | Sender's public key | PublicKeyAccount \(Array\[Byte\]\) | 32 |
| 4 | Recipient | Address | 26 |
| 5 | Fee | Long | 8 |
| 6 | Timestamp | Long | 8 |
| 7 | Lease ID | ByteStr \(Array\[Byte\]\) | 32 |
| 8.1 | Proofs version \(1\) | Byte | 1 |
| 8.2 | Proofs count | Short | 2 |
| 8.3 | Proof 1 length \(P1\) | Short | 2 |
| 8.4 | Proof 1 | ByteStr \(Array\[Byte\]\) | P1 |
| 8.5 | Proof 2 length \(P2\) | Short | 2 |
| 8.6 | Proof 2 | ByteStr \(Array\[Byte\]\) | P2 |
| ... | ... | ... | ... |



