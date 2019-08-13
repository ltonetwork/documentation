# Utils

**POST /utils/hash/secure**

![master](https://img.shields.io/badge/MAINNET-available-4bc51d.svg)

Produce a secure hash of a specified message.

**Request:**

```text
ltonetwork!
```

**Response JSON example:**

```javascript
{
  "message": "ltonetwork!",
  "hash": "CU8QRLgxAbwsL616cZBRouXHfUf9dJVfiUZza5egGTs2"
}
```

#### POST /utils/hash/fast

![master](https://img.shields.io/badge/MAINNET-available-4bc51d.svg)

Fast hash of specified message.

**Request:**

```text
ltonetwork!
```

**Response JSON example:**

```javascript
{
  "message": "ltonetwork!",
  "hash": "2w3ezYt5p3KfEZ8K2dX9SiQzSuxTUy5AV4VW7AoPWHCF"
}
```

#### GET /utils/seed/{length}

![master](https://img.shields.io/badge/MAINNET-available-4bc51d.svg)

Generate a random seed of specified length.

**Response JSON example:**

```javascript
{
  "seed": "3XcHLU6bYRax1c"
}
```

#### GET /utils/seed

![master](https://img.shields.io/badge/MAINNET-available-4bc51d.svg)

Generate a random seed.

**Response JSON example:**

```javascript
{
  "seed": "2uwLAe7Rp7TuNiBTKsmTEJ5wxGqkBHjcyPq2tMXiWye7"
}
```

