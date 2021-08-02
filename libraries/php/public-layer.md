# Public layer

The PHP client library can be used to create, sign and broadcast transactions to an LTO public node.

```php
use LTO\Transaction\Transfer;
use LTO\PublicNode;

$node = new PublicNode('https://nodes.lto.network');

$amount = 1000e8; // Amount of LTO to transfer (*10^8)
$recipient = "3Jo1JCrBvnWCg37VDxMXAjYhsS9rRDLBSze";

$transferTx = (new Transfer($amount, $recipient))
    ->signWith($account)
    ->broadcastTo($node);
```

## Transaction types

### Transfer

Send LTO tokens from your account to another account.

```php
use LTO\Transaction\Transfer;

$amount = 1000e8; // Amount of LTO to transfer (*10^8)
$recipient = "3Jo1JCrBvnWCg37VDxMXAjYhsS9rRDLBSze";

$transferTx = new Transfer($recipient, $amount);
```

Optionally you can set an attachment message for the transfer.

```php
$transferTx->setAttachment("Thanks");
```

### Mass Transfer

The Mass Transfer transaction is used to send LTO tokens from one account to multiple recipients. Recipients are added with the `addTransfer()` method.

```php
use LTO\Transaction\MassTransfer;

$massTransferTx = (new MassTransfer())
    ->addTransfer($recipient1, $amount1)
    ->addTransfer($recipient2, $amount2);
```

Optionally you can set an attachment for the mass transfer. The message applies to all recipients. It's not possible to specify a message per recipient.

{% hint style="info" %}
Mass transfer transactions have a base fee + a fee per recipient.
{% endhint %}

### Lease

Start leasing LTO to a node. This increased the chance for a node to mine a block and thus will increase mining rewards. Community nodes typically share this reward among the leasers. The leased tokens do not leave your wallet.

```php
use LTO\Transaction\Lease;

$amount = 5000e8; // Amount of LTO to lease (*10^8)
$recipient = "3JexCgRXGFUiuNoJTkkWucSumteRWdb8kKU";

$leaseTx = new Lease($recipient, $amount);
```

#### Cancel Lease

A lease can be canceled at any time through a Cancel Lease transaction. To cancel a lease, you need the id of the transaction that created the lease.

```php
use LTO\Transaction\Lease;

$leaseTxId = "BsWJsPyo7k3ytVjuNzfZWUrC1LpwqFTqgxx8J4YB2wgY";

$leaseTx = new CancelLease($leaseTxId);
```

Alternatively, create a Cancel Lease transaction from an existing Lease transaction using `$leaseTx->cancel()`. Note that the new Cancel Lease transaction needs to be signed and broadcasted.

```php
$leaseTx->cancel()
    ->signWith($account)
    ->broadcastTo($node);
```

### Sponsor

Sponsor an account, offering to pay for all transaction fees for that account.

```php
use LTO\Transaction\Sponsor;

$recipient = "3JexCgRXGFUiuNoJTkkWucSumteRWdb8kKU";

$sponsorTx = new Sponsor($recipient);
```

{% hint style="danger" %}
You should only sponsor an account you trust, and/or have a legally binding agreement with. A sponsored account holder can easily drain your account through spam transactions. If the account holder is running a node, he/she can claim part of the spend tokens as mining reward. **Limit the amount of tokens on the sponsoring account**, adding funds when necessary.
{% endhint %}

#### Cancel Sponsor

The sponsorship can be canceled at any time through a Cancel Sponsor transaction.

```php
use LTO\Transaction\CancelSponsor;

$recipient = "3JexCgRXGFUiuNoJTkkWucSumteRWdb8kKU";

$cancelSponsorTx = new CancelSponsor($recipient);
```

### Anchor

Write a hash of a document or data to the blockchain. This makes it possible to prove the timestamp and show the document hasn't been modified or tampered with.

```php
use LTO\Transaction\Anchor;

$hash = hash('sha256', 'Hello');

$anchorTx = new Anchor($hash, 'hex');
```

The hash can be given as hexadecimal \(`hex`\) value, which is the default, as binary \(`raw`\) or encoded with `base58` or `base64`.

A single anchor transaction can anchor up to 100 hashes to the blockchain. Multiple anchors should be added through the `addHash()` method.

```php
use LTO\Transaction\Anchor;

$anchorTx = (new Anchor())
    ->addHash(hash('sha256', 'one'))
    ->addHash(hash('sha256', 'two'))
    ->addHash(hash('sha256', 'three'));    
```

{% hint style="info" %}
Anchor transactions have a base fee + a fee per hash.
{% endhint %}

Use the `getHash()` or `getHashes()` method to get the hash values with a specific encoding \(`hex`, `raw`, `base58`, or `base64`\).

