# Lease Transactions

#### POST /leasing/lease

![master](https://img.shields.io/badge/MAINNET-available-4bc51d.svg)

Creates lease transaction.

**Request params**

```text
"sender" - Sender address, Base58-encoded
"fee" - Amount of transaction fee
"amount" - amount of leased waves
```

**Request JSON example**

```javascript
 {
  "sender" : "3HgqG68qfeVz5dqbyvqnxQceFaH49xmGvUS",
  "fee" : 500000000,
  "amount" : 500000000,
  "recipient" : "address:3HQanDJhZSsSLbCjTCsMYpPvuj2ieGwKwQ9"
}
```

**Response JSON example**

```javascript
{
 "type":10,
 "id":"9q7X84wFuVvKqRdDQeWbtBmpsHt9SXFbvPPtUuKBVxxr",
 "sender":"3MtrNP7AkTRuBhX4CBti6iT21pQpEnmHtyw",
 "senderPublicKey":"G6h72icCSjdW2A89QWDb37hyXJoYKq3XuCUJY2joS3EU",
 "fee":100000000,
 "timestamp":46305781705234713,
 "signature":"4gQyPXzJFEzMbsCd9u5n3B2WauEc4172ssyrXCL882oNa8NfNihnpKianHXrHWnZs1RzDLbQ9rcRYnSqxKWfEPJG"
}
```

#### POST /leasing/cancel

![master](https://img.shields.io/badge/MAINNET-available-4bc51d.svg)

Creates lease cancel transaction.

**Request params**

```text
"sender" - Sender address, Base58-encoded
"fee" - Amount of transaction fee
"leaseId" - lease id for cancel
```

**Request JSON example**

```javascript
{
  "sender" : "3HgqG68qfeVz5dqbyvqnxQceFaH49xmGvUS",
  "fee" : 500000000,
  "leaseId" : "CYPYhYe9M94t958Nsa3DcYNBZTURwcFgQ3ojyjwEeZiK"
}
```

**Response JSON example**

```javascript
{
  "type" : 9,
  "id" : "895ryYABK7KQWLvSbw8o8YSjTTXHCqRJw1yzC63j4Fgk",
  "sender" : "3HgqG68qfeVz5dqbyvqnxQceFaH49xmGvUS",
  "senderPublicKey" : "DddGQs63eWAA1G1ZJnJDVSrCpMS97NH4odnggwUV42kE",
  "fee" : 500000000,
  "timestamp" : 1495625418143,
  "signature" : "2SUmFj4zo7NfZK7Xoqvqh7m7bhzFR8rT7eLtqe9Rrp18ugFH9SSvoTx1BtekWhU7PN1uLrnQCpJdS8JhmcBAjmb9",
  "leaseId" : "CYPYhYe9M94t958Nsa3DcYNBZTURwcFgQ3ojyjwEeZiK"
}
```

#### 

