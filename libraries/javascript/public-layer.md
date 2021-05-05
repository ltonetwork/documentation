# Public layer

## Transactions

Create a transaction and sign it from a minimal set of required params. If you want to create Transfer transaction the minimum you need to provide is **amount** and **recipient** as defined in Transfer params:

```javascript
const { transfer } = require('@lto-network/lto-transactions')
const seed = 'some example seed phrase'
const signedTranserTx = transfer({ 
  amount: 1,
  recipient: '3JmEPiCpfL4p5WswT21ZpWKND5apPs2hTMB'
}, seed)
```

Output will be a signed transfer transaction:

```text
{ 
  id: '3sgkGCxZmPpKDz8BNztWNoVEiXXgWgeZdYpJNh1CqtKp',
  type: 4,
  version: 2,
  senderPublicKey: '98Pw96PizgJC7MHT8RUDJGS7YGr68YDqmSA2X83XJeDX',
  recipient: '3JmEPiCpfL4p5WswT21ZpWKND5apPs2hTMB',
  amount: 1,
  attachment: '',
  fee: 100000,
  timestamp: 1536917842558,
  proofs: [ 
    '4r7Amhzmpj2yh7uCiTkTjosVwKfHUTucoyitRXafzTBtQrsdqVGcJvJdneHakNq2LcsBWCxfDowkke7RbAMMZoaQ' 
  ]
}
```

You can also create transaction, but not sign it:

```javascript
const unsignedTransferTx = transfer({ 
  amount: 1,
  recipient: '3JmEPiCpfL4p5WswT21ZpWKND5apPs2hTMB',
  //senderPublicKey is required if you omit seed
  senderPublicKey: '6nR7CXVV7Zmt9ew11BsNzSvVmuyM5PF6VPbWHW9BHgPq' 
})
```

Now you are able to POST it to LTO API or store for future purpose or you can add another signature from another party:

```javascript
const otherPartySeed = 'other party seed phrase'
const transferSignedWithTwoParties = transfer(signedTranserTx, otherPartySeed)
```

So now there are two proofs:

```text
{ 
  id: '3sgkGCxZmPpKDz8BNztWNoVEiXXgWgeZdYpJNh1CqtKp',
  type: 4,
  version: 2,
  senderPublicKey: '98Pw96PizgJC7MHT8RUDJGS7YGr68YDqmSA2X83XJeDX',
  recipient: '3JmEPiCpfL4p5WswT21ZpWKND5apPs2hTMB',
  amount: 1,
  attachment: '',
  fee: 100000,
  timestamp: 1536917842558,
  proofs: [ 
    '4r7Amhzmpj2yh7uCiTkTjosVwKfHUTucoyitRXafzTBtQrsdqVGcJvJdneHakNq2LcsBWCxfDowkke7RbAMMZoaQ',
    '4m2GCeWc3jFg7qE7D67rzD26KTe2YMaSSz99GcxGCezBAuh6LSWBCEnDbPDfRMKDoCZDdTLgjovdF9LhDzan4Qah' 
  ]
}
```

## Broadcast

To send transaction you can use either node [REST API](https://nodes.lto.network/api-docs/index.html#!/transactions/broadcast) or broadcast helper function:

```javascript
const {broadcast} = require('@lto-network/lto-transactions');
const nodeUrl = 'https://nodes.lto.network';

broadcast(signedTx, nodeUrl).then(resp => console.log(resp))
```

You can send tx to any lto node you like:. E.g.:

* [https://testnet.lto.network](https://testnet.lto.network/) - lto TESTNET nodes hosted by LTO Network
* [https://nodes.lto.network](https://nodes.lto.network/) - lto MAINNET nodes hosted by LTO Network