```php
$hashes = $anchorTx->getHashes('hex');
```

### Association

An association is a uni-directional relationship between two accounts. It can be used for setting up a trust network or as a graph for constructed DID identities.

#### Type

Associations have a numeric type. This type is context-specific and interpreted by the client applications that index associations.

{% hint style="info" %}
If you're creating a new protocol or specification that uses associations, please contact LTO Network to request assigning you a numeric range. This prevents choosing values that overlap the range of an existing protocol.
{% endhint %}

```php
use LTO\Transaction\Association;

$recipient = "3Jo1JCrBvnWCg37VDxMXAjYhsS9rRDLBSze";
$type = 0x10; // Endorsement

$associationTx = new Association($recipient, $type);
```

#### Hash

Optionally the association can have a hash. The meaning of the hash should be specified by the protocol that describes the meaning of the association.

```php
use LTO\Transaction\Association;

$recipient = "3Jo1JCrBvnWCg37VDxMXAjYhsS9rRDLBSze";
$type = 0x200; // Verified credential
$hash = hash('sha256', $credential);

$associationTx = new Association($recipient, $type, $hash);
```

An association with the same sender, recipient, type, and hash overwrites the existing association.

#### Expiration

An association can expire at a specific date and time.

```php
$associationTx = (new Association($recipient, $type))
    ->expiresOn(new \DateTime('now + 1 year'));
```

To prevent the association from expiring, it must be reissued before the expiry date. Calling `expiresOn()` on an existing Association transaction creates a new transaction that can be signed and broadcasted.

#### Revoke Association

Associations can be revoked at any time using a Revoke Association transaction. To revoke an association, the same recipient, type, and hash must be used as when the association was created.

```php
use LTO\Transaction\Association;

$recipient = "3Jo1JCrBvnWCg37VDxMXAjYhsS9rRDLBSze";
$type = 0x10; // Endorsement

$revokeAssociationTx = new RevokeAssociation($recipient, $type);
```

Alternatively, the `revoke()` method can be used on an existing association transaction. Note that this transaction needs to be signed and broadcasted. 

```php
$associationTx
    ->revoke()
    ->signWith($account)
    ->broadcastTo($node);
```

### Set Script

Smart accounts have a custom script that defines how transactions should be validated. The script needs to be compiled by the node before it's broadcasted as a transaction.

Scripts are written in the [Ride programming language](https://docs.waves.tech/en/ride/).

```php
use LTO\PublicNode;

$node = new PublicNode('https://nodes.lto.network');

$script = <<<SCRIPT
  match tx {
    case t:  TransferTransaction => false
    case mt: MassTransferTransaction => false
    case ss: SetScriptTransaction => false
    case _ => sigVerify(tx.bodyBytes, tx.proofs[0], tx.senderPublicKey)
  }
SCRIPT;

$node->compile($script)
    ->signWith($account)
    ->broadcastTo($node);
```

#### Clearing a script

To clear a script, use the Set Script transaction with a NULL script.

```php
use LTO\Transaction\SetScript;

$clearScriptTx = new SetScript(null);
```

## Sponsored fee

By default the account that signs the transaction has to pay the transaction fee. It's possible for another account to pay the fee instead through the sponsored fee feature.

```php
use LTO\Transaction\Anchor;

$anchorTx = (new Anchor())
    ->signWith($newAccount)
    ->payFeeWith($mainAccount)
    ->broadcastTo($node);
```

_In this example, a zero-anchor transaction is done by a new account to register it as an implicit identity._

## Multiple signatures

For smart accounts, a transaction might need multiple signatures. This can be done by calling `signWith()` multiple times.

```php
use LTO\Transaction\Transfer;
use LTO\PublicNode;

$node = new PublicNode('https://nodes.lto.network');

$amount = 1000e8; // Amount of LTO to transfer (*10^8)
$recipient = "3Jo1JCrBvnWCg37VDxMXAjYhsS9rRDLBSze";

$transferTx = (new Transfer($amount, $recipient))
    ->signWith($account1)
    ->signWith($account2)
    ->signWith($account3)
    ->broadcastTo($node);
```

It's unlikely that you'll have the private keys of all accounts at the same time because typically with multisig each key is controlled by a different individual. Instead, should store the signed transaction or share it \(over the private layer\) to have the second account add a signature and broadcast it.

```php
use LTO\Transaction;
use LTO\PublicNode;

$node = new PublicNode('https://nodes.lto.network');

$data = $mongodb->findOne(['id' => $txId]);
$tx = Transaction::fromData($data);

$tx->signWith($account);

if (count($tx->proofs) >= 2) {
    $node->broadcast($tx);
}
```

