# Anchoring

Anchoring on a blockchain is a simple and cheap way to notarize documents and timestamp data. Authorization levels and ACLs offer limited protection against manipulation by those in charge or tasked to maintain the system.

You can provide indisputable proof of existence by securing a hash on the LTO Network public blockchain: [https://anchor-demo.lto.network/demo](https://anchor-demo.lto.network/demo/)

## REST API

The LTO node comes in several flavors. Anchoring nodes expose an HTTP REST interface allowing you to easily submit anchor transactions. The transactions are signed by the node and \(thus\) paid by the account associated with the node.

To anchor send a POST request to [https://anchor-demo.lto.network/](https://anchor-demo.lto.network/api-docs/) with a JSON body.

```text
{
  "hash": "47DEQpj8HBSa+/TImW+5JCeuQeRkm5NMpJWZG3hSuFU=",
  "encoding": "base64"
}
```

The following encodings are accepted; _hex_, _base58,_ and _base64_.

The node exposes a Swagger UI which you can use to try out all the available HTTP endpoints.

![Swagger interface LTO Network Anchoring node](https://cdn-images-1.medium.com/max/1600/1*-tuVnK4w9JuAxc5HP2l9Ag.png)

### LTO mainnet

The demo service uses LTO testnet. Transactions on testnet are free, but the network isn't secure and must not be used in production.

To anchor on LTO mainnet, [install your own anchoring node](../../anchoring-node-1/installation-guide/) using docker. Submitting anchoring transactions on mainnet requires a transaction fee. 

If the node is set up correctly, you’ll need to provide the API key for each request as `X-LTO-Key` request header. This key has been configured as environment variable `LTO_API_KEY` during set up.

