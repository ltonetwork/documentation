---
description: How to use a Ledger device with LTO Network
---

# Web wallet

### Use a Ledger device with the Web Wallet

Click here to access the Web Wallet: [https://lto.stakely.io/](https://lto.stakely.io/)

_Please be sure to double-check that you are accessing the correct website._

1. Connect and unlock your Ledger device.
2. Open the LTO Network app.
3. The Ledger device should be recognized immediately and the Web Wallet will display your addresses and balances.

If the Web Wallet does not recognize your Ledger device, be sure that no application is already using the device, like other browsers, wallets, or virtual machines. Try to close running programs or reboot your pc if you encounter problems.

The Web Wallet offers some useful functionalities like a **network switch**, to carry out mainnet or testnet transactions; a **balance viewer**, where you will be able to see your total and available tokens; an **address selector** and the fields needed to create transactions.

This is how it should look like when a Ledger device with the LTO Network app is installed and loaded:

![](https://camo.githubusercontent.com/828b3c173812a8d6a12b5e6fa5f3460210e58ebd2daa6d560493a16c0fc426ab/68747470733a2f2f692e696d6775722e636f6d2f345a7573305a652e706e67)

Feel free to experiment with changing your address ID or switch the network, everything will be updated instantly.

Regarding the address selector: inside a Ledger device, there are thousands of possible LTO addresses that can be used. With this web wallet, you can use up to eleven mainnet and testnet wallets, more than enough for normal use.

Here is an example of how to do a Start Lease transaction using the web wallet:

### Perform a Start Lease transaction through a Ledger device using the Web Wallet

Fill in all form fields: transaction type, amount, recipient, and fee. Once you have finished, click on **Sign transaction**.

The minimal fee for a transaction will be filled in automatically.

![](https://camo.githubusercontent.com/6d8806ac5b4c032b7c6c299fc69b8ab6367dc4832f056925c2f2596e58dc10a1/68747470733a2f2f692e696d6775722e636f6d2f5141693776594c2e706e67)

The Web Wallet will send this data to your Ledger device.

The hardware wallet will display on the screen the transaction data: transaction type, fee, amount, recipient, and transaction ID.

**Always double-check that the data shown on the Ledger device coincide with the data on Web Wallet.**

Once you have validated the data, press the **accept transaction** button. You will see on the Web Wallet that the transaction was signed and this notification will pop up:

![](https://camo.githubusercontent.com/38fc9c848356a58a7337f81cc31266357312dd7e119b67efb32a5be29b90f53c/68747470733a2f2f692e696d6775722e636f6d2f34664a3158626c2e706e67)

It means that we have the transaction ready to be broadcasted over the LTO Network blockchain. Press the **Broadcast transaction** button and it will be included in a block in a few seconds.

![](https://camo.githubusercontent.com/259eb93cc9c6f1b7798e186c28c3f62d4ac7957c88284f376e0badca1f8b589d/68747470733a2f2f692e696d6775722e636f6d2f693263674677742e706e67)

Finally, a notification will appear with a link to track this transaction with the official LTO Network **blockchain explorer**.

In case there is some error with the transaction data -as unavailable funds- an error message will be shown instead of this last notification.

{% hint style="success" %}
The Web Wallet was built to be a full **client-side** Ledger Hardware Wallet interface. There is no data interchange between the Ledger Device or Web Wallet to any external server in the signature process.
{% endhint %}

