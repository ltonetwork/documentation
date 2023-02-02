# Transaction fees

Transaction fees act as a reward for the miner. The fees are enforced by the consensus model. The transaction fee can be voted up or down through fee voting.

### Fee distribution

For every transaction, 50% of the fee isn't awarded and thus effectively taken out of circulation (aka burned). The remaining fee is split up among the current leader, and the next elected node at a ratio of 2:3.

The fee is distributed 20% to the leader, 30% to the next one, and 50% is burned. For more information, read about the [NG consensus algorithm](fair\_proof\_of\_stake\_fpos.md#ng-protocol).

### Sponsored accounts

Normally the fee is automatically deducted from the sender's address. With sponsored accounts, it's possible for a third party to pay for all transaction fees of an account.

If the sponsor has insufficient funds, the fee is deducted from the sender's account. This prevents a third party to disable an account through a dummy sponsorship.

If an account has multiple sponsors, it works as last in first out. If the most recently added sponsor has insufficient funds, the next sponsor is tried, this continues until the sender's account is charged for the fee.

{% content-ref url="transactions/sponsor.md" %}
[sponsor.md](transactions/sponsor.md)
{% endcontent-ref %}

### Sponsored transactions

By default the account that signs the transaction also pays the transaction fees. It's possible for a different account to pay the fee instead this account needs to co-sign the transaction and add it as proof.

```javascript
{
  "type": 15,
  "version": 3,
  "id": "8M6dgn85eh3bsHrVhWng8FNaHBcHEJD4MPZ5ZzCciyon",
  "sender": "3Jq8mnhRquuXCiFUwTLZFVSzmQt3Fu6F7HQ",
  "senderPublicKey": "AJVNfYjTvDD2GWKPejHbKPLxdvwXjAnhJzo6KCv17nne",
  "sponsor": "3JiPMnx485EVhasHfD4f36v4Hydmn7XYFFo",
  "sponsorPublicKey": "22wYfvU2op1f3s4RMRL2bwWBmtHCAB6t3cRwnzRJ1BNz"
  "fee": 35000000,
  "timestamp": 1610397549043,
  "anchors": [],
  "proofs": [
    "4aMwABCZwtXrGGKmBdHdR5VVFqG51v5dPoyfDVZ7jfgD3jqc851ME5QkToQdfSRTqQmvnB9YT4tCBPcMzi59fZye"
    "58oNafDLERaW9crixHFv9mwiaA3miDJQtAAQMdB9xRFLaYMQRB8fGqpZWeB5w6kBbT6mbcxpyXFFSFMG6xE51RaU"
  ],
  "height": 1069662
}
```

_This is a zero-anchor transaction to register  `did:lto:3Jq8mnhRquuXCiFUwTLZFVSzmQt3Fu6F7HQ`. The transaction fee is paid by account `3JiPMnx485EVhasHfD4f36v4Hydmn7XYFFo`._

The account that sponsors a transaction, can be a sponsored account. In that case, the fee will be paid by the account's sponsor.

{% hint style="danger" %}
Sponsoring transactions is intended for accounts that don't hold any tokens. Beware that the sponsor isn't part of the binary message that's signed. This means that a malicious node could remove the signature of the sponsor, forcing the transaction to be paid by the sender.
{% endhint %}
