---
description: LTO wallet for the commandline terminal. Great for automating tasks.
---

# LTO CLI

## Installation

```
pip install lto-cli
```

[pip](https://pip.pypa.io/en/stable/) is the package installer for Python.

{% hint style="warning" %}
On some operating systems, like Ubuntu, the `pip` command is linked to Python 2. In that case use `pip3` instead.
{% endhint %}

## Setup

### Manage accounts

```
lto account create
echo "my seed" | lto account seed --name foobar
lto account list
lto account show 3JuijVBB7NCwCz2Ae5HhCDsqCXzeBLRTyeL
lto account set-default foobar
lto account remove 3JuijVBB7NCwCz2Ae5HhCDsqCXzeBLRTyeL
```

### Public node

```
lto node set https://nodes.lto.network
lto node show
```

## Information

### Balance

```
lto balance
lto balance 3MyGpJh6Bb8auF3HtSr2dSJjqQVxgqLynpK
```

### Node

```
lto node status
```

## Transactions

### Anchor

```
HASH1=e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
HASH2=d4735e3a265e16eee03f59718b9b5d03019c07d8b6c51f90da3a666eec13ab35

lto anchor --hash $HASH1
lto anchor --hash $HASH1 --hash $HASH2   # Multiple anchors
lto anchor --hash $HASH2:$HASH1          # Mapped anchor
```

### Associations

```
lto association issue --recipient 3MyGpJh6Bb8auF3HtSr2dSJjqQVxgqLynpK --type 1 --hash e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
lto association revoke --recipient 3MyGpJh6Bb8auF3HtSr2dSJjqQVxgqLynpK --type 1 --hash e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
lto association in
lto association out
```

### Data

```
echo '{"foo": "bar"}' | lto data set
lto data get
lto data get --key=foo
```

_Data is read as JSON from stdin._

### Transfer

```
lto transfer --recipient 3MyGpJh6Bb8auF3HtSr2dSJjqQVxgqLynpK --amount 1000000000
```

### Mass-transfer

```
echo "3N6MFpSbbzTozDcfkTUT5zZ2sNbJKFyRtRj:1000000000
3NBC7ETcdPbf4QAXSop5UCJ53yX34aGPXoz:800000000" | lto mass-transfer
```

_Recipient/amount pairs are read from stdin._

### Burn

```
lto burn --amount=1000
```

### Leasing

```
lto lease create --recipient 3MyGpJh6Bb8auF3HtSr2dSJjqQVxgqLynpK --amount 1000000000
lto lease cancel --leaseid 6XmeG7SRWiw8pD6Uad6D9AAaY354v5TV6AJMhPpHMkqy
lto lease in
lto lease out
```

### Sponsorship

```
lto sponsorship create --recipient 3MyGpJh6Bb8auF3HtSr2dSJjqQVxgqLynpK
lto sponsorship cancel --recipient 3MyGpJh6Bb8auF3HtSr2dSJjqQVxgqLynpK
lto sponsorship in
```

### Script

```scala
match tx {
  case t:  TransferTransaction => false
  case mt: MassTransferTransaction => false
  case ss: SetScriptTransaction => false
  case b: BurnTransaction => false
  case _ => sigVerify(tx.bodyBytes, tx.proofs[0], tx.senderPublicKey)
}
```

{% hint style="warning" %}
This script will disallow transfer and burn transactions for an account and disable changing the script again. **Your tokens will be locked if you use it!**
{% endhint %}

```
cat myscript.ride | lto script
```

## Common options

```
--network CHAINID
--account NAME|ADDRESS
--sponsor NAME|ADDRESS
--no-broadcast
--unsigned
```

**--network**

Use `--network T` or `-T` to use testnet instead of mainnet. You need to set up accounts specifically for testnet.

**--account**

Select one of the accounts configured during setup. The account can be referenced by name or address. The name is only known locally. If this option is omitted, the default account is used.

**--sponsor**

Choose an account to sponsor the transaction. The sponsor will co-sign the transaction and pay the transaction fee.

**--no-broadcast**

Create and sign the transaction, but don't broadcast it to the node. The JSON will be outputted.

**--unsigned**

Create the transaction, but don't sign it. This option should only be used in combination with `--no-broadcast`.
