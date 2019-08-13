# Troubleshooting FAQ

#### 1. My public node seems to be up and running but when checking the logs from time to time I get a message of this type:

```bash
Error mining Block: BlockAppendError(Block is not a child of the last block,Block(4Bk5FxnuKMPqZeh4Rfyn1pE6UyNYfznQd7MtUhKtNug15WoxvhkjtCeo4AVMAW2AEXFw2DMfxd1MZ3G71SiJdnUC -> 245bVsJ..., txs=0, features=Set()))
```

When this happens, your node most likely went out-of-sync, in order to fix it you should follow the next steps:

```bash
# stop your node (if spinned up by docker compose)
$ docker-compose down

# sync your node's clock with NTP server
$ sudo service ntp stop
$ sudo ntpdate pool.ntp.org
$ sudo service ntp start

# spin up your node again
$ docker-compose up -d
```

