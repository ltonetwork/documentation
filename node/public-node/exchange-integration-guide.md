# Exchange integration guide

As an exchange, you may choose to rely only on the [REST API ](rest-api/)of the LTO public node. This means that there's no dependency on a client library or use of custom logic.

Please follow the general guide for [installing a public node](installation-guide/). The recommended method is using docker. Alternatively, there's a .deb package available for Debian / Ubuntu.

{% hint style="warning" %}
When setting up the node, you're required to configure a wallet seed phrase and API key. **Both should be kept extremely safe.** It's recommended to ensure that the REST API (on port 6869) is only available locally and not exposed to the internet.
{% endhint %}

_For the examples, we assume the node is running on localhost._

## Withdrawal

The node will create a default wallet address based on the wallet seed. This address should serve as the hot wallet for the exchange. When a user does a withdrawal of LTO, tokens are sent from this address.

Transferring tokens from the hot wallet to a recipient is done by sending a `POST` request to `/transactions/submit`. The transaction will be signed by the node. Signing a transaction requires authentication with the API key using the `Authorization` header.

```
$ echo '{"sender":"3JmCa4jLVv7Yn2XkCnBUGsa7WNFVEMxAfWe", "recipient":"3JzLRZFtqRoormvAKTRXJtQAUN6WkfoKdf2", "amount": 100000000000, "fee":100000000}' \
curl -X POST http://localhost:6869/transactions/submit/transfer --data @- -H "Authorization: bearer secret"
```

The amount in LTO should be done `* 10^8`. The transfer fee is 1 LTO. The sender is the address of the hot wallet.

{% hint style="warning" %}
The `/transactions/submit` endpoint is available since v1.5.2. For older versions, first sign and then broadcasted the signed transaction.
{% endhint %}

## Deposit

### Create intermediate wallet

To enable users to deposit tokens they require an intermediate wallet on the LTO Network with a private key that's controlled by the exchange.

The public node is able to generate over 4 billion (2^32) addresses using a single seed phrase. This should be more than sufficient for any use case.

To create a new address send a `POST` request to `/wallet/addresses`. You're required to authenticate with the API key via the `Authorization` header.

```
$ curl -X POST http://localhost:6869/wallet/addresses -H "Authorization: bearer secret"
{ "address" : "3QV4xdTK9aqMYrK5Tuojq2XXw8nX1NNkhjE" }
```

The node will only return a wallet address and not a private key. The node can sign transactions for that address. It's not possible to extract the private key via the REST API.

### Watch transactions

When a user sends LTO to the intermediate wallet, it should be picked up by the software of the exchange, so the amount can be added to the user's balance.

The public node will node do a callback for transactions. Instead, the software of the exchange should poll the blocks.

```
$ curl http://localhost:6869/blocks/1834779
{
  version: 3,
  timestamp: 1646393744714,
  reference: "3kmSG1M95L5koCCoRZWNC9ya9GWXJNwxN6J2qwcSUZGEGoc8MpbZxLDFiwZdfPJZZRe8MHpsZR81gECwh3EG86am",
  nxt-consensus: { },
  features: [ ],
  generator: "3MpL7qLzS8UZH4cNcZsvJUWouiXJ6ZQdRCV",
  signature: "5jMJTpKvkJTHmLJjMKwHxJYpSNna9hcGL3DBTdxA3v9gwcsvLuB5yP5uq7C2yF1FpLtLfUAtJkMqm2yaChnuWnrq",
  blocksize: 225,
  transactionCount: 0,
  fee: 0,
  transactions: [ ],
  height: 1834779
}
```

Filter the transactions based on type and recipient address and process them.



{% hint style="warning" %}
* Make sure this process is [ACID](https://en.wikipedia.org/wiki/ACID) and/or [idempotent](https://en.wikipedia.org/wiki/Idempotence), so balance updates are never lost or done twice.
* Keep track of the last processed block and do not rely on the `/blocks/last` endpoint.
* It's recommended to delay at between 10 blocks and 100 blocks, because of potential chain reorganization (hard finalization after 100 blocks).
{% endhint %}

Implementing this logic is outside the scope of the LTO Public Node. See [lto-chain-listener](https://github.com/ltonetwork/lto-chain-listener) as an example of this implementation.

### Transfer to hot wallet

LTO from the intermediate wallet should be sent to your hot wallet once processed. This is done by sending a `POST` request to `/transactions/submit`. The transaction will be signed by the node. Signing a transaction requires authentication with the API key using the `Authorization` header.

```
$ echo '{"sender":"3N5eQyFLimmv2Q1noYDnnrn5FNbNT9m3FZV", "recipient":"3JmCa4jLVv7Yn2XkCnBUGsa7WNFVEMxAfWe", "amount": 99900000000, "fee":100000000}' \
curl -X POST http://localhost:6869/transactions/submit/transfer --data @- -H "Authorization: bearer secret"
```

The amount in LTO should be done `* 10^8`. The sender is the address of the intermediate wallet. The recipient is the address of the hot wallet. Note that the transaction fee is 1 LTO. This must be subtracted from the amount in order to transfer all tokens from the wallet.

## Cold wallet

It's highly recommended to create a cold wallet that isn't related to a node. This can be done using the [LTO CLI](../../wallets/lto-cli.md).

Most exchanges choose to make transferring LTO between the hot and cold wallet a manual process.

{% hint style="danger" %}
Please take decentralization of the network and our community in mind and disable staking for tokens held on the exchange. This can be done by setting the environment variable `LTO_ENABLE_MINING=no` when starting the docker container.
{% endhint %}
