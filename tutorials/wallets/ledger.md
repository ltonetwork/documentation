---
description: How to use a Ledger device with LTO Network
---

# Ledger

![](https://camo.githubusercontent.com/b74e6b5ce2eab3d4c8ca16c922f3d1c6ecc05829b24f709b0cd0a338c361ad8d/68747470733a2f2f692e696d6775722e636f6d2f6834463576456f2e6a7067)

A Ledger device is a hardware wallet. Hardware wallets are considered very secure for the storage of a user’s private keys in the blockchain. Your digital assets are safe even when used on an infected or untrusted computer.

Please note that this tutorial applies only to **Native LTO coins**. Do not try to send or receive tokens directly from Ethereum or Binance chains as your funds may be lost forever!

### Before you begin

* You have initialized your Ledger device
* The latest firmware has been installed
* Ledger Live is ready to use

### Install the LTO Network app on your Ledger Nano

1. Open the Manager in Ledger Live.
2. Connect and unlock your Ledger Nano.
3. If asked, allow the manager on your device by pressing the right button.
4. Find LTO Network in the app catalog.
5. Click the Install button of the app.
6. An installation window appears.
7. Your device will display Processing…
8. The app installation is confirmed.

There are currently two ways to interact with the LTO Network blockchain using a Ledger hardware wallet: an installable **CLI Wallet** and a **Web Wallet**.

## Stakely.io web-wallet

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

{% hint style="info" %}
The Web Wallet was built to be a full **client-side** Ledger Hardware Wallet interface. There is no data interchange between the Ledger Device or Web Wallet to any external server in the signature process.
{% endhint %}

## Commandline

### Use a Ledger device with the CLI Wallet

It was the first interface for this Ledger hardware wallet app. It consists of an **executable** that autodetects your device showing a list of possible tasks.

These executables can be downloaded here, make sure you download the correct binary and most recent for your platform \(Windows, Linux, macOS\): [https://github.com/iicc1/ledger-app-lto/releases](https://github.com/iicc1/ledger-app-lto/releases)

Once you have the executable downloaded, **double-click** it, and a command prompt will appear. Don't be afraid of the console, you won't need to write any command! This is a fairly easy way to use the wallet, you just need to enter the numbers and transaction fields.

![](https://camo.githubusercontent.com/1732511b071c1cbf2d3e751579ca66ae7abd6eac5bb38beb155939e66f3d7f55/68747470733a2f2f74656c656772612e70682f66696c652f6261633431653064383430393762313137326533622e706e67)

There are six possible options to choose which are self-explanatory.

Let's see an example of how to **send LTO** with this CLI Wallet:

### Send LTO through a Ledger device using the CLI Wallet

You should have your Ledger device with the LTO Network app selected and the Cli wallet opened:

![](https://camo.githubusercontent.com/c069ff1e27ef795fad47d3e381fba39bf59e10c5cc39fbe6e5919da789bccdb8/68747470733a2f2f74656c656772612e70682f66696c652f3164373530626335386361316234353164623437342e706e67)

We first need to get our **public key** and **address** from the Ledger device, so put a **1** in the console and press enter.

A message asking for a BIP-32 path will show up, just click enter. We are going to use the default path in this tutorial.

Then, our public key and address will appear on the screen:

![](https://camo.githubusercontent.com/9d39057f21ee57700b8b990771ff2ad95d7553a250063bb5fd387e6ebbe56d55/68747470733a2f2f74656c656772612e70682f66696c652f3836323039363533633435363939626135643565642e706e67)

Now that we have this data, we can proceed to send a transaction. In this tutorial, I will do a transfer, so input a **2** and press enter.

A pop-up message asking for a path will emerge: just press enter.

Then it will ask you for a public key. You need to paste the public key we have fetched in the previous step; after that, press enter.

_Note that in Windows, to copy something on the console, you need to select the text you want to copy and press enter. Then, to paste, just do a right-click on the console._

The CLI Wallet will ask you for a recipient. Paste the address you want to send LTO and press enter.

In the next step, you need to introduce how many LTOs you want to send. Press enter and it will try to **sign this data** with your Ledger device.

The Ledger device will display on its screen the transaction data: recipient, fee, amount, transaction ID, and your address.

**Always double-check that the data shown by your Ledger device coincide with the data on the console.**

Once you have validated the data, press the **accept transaction** button. You will see on the CLI Wallet that the transaction was signed:

![](https://camo.githubusercontent.com/74e1b5740f3e33c11f8219fcb5ecabdd1003822d0ffa7d5952c89521cc2efc1a/68747470733a2f2f74656c656772612e70682f66696c652f3465303663343735313861363663343366386135372e706e67)

Finally, we need to **broadcast** this transaction. To do that, copy everything between brackets \(included\) and paste it here: [https://nodes.lto.network/api-docs/index.html\#!/transactions/broadcast](https://nodes.lto.network/api-docs/index.html#!/transactions/broadcast)

![](https://camo.githubusercontent.com/cac97504fcef14e241e1ddc4cd94feab8e13c4b0ac09ad818260445fdefa6f50/68747470733a2f2f74656c656772612e70682f66696c652f3231353666666565623163626365326235326535312e706e67)

Press **Try it out!** and there should not be errors in the Response Body

If you want to track your transaction, paste the **id** value from the **Response Body** on the explorer: [https://explorer.lto.network](https://explorer.lto.network/)

## Further Security Notes and Further Developments

Although a Ledger hardware wallet is a very secure way of storing your LTO, always initialize your account by broadcasting a transaction to enhance the security of your account effectively reducing the chances of being hit by an “Address collision”.

Some features: data transactions, mass transfers, and other new transactions -even if supported by the Ledger integration- are not supported by the web/CLI wallet. They might be implemented in future releases of the web wallet.

Ledger devices use the BIP-32 deterministic wallet generation, so even if all Cli and Web LTO wallet interfaces disappear, you will be able to recover any wallet by using your Ledger device seed.

### Final Notes

Ledger hardware wallet support for LTO Network was an unofficial development by the LTO Network community. You can contribute to the projects here:

* Ledger app + Cli Wallet: [https://github.com/iicc1/ledger-app-lto](https://github.com/iicc1/ledger-app-lto)
* Web Wallet \(recommended\): [https://github.com/iicc1/lto-network-ledger-wallet-ui](https://github.com/iicc1/lto-network-ledger-wallet-ui)
* Web Wallet \(deprecated\): [https://github.com/iicc1/lto-ledger-vue](https://github.com/iicc1/lto-ledger-vue)

### Support

If you encounter any issue with your Ledger device, Web wallet, or CLI wallet, please join the LTO Network Tech Chat, or open an issue on Github.

* LTO Network tech Telegram chat: [https://t.me/ltotech](https://t.me/ltotech)
* Ledger application GitHub issues: [https://github.com/iicc1/ledger-app-lto/issues](https://github.com/iicc1/ledger-app-lto/issues)
* Ledger web interface GitHub issues: [https://github.com/iicc1/lto-network-ledger-wallet-ui/issues](https://github.com/iicc1/lto-network-ledger-wallet-ui/issues)

