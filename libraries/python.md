---
description: Python client library for interacting with LTO Network
---

# Python

{% hint style="success" %}
Visit the [project on GitHub](https://github.com/ltonetwork/lto-api.python).
{% endhint %}

### Initiate library

```python
pl = PyCLTO.PyCLTO() pl.NODE = "https://testnet.ltonode.turtlenetwork.eu" pl.setChain("testnet")
```

### Initiate addresses

```python
addr1 = pl.Address(seed="seedhere")
addr2 = pl.Address(seed="seed2here")
addr3 = pl.Address(seed="scripted account seed here")
```

### Mass transaction

```python
transfers = [ { 'recipient': addr1.address, 'amount': 1 }, { 'recipient': addr2.address, 'amount': 1 }, ] addr1.massTransferLTO(transfers)
```

### Sponsor transaction

```python
addr1.sponsor(addr2)
```

### Cancel sponsor transaction

```python
addr1.cancelSponsor(addr2)
```

### Lease transaction

```python
tx = addr1.lease(addr2,1) print(tx)
```

### Lease cancel transaction

```python
addr1.leaseCancel(tx['id'])
```

### Set script transaction

```python
script = 'match tx { \n' +
' case _ => true\n' +
'}' addr3.setScript(script)
```

