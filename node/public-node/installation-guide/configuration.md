# Configuration

**Configuration**

Before you run the node you will need to configure a few environment variables per service:

**Public Node Container**

1. `LTO_WALLET_SEED`: The seed of your wallet. Your account will need at least 1000 LTO to be able to start mining.
2. `LTO_WALLET_SEED_BASE58`: The seed of your wallet but then base58 encoded. This will overwrite the `LTO_WALLET_SEED`
3. `LTO_WALLET_PASSWORD`: This password is used to encrypt your seed on disk.
4. `LTO_API_KEY`: Choose an api-key this need to be same in the `Anchor service` so that is able to communicate with the public node.
5. `LTO_NETWORK`: Choose the network you want to connect your node to. The options are: `MAINNET` and `TESTNET` (default is`MAINNET`).

For other options check out: [Public Node on Github](https://github.com/ltonetwork/lto-public-node)

**Anchor service**

1. `LTO_API_KEY`: The same ApiKey as was set in the `Public Node`.
