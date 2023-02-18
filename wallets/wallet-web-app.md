# LTO web wallet

The official wallet web app is available at [https://wallet.lto.network](https://wallet.lto.network) and enables users to:

* Generate a **new seed phrase** (new wallet keys) or **connect a Ledger Device**
* Transfer native LTO coins to an address
* Lease native LTO coins to an address
* Anchor document hashes to the LTO Network blockchain
* Utilize the bridge (swap to other networks like Ethereum and Binance Smart Chain)

In the general wallet, multiple accounts can be created and imported.

The wallet web app operates fully on the client side. Accounts are only stored locally, encrypted, and secured with the password that has been set during creation or import. Network information (like balance, and previous transactions) is fetched from a node's API, using the public wallet address.

Transactions are signed locally, and only the signed transaction is broadcasted to a node to be propagated in the LTO Network. No sensitive data is ever transmitted to an external server.

## Create Account

In order to create an account, go to [https://wallet.lto.network/](https://wallet.lto.network/) and click on **Create Account** in the top right corner. It will notify you about creating a new account - click **Continue**.

![](<../.gitbook/assets/image (4).png>)

A name and password are asked to store and secure your account. (Note that this is a non-custodial wallet and this data resides on your PC)

![](<../.gitbook/assets/image (1).png>)

Click Continue.\
The next screen will show you your 15 word seed phrase. **Write these words down carefully!** This is needed if you ever want to import your account again.

![](<../.gitbook/assets/image (3).png>)

The screen provides two options:

* "I've written it down" - Click this if you have written down your seed phrase, you will be asked to **verify the seed** after. Confirm if you did.
* "Do it later" - It will open the wallet immediately **without** confirming/verifying the seed phrase. You can obtain the seed phrase in the "Settings" menu when the account is unlocked.

## Interface

The interface of the wallet is pretty self-explanatory and intuitive while using it.

### Main screen

1. Your wallet's public address
2. The total amount your wallet holds
3. The amount of effective LTO for the past 1000 blocks
4. The total amount spendable
5. The total amount your wallet holds + the amount leased to it
6. List of transfers (incoming/outgoing)
7. Menu

![](<../.gitbook/assets/image (2).png>)

### Leasing

1. Start a new lease (full how-to available [here](../tutorials/buying-and-staking-lto/staking-lto-tokens.md))
2. List of lease transactions for your wallet
3. Button to cancel an active lease (only visible on active leases)

![](<../.gitbook/assets/image (6).png>)

### Bridge

The Bridge allows swapping LTO to tokens to ERC20 on Ethereum or BEP2 on Binance chain. For more information, please follow the bridging guide.

{% content-ref url="../tutorials/buying-and-staking-lto/using-the-lto-bridge.md" %}
[using-the-lto-bridge.md](../tutorials/buying-and-staking-lto/using-the-lto-bridge.md)
{% endcontent-ref %}

### **Settings**

1. Your public wallet address
2. Your public key
3. Your backup phrase (seed phrase) (Do not share this with anyone!)
4. Your private key (Do not share this with anyone!)
5. Ability to set a script (Smart Account). It provides an option to disable the ability to transfer coins and would set it so it can only do other transactions like anchoring. Do not use this as a coin holder, this is mostly used by integrators.

**Note** that certain attributes are not visible in case you are connected with a Ledger device or Universal Wallet.

![](<../.gitbook/assets/image (7).png>)
