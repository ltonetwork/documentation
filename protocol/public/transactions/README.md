# Transactions

| Tx type \# | Tx type name |
| :--- | :--- |
| 1 | Genesis Transaction |
| 4 | [Transfer Transaction](transfer-transaction.md) |
| 8 | [Lease Transaction](../../../public-node-1/rest-api/lease_transactions.md) |
| 9 | [Cancel Lease Transaction](cancel-lease-transaction.md) |
| 11 | [Mass Transfer Transaction](mass_transfer_transaction.md) |
| 13 | Set Script Transaction |
| 15 | [Anchor Transaction](anchor.md) |
| 16 | [Association Transaction](association.md) |
| 17 | [Revoke Association Transaction](revoke-association.md) |
| 18 | [Sponsor Transaction](sponsor.md) |
| 19 | [Cancel Sponsor Transaction](cancel-sponsor.md) |

## Transaction Fees

Transaction fees act as a reward for the miner. Because these are the only rewards, this prevents inflation of the network since no new tokens are introduced. 0.1 LTO of the fee isn't awarded \(aka burned\), resulting in deflation of the network.

| Transaction | Fee \(LTO\) | Minimum \(LTO\) | Burned |
| :--- | :--- | :--- | :--- |
| Transfer | 1 | 0.1 | 0.1 |
| Lease | 1 | 0.1 | 0.1 |
| Cancel Lease | 1 | 0.1 | 0.1 |
| Mass Transfer | 1 + 0.1 \* N | 0.1 + 0.1 \* N | 0.1 |
| Anchor | 0.35 | 0.1 | 0.1 |
| Invoke Association | 1 | 0.1 | 0.1 |
| Revoke Association | 1 | 0.1 | 0.1 |
| Sponsor | 5 | 5 | 0.1 |
| Cancel Sponsor | 5 | 5 | 0.1 |
| Script | 5 | 5 | 0.1 |

The absolute minimum fees are enforced by the consensus model. The current fees are configured by the nodes as the minimum acceptable fee.

Nodes will reject broadcasting transactions that offer a lower fee than configured. However, when running your own node, it's possible to offer any fee equal to or above the minimum. Mining nodes will not process transactions with a fee that's lower than configured, so likely these transactions will stay in the utx pool until your own node is able to mine or until they time out \(after 90 minutes\).

### Fee distribution

For every transaction 0.1 LTO isn't awarded and thus effectively taken out of circulation \(aka burned\). The remaining fee is split up among the current leader, and the next elected node. The fee is distributed 40% to the leader and 60% to the next one.

