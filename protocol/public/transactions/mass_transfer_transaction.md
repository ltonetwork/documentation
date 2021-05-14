# Mass Transfer

To address this issue, we're going to introduce a new transaction type, Mass Transfer. A Mass Transfer combines several ordinary Transfer transactions that share a single sender. Under the hood, it has a list of recipients, and an amount to be transferred to each recipient.

```cpp
{
  "type" : 11,
  "version" : 3,
  "id" : "BG7MQF8KffVU6MMbJW5xPowVQsohwJhfEJ4wSF8cWdC2",
  "sender" : "3HhQxe5kLwuTfE3psYcorrhogY4fCwz2BSh",
  "senderKeyType": 1,
  "senderPublicKey" : "7eAkEXtFGRPQ9pxjhtcQtbH889n8xSPWuswKfW2v3iK4",
  "fee" : 200000,
  "timestamp" : 1518091313964,
  "proofs" : [ "4Ph6RpcPFfBhU2fx6JgcHLwBuYSpn..." ],
  "attachment" : "59QuUcqP6p",
  "transfers" : [
    {
      "recipient" : "3HUQa6qtLhNvBJNyPV1pDRahbrcuQkaDQv2",
      "amount" : 100000000
    }, {
      "recipient" : "3HaAdZcCXAqhvFj113Gbe3Kww4rCGMUZaEZ",
      "amount" : 200000000
    },
    ...
  ]
}
```

It's easy to see how compact this transaction is compared to several Transfer transactions. Here we have a sequence of recipients and associated amounts, while sender, fee, timestamp, and signature occur just once.

### Constraints

The maximum number of recipients in a single transaction is 100. There is no minimum recipient number. You can create a Mass Transfer Transaction with one or even zero recipients. In addition, restrictions that apply to Transfers apply here as well, such as you cannot send a negative amount and cannot send more than you have on your account.

Other than that, we've decided not to put any restrictions on transactions that are harmless, even if they may seem against common sense. For example, transfers to self are allowed, as well as zero-valued transfers. In the recipient list, a recipient can occur several times, this is not considered an error.

### Fees

The Mass Transfer fee is made up of two amounts: a fixed one plus a per-recipient one. By default the formula looks like:

```text
1 + 0.1 * N
```

where `N` is the number of recipients in the transaction. The total is rounded up to the nearest 100\_000. Fee can be configured in miner node settings using the usual syntax, just keep in mind that there are two parts to it, the base \(`BASE`\) fee and the variable \(`VAR`\) fee. Below is an excerpt from the configuration file that ships with the node:

```cpp
mass-transfer {
  BASE = 100000000
  VAR = 10000000
}
```

### Binary schema

The binary data structure of the unsigned transaction.

|  | Field Name | Type | Length |
| :--- | :---: | :---: | :--- |
| 1 | Transaction type | Byte \(constant, value=11\) | 1 |
| 2 | Version | Byte \(constant, value=3\) | 1 |
| 3 | Timestamp | Long | 8 |
| 4 | Sender's key type | KeyType \(Byte\) | 1 |
| 5 | Sender's public key | PublicKey \(Array\[Byte\]\) | 32 \| 33 |
| 6 | Fee | Long | 8 |
| 7 | Number of transfers \(T\) | Short | 2 |
| 8 | Recipient 1 | Address \(Array\[Byte\]\) | 26 |
| 9 | Amount 1 | Long | 8 |
| ... |  |  |  |
| 10 | Attachment length \(N\) | Byte | 2 |
| 11 | Attachment | Array\[Byte\] | N |

{% hint style="info" %}
* Recipient and Amount are repeated for each transfer.
* Integers \(short, int, long\) have a big endian byte order.
{% endhint %}

