# FAQ

#### 1. Is there actually a page where all the resources concerning nodes are mentioned ? Like all the different commands and their effects ?

Yes, in this section [_**NODE API**_](./).

#### 2. My Digital Ocean node is fully synced. I have managed to lease 1000+ LTO to the node. How can I know if it is already mining?

There is a method but the API key is needed: `GET /debug/minerInfo` which shows all miners who has enough generating balance to be able to generate block.

#### 3. How can you GET the LTO balance of a Wallet via API?

1. GET /addresses/balance/details/{address} 
2. GET /addresses/balance/{address}

#### 4. I am unable to connect to my nodes API endpoint after successful setup of my node when I try a get request [http://my\_server\_ip\_address:6868/addresses/balance/](http://my_server_ip_address:6868/addresses/balance/) wallet\_address it gives an error "Error: Failed sending data to the peer".

API port is 6869 by default, make sure you have enabled it in your config. You can enable and configure your node API using the optional LTO\_ENABLE\_REST\_API and LTO\_API\_KEY environment variable.

#### 5. How can I sign a transaction with a private key using the REST API?

There are 2 ways to sign transactions:

1. Use a node. But that node should know the private key of your address. In other words, it should be your node, because you should never send your private key to anybody else.
2. Use libraries for different languages \(Python, C\#, JavaScript, Java\). Libraries can sign transactions with the provided private key and send to the network already signed transactions.