For more information see the [NG documentation on Waves.](https://docs.waves.tech/en/blockchain/waves-protocol/waves-ng-protocol)

### Sponsored accounts

Normally the fee is automatically deducted from the sender's address. With sponsored accounts, it's possible for a third party to pay for all transaction fees of an account.

If the sponsor has insufficient funds, the fee is deducted from the sender's account. This prevents a third party to disable an account through a dummy sponsorship.

If an account has multiple sponsors, it works as last in first out. If the most recently added sponsor has insufficient funds, the next sponsor is tried, this continues until the sender's account is charged for the fee.

## Signing a transaction

`ED25519` is used for all the signatures in the project.

The process is as follows: create the special bytes for signing \(for transaction or block, you can find it [here](https://github.com/ltonetwork/documentation/tree/c01951988c8797dc36ac6098133b139eaffade7c/technical-details/data-structures.md)\), then create a signature using these bytes and the private key bytes.

For the validation of signature is enough signature bytes, signed object bytes and the public key.

{% hint style="warning" %}
There are many valid \(not unique!\) signatures for the same array of bytes \(block or transaction\).
{% endhint %}

### Example

Transaction data:

| Field | Value |
| :--- | :--- |
| Sender address \(not used, just for information\) | 3N9Q2sdkkhAnbR4XCveuRaSMLiVtvebZ3wp |
| Private key \(used for signing, not in tx data\) | 7VLYNhmuvAo5Us4mNGxWpzhMSdSSdEbEPFUDKSnA6eBv |
| Public key | EENPV1mRhUD9gSKbcWt84cqnfSGQP5LkCu5gMBfAanYH |
| Recipient address | 3NBVqYXrapgJP9atQccdBPAgJPwHDKkh6A8 |
| Amount | 1 |
| Fee | 1 |
| Fee asset id | BG39cCNUFWPQYeyLnu7tjKHaiUGRxYwJjvntt9gdDPxG |
| Timestamp | 1479287120875 |
| Attachment \(as byte array\) | \[1, 2, 3, 4\] |

Bytes:

| \# | Field name | Type | Position | Length | Value | Base58 bytes value |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Transaction type \(0x04\) | Byte | 0 | 1 | 4 | 5 |
| 2 | Sender's public key | Bytes | 1 | 32 | ... | EENPV1mRhUD9gSKbcWt84cqnfSGQP5LkCu5gMBfAanYH |
| 3 | Timestamp | Long | 33 | 8 | 1479287120875 | 11frnYASv |
| 4 | Amount | Long | 41 | 8 | 1 | 11111112 |
| 5 | Fee | Long | 49 | 8 | 1 | 11111112 |
| 6 | Recipient's address | Bytes | 57 | 26 | ... | 3NBVqYXrapgJP9atQccdBPAgJPwHDKkh6A8 |
| 7 | Attachment's length \(N\) | Short | 83 | 2 | 4 | 15 |
| 8 | Attachment's bytes | Bytes | 85 | N | \[1,2,3,4\] | 2VfUX |

_**Total data bytes for sign:**_

`Ht7FtLJBrnukwWtywum4o1PbQSNyDWMgb4nXR5ZkV78krj9qVt17jz74XYSrKSTQe6wXuPdt3aCvmnF5hfjhnd1gyij36hN1zSDaiDg3TFi7c7RbXTHDDUbRgGajXci8PJB3iJM1tZvh8AL5wD4o4DCo1VJoKk2PUWX3cUydB7brxWGUxC6mPxKMdXefXwHeB4khwugbvcsPgk8F6YB`

_**Signature of transaction data bytes**_ **\(one of an infinite number of valid signatures\):**

`2mQvQFLQYJBe9ezj7YnAQFq7k9MxZstkrbcSKpLzv7vTxUfnbvWMUyyhJAc1u3vhkLqzQphKDecHcutUrhrHt22D`

_**Total transaction bytes with signature:**_

`6zY3LYmrh981Qbzj7SRLQ2FP9EmXFpUTX9cA7bD5b7VSGmtoWxfpCrP4y5NPGou7XDYHx5oASPsUzB92aj3623SUpvc1xaaPjfLn6dCPVEa6SPjTbwvmDwMT8UVoAfdMwb7t4okLcURcZCFugf2Wc9tBGbVu7mgznLGLxooYiJmRQSeAACN8jYZVnUuXv4V7jrDJVXTFNCz1mYevnpA5RXAoehPRXKiBPJLnvVmV2Wae2TCNvweHGgknioZU6ZaixSCxM1YzY24Prv9qThszohojaWq4cRuRHwMAA5VUBvUs`

## Proofs

In order to support Smart Accounts, transactions have the signature field replaced with an array of so called "proofs". Proofs are an alternative way to authorize the transaction that is more flexible than signatures and enables smart contracts such as multisig and atomic swap. Each proof is a Base58 encoded byte string and can be a signature, a secret, or anything else – the semantics of a proof is dictated by the smart contract that interprets it. There can be up to 8 proofs at most 64 bytes each.

By default, only one proof is used, which must be the transaction signature by the sender. It should be the very first element in the proofs array, while all the other elements are ignored. The JSON looks like

`"proofs": [ "21jgWvYq6XZuke2bLE8bQEbdXJEk..." ]`

## Calculating Transaction Id

Transaction Id is not stored in the transaction bytes and for most of transactions \(except Payment\) it can be easily calculated from the special bytes for signing using`blake2b256(bytes_for_signing)`. For Payment transaction Id is just the signature of this transaction.

