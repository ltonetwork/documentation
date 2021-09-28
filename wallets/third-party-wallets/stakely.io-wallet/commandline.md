---
description: How to use a Ledger device with LTO Network on the commandline
---

# Commandline

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

