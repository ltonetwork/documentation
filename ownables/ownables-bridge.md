# Ownables Bridge

An OWNABLE can be presented as unlockable content for an NFT. This allows OWNABLES to be traded on platforms like [OpenSea](https://opensea.io/) and [Rarible](https://rarible.com/).

At any time, either the NFT or the OWNABLE is locked. When the NFT is unlocked and tradable, the OWNABLE needs to be in the locked state and uploaded to an OWNABLES bridge. The bridge allows the new NFT owner to obtain the OWNABLE.

## Universal Wallet

As the owner of the NFT, you can obtain your OWNABLE using LTO's [Universal Wallet](../wallets/universal-wallet.md).

Universal Wallet will show your Ethereum (EIP-155) address in addition to your LTO address. If you create an account with the seed phrase from your Ethereum wallet, like Metamask, you'll notice that the Ethereum address will match. Alternatively, if you create a new account, you'll first need to transfer the NFT to the displayed Ethereum address.

{% hint style="warning" %}
When bridging, the cipher (cryptographic protocol) of the LTO wallet must match that of the blockchain the NFT is on. If the NFT is on an EVM network, like Ethereum, you're only able to obtain the Ownable with an [secp256k1](../protocol/accounts/#secp256k1) account.
{% endhint %}

### Obtaining the OWNABLE

Before the NFT can be downloaded from the bridge, the NFT will be locked. Universal Wallet will send a transaction with a `lock` call to the NFT smart contract. You'll need to be able to pay the gas fee of the transaction from the wallet address.

Once the NFT is locked, Universal Wallet will download the OWNABLE package and event chain from the bridge. The download request is signed by the wallet, allowing the bridge to verify that you are the owner of the NFT. The NFT contract emits a `Lock` event.

The downloaded OWBNABLE is instantiated within your wallet. The wallet will unlock the OWNABLE, using the `Lock` event of the NFT contract as proof.

### Unlocking the NFT

While you'll remain the owner of the NFT, you'll be unable to transfer it until it's unlocked. To unlock the NFT, you'll need to send the OWNABLE to the bridge.

Universal Wallet will start by locking the OWNABLE. Only locked OWNABLES are accepted by the bridge. This ensures that the OWNABLE isn't modified while bridged.

Both the OWNABLE package and event chain are uploaded to the bridge. The bridge will verify the event chain and make sure that the OWNABLE you are indeed the current owner. It will send proof in the form of a cryptographic signature. This proof is needed to unlock the NFT

The wallet will send a transaction with an `unlock` call to the NFT smart contract, passing the cryptographic proof it got from the bridge.

Once unlocked, you'll able to transfer and trade it on an NFT platform.

## Choosing a Bridge

Anyone is able to host an OWNABLE bridge. It's up to the issuer of the NFT contract to decide which bridge or bridges can be used. Only cryptographic proof of a bridge that's authorized by the smart contract, can be used to unlock the NFT.

By default, Universal Wallet will continue to use the bridge it obtained the OWNABLE from. If multiple bridges are authorized you'll be able to switch bridges in the app.

### Backup

A bridge _may_ offer to backup up the OWNABLE package and event chain. This is an extra safeguard to ensure the OWNABLE is never lost. However, it should not be used to replace the cloud backup made by Universal Wallet.
