# Mass Transfer

To address this issue, we're going to introduce a new transaction type, Mass Transfer. A Mass Transfer combines several ordinary Transfer transactions that share a single sender. Under the hood it has a list of recipients, and an amount to be transferred to each recipient.

```cpp
{
  "type" : 11,
  "version" : 1,
  "id" : "BG7MQF8KffVU6MMbJW5xPowVQsohwJhfEJ4wSF8cWdC2",
  "sender" : "3HhQxe5kLwuTfE3psYcorrhogY4fCwz2BSh",
  "senderPublicKey" : "7eAkEXtFGRPQ9pxjhtcQtbH889n8xSPWuswKfW2v3iK4",
  "fee" : 200000,
  "timestamp" : 1518091313964,
  "proofs" : [ "4Ph6RpcPFfBhU2fx6JgcHLwBuYSpn..." ],   // see Proofs below
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

It's easy to see how compact this transaction is compared to several Transfer transactions. Here we have a sequence of recipients and associated amounts, while sender, fee, timestamp and signature occur just once.

### Constraints

The maximum number of recipients in a single transaction is 100. There is no minimum recipient number. You can create a Mass Transfer Transaction with one or even zero recipients. In addition, restrictions that apply to Transfers apply here as well, such as you cannot send negative amount and cannot send more than you have on your account.

Other than that, we've decided not to put any restrictions on transactions that are harmless, even if they may seem against common sense. For example, transfers to self are allowed, as well as zero valued transfers. In the recipients list, a recipient can occur several times, this is not considered an error.

### Fees

Unlike other transactions, Mass Transfer fee is made up of two amounts: a fixed one plus a per-recipient one. By default the formula looks like:

```text
1 + 0.1 * N
```

where `N` is the number of recipients in transaction. The total is rounded up to the nearest 100\_000. Fee can be configured in miner node settings using the usual syntax, just keep in mind that there are two parts to it. Below is an excerpt from the configuration file that ships with the node:

```cpp
transfer {
    LTO = 100000
}
mass-transfer {
    # Fee for MassTransfer transaction is calculated as
    # [transfer fee] + [mass transfer fee] * [number of transfers in transaction]
    LTO = 10000
}
```



