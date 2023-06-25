# Accounts

## **Creation**

You can create a new account with a random seed with keypair (ed25519):

```javascript
import LTO from '@ltonetwork/lto';

const lto = new LTO("T");
const account = lto.account();

console.log(account.seed);  // lion devote brush lemon salmon eyebrow near autumn aspect april ugly position dismiss suit finger
console.log(account.publicKey);  // AvWa7XokpR284pNCnoKZhudQdNA5AV3PXPi6HhggAhbT
console.log(account.privateKey);  // 4dXzhzRcpiukcRBUGfre8s8aRaUqwyKHUzfbQTtNRRMFxZXQ6BsbfKPbA2QVBELNjoxxy6NQkii6HVg1zPzti4mB
```

It's also possible to recover a keypair from an existing seed:

```javascript
import LTO from '@ltonetwork/lto';

const lto = new LTO('T');
const seed = 'satisfy sustain shiver skill betray mother appear pupil coconut weasel firm top puzzle monkey seek';
const account = lto.account({ seed });
```

### Nonce

You can create multiple accounts from a single seed phrase, by passing a nonce.

```javascript
import LTO from '@ltonetwork/lto';

const lto = new LTO('T');
const seed = 'satisfy sustain shiver skill betray mother appear pupil coconut weasel firm top puzzle monkey seek';

const account1 = lto.account({ seed, nonce: 0 });
const account2 = lto.account({ seed, nonce: 1 });
const account3 = lto.account({ seed, nonce: 2 });
```

Alternatively, pass a binary value as a nonce. Use the `Binary` class to convert a string to a binary value

```javascript
import LTO, { Binary } from '@ltonetwork/lto';

const lto = new LTO('T');
const seed = 'satisfy sustain shiver skill betray mother appear pupil coconut weasel firm top puzzle monkey seek';

const account = lto.account({ seed, nonce: new Binary('some value') });
```

### Multi-chain accounts

LTO Networks supports 3 ciphers: ed25519, secp256k1, and secp256r1. To create an Ethereum-compatible key pair, use the secp256k1 key type and set the correct derivation path

```javascript
import LTO from '@ltonetwork/lto';

const lto = new LTO('T');
const account = lto.account({ keyType: 'secp256k1', derivationPath: `m/44'/60'/0'/0` });

console.log(account.getAddressOnNetwork('ethereum'));
```

## Seed encryption

It's recommended to encrypt your seed when storing the account

```javascript
import LTO from '@ltonetwork/lto';

const lto = new LTO('T');
const seed = 'satisfy sustain shiver skill betray mother appear pupil coconut weasel firm top puzzle monkey seek';
const account = lto.account({ seed });

const password = 'verysecretpassword';
const encrypted = account.encrypt(password); 

console.log(encrypted); //U2FsdGVkX18tLqNbaYdDu5V27VYD4iSylvKnBjMmvQoFFJO1KbsoKKW1eK/y6kqahvv4eak8Uf8tO1w2I9hbcWFUJDysZh1UyaZt6TmXwYfUZq163e9qRhPn4xC8VkxFCymdzYNBAZgyw8ziRhSujujiDZFT3PTmhhkBwIT7FMs=
```

Supply the password to create the account from an encrypted seed

```javascript
import LTO from '@ltonetwork/lto';

const encryptedSeed = 'U2FsdGVkX18tLqNbaYdDu5V27VYD4iSylvKnBjMmvQoFFJO1KbsoKKW1eK/y6kqahvv4eak8Uf8tO1w2I9hbcWFUJDysZh1UyaZt6TmXwYfUZq163e9qRhPn4xC8VkxFCymdzYNBAZgyw8ziRhSujujiDZFT3PTmhhkBwIT7FMs=';
const password = 'verysecretpassword';

const account = lto.account({ seed: encryptedSeed, password });

console.log(account.phrase); // satisfy sustain shiver skill betray mother appear pupil coconut weasel firm top puzzle monkey seek
```
