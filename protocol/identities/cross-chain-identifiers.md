# Cross-chain identifiers

By default, an LTO identity node will only resolve DIDs based on LTO network addresses. It's possible to configure the service to index DIDs based on addresses of other blockchains, like Ethereum or Bitcoin.

## LTO Network cross-chain DID method

DIDs with the method "**ltox**" can be resolved by the LTO Network identity node that supports cross-chain identifiers.

The method-specific string is comprised of a CAIP-2 blockchain ID and an address on that specific chain.

> tlox:eip155:1:0xab16a96d359ec26a11e2c2b3d8f8b8942d5bfcdb

```text
ltox-did = "did:ltox:" chain-id ":" address
chain-id = namespace + ":" + reference
chain-namespace = {3,16}\*( ALPHA / DIGIT )
chain-reference = {1,47}\*( ALPHA / DIGIT )
address = {1,63}\*( ALPHA / DIGIT )
```

The method-specific string is case-sensitive.

