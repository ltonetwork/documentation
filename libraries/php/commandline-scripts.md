# Commandline scripts

## Generate account from seed

Get the seed from `stdin` to generate account info.

```text
$ vendor/bin/lto-seed <<< "manage manual recall harvest series desert melt police rose hollow moral pledge kitten position add"

address: 3JmCa4jLVv7Yn2XkCnBUGsa7WNFVEMxAfWe
sign:
  secretkey: 4zsR9xoFpxfnNwLcY4hdRUarwf5xWtLj6FpKGDFBgscPxecPj2qgRNx4kJsFCpe9YDxBRNoeBWTh2SDAdwTySomS
  publickey: GjSacB6a5DFNEHjDSmn724QsrRStKYzkahPH67wyrhAY
encrypt:
  secretkey: 4q7HKMbwbLcG58iFV3pz4vkRnPTwbrY9Q5JrwnwLEZCC
  publickey: 6fDod1xcVj4Zezwyy3tdPGHkuDyMq8bDHQouyp5BjXsX
```

By default a mainnet \(`L`\) address is generated. Pass `T` as argument to create a testnet address.

```text
$ vendor/bin/lto-seed T <<< "manage manual recall harvest series desert melt police rose hollow moral pledge kitten position add"
```

## Account info

Show account info from the secret key. The secret key should be base58 encoded.

```text
$ vendor/bin/lto-account 4zsR9xoFpxfnNwLcY4hdRUarwf5xWtLj6FpKGDFBgscPxecPj2qgRNx4kJsFCpe9YDxBRNoeBWTh2SDAdwTySomS

address: 3JmCa4jLVv7Yn2XkCnBUGsa7WNFVEMxAfWe
sign:
  secretkey: 4zsR9xoFpxfnNwLcY4hdRUarwf5xWtLj6FpKGDFBgscPxecPj2qgRNx4kJsFCpe9YDxBRNoeBWTh2SDAdwTySomS
  publickey: GjSacB6a5DFNEHjDSmn724QsrRStKYzkahPH67wyrhAY
encrypt:
  secretkey: 4q7HKMbwbLcG58iFV3pz4vkRnPTwbrY9Q5JrwnwLEZCC
  publickey: 6fDod1xcVj4Zezwyy3tdPGHkuDyMq8bDHQouyp5BjXsX
```

By default a mainnet \(`L`\) address is generated. Pass `T` as third argument to create a testnet address.

```text
$ vendor/bin/lto-seed 4zsR9xoFpxfnNwLcY4hdRUarwf5xWtLj6FpKGDFBgscPxecPj2qgRNx4kJsFCpe9YDxBRNoeBWTh2SDAdwTySomS T
```

## Sign message

Sign a message \(from `stdin`\). The secret key should be base58 encoded. Outputs the signature and hash \(`base58` encoded\).

```text
$ cat message.json | vendor/bin/lto-sign 4zsR9xoFpxfnNwLcY4hdRUarwf5xWtLj6FpKGDFBgscPxecPj2qgRNx4kJsFCpe9YDxBRNoeBWTh2SDAdwTySomS

signature: 5i9gBaHwg9UFPuwU63LBdBR29yZdRDstWM9z7oo8GzevWhBdAAWwCSRUQbPLaCT3nFgjbQuuWxVQckzCd3CoFig4
hash:      42TEXg1vFAbcJ65y7qdYG9iCPvYfy3NDdVLd75akX2P5
```

