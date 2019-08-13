# Transaction Fees

## Transaction Fees

Transaction fees act as a reward for the miner. Because these are the only rewards, this prevents inflation of the network since no new tokens are introduced. There are currently 5 types of transactions, each with a fixed fee. You can check the transaction examples [here](../lto_node_rest_api/example_transactions.md).

| Transcation | Fee \(LTO\) |
| :--- | :--- |
| Transfer | 1 |
| Lease | 1 |
| Cancel Lease | 1 |
| Mass Transfer | 1 + 0.1 \* N |
| Data | 1 |
| Anchor | 0.25 |

**NOTE**: Currently the fees are static. In the future they might become configurable by node owners. This way the transaction price becomes independent from the token value.

