# Wallet Web app

In the general wallet, multiple accounts can be created and imported.

The wallet web app operates fully on the client-side. Accounts are only stored locally, encrypted, and secured with the password that has been set during creation or import.  
Network information \(like balance, previous transactions\) is fetched from a node's API, using the public wallet address.  
Transactions are signed locally, and only the signed transaction is broadcasted to a node to be propagated in the LTO Network. No sensitive data is ever transmitted to an external server.

