# Transaction Fees

## Transaction Fees

Transaction fees act as a reward for the miner. Because these are the only rewards, this prevents inflation of the network since no new tokens are introduced.

| Transaction | Fee \(LTO\) | Minimum \(LTO\) |
| :--- | :--- | :--- |
| Transfer | 1 | 0.1 |
| Lease | 1 | 0.1 |
| Cancel Lease | 1 | 0.1 |
| Mass Transfer | 1 + 0.1 \* N | 0.1 + 0.1 \* N |
| Anchor | 0.35 | 0.1 |
| Invoke Association | 1 | 0.1 |
| Revoke Association | 1 | 0.1 |
| Sponsor | 5 | 5 |
| Cancel Sponsor | 1 | 0.1 |
| Script | 5 | 5 |

The absolute minimum fees are enforced by the consensus model. The current fees are configured by the nodes as the minimum acceptable fee.

### Fee distribution

For every transaction 0.1 LTO isn't awarded and thus effectively taken out of circulation \(aka burned\). To remaining fee is split up among the current leader, and the next elected node. The fee is distributed 40% to the leader and 60% to the next one.

For more information see the [NG documentation on Waves.](https://docs.waves.tech/en/blockchain/waves-protocol/waves-ng-protocol)

