---
description: How to use a Ledger device with LTO Network
---

# Stakely.io wallet

## Web wallet

Click here to access the Web Wallet: [https://lto.stakely.io/](https://lto.stakely.io/)

![](https://camo.githubusercontent.com/828b3c173812a8d6a12b5e6fa5f3460210e58ebd2daa6d560493a16c0fc426ab/68747470733a2f2f692e696d6775722e636f6d2f345a7573305a652e706e67)

{% page-ref page="ledger.md" %}

## CLI wallet

![](https://camo.githubusercontent.com/1732511b071c1cbf2d3e751579ca66ae7abd6eac5bb38beb155939e66f3d7f55/68747470733a2f2f74656c656772612e70682f66696c652f6261633431653064383430393762313137326533622e706e67)

{% page-ref page="commandline.md" %}

## Further Security Notes and Further Developments

Although a Ledger hardware wallet is a very secure way of storing your LTO, always initialize your account by broadcasting a transaction to enhance the security of your account effectively reducing the chances of being hit by an “Address collision”.

Some features: data transactions, mass transfers, and other new transactions -even if supported by the Ledger integration- are not supported by the web/CLI wallet. They might be implemented in future releases of the web wallet.

Ledger devices use the BIP-32 deterministic wallet generation, so even if all Cli and Web LTO wallet interfaces disappear, you will be able to recover any wallet by using your Ledger device seed.

### Final Notes

Ledger hardware wallet support for LTO Network was an unofficial development by the LTO Network community. You can contribute to the projects here:

* Ledger app + Cli Wallet: [https://github.com/iicc1/ledger-app-lto](https://github.com/iicc1/ledger-app-lto)
* Web Wallet \(recommended\): [https://github.com/stakely/lto-network-ledger-wallet-ui](https://github.com/stakely/lto-network-ledger-wallet-ui)
* Web Wallet \(deprecated\): [https://github.com/iicc1/lto-ledger-vue](https://github.com/iicc1/lto-ledger-vue)

### Support

If you encounter any issue with your Ledger device, Web wallet, or CLI wallet, please join the LTO Network Tech Chat, or open an issue on Github.

* LTO Network tech Telegram chat: [https://t.me/ltotech](https://t.me/ltotech)
* Ledger application GitHub issues: [https://github.com/iicc1/ledger-app-lto/issues](https://github.com/iicc1/ledger-app-lto/issues)
* Ledger web interface GitHub issues: [https://github.com/stakely/lto-network-ledger-wallet-ui/issues](https://github.com/stakely/lto-network-ledger-wallet-ui/issues)

