# Accounts

## **Creation**

You can create a new account with a random seed with keypair (ed25519):

```javascript
import LTO from "@ltonetwork/lto";

const lto = new LTO("T");
const account = lto.account();

console.log(account.seed);  // lion devote brush lemon salmon eyebrow near autumn aspect april ugly position dismiss suit finger
console.log(account.publicKey);  // AvWa7XokpR284pNCnoKZhudQdNA5AV3PXPi6HhggAhbT
console.log(account.privateKey);  // 4dXzhzRcpiukcRBUGfre8s8aRaUqwyKHUzfbQTtNRRMFxZXQ6BsbfKPbA2QVBELNjoxxy6NQkii6HVg1zPzti4mB
```

It's also possible to recover a keypair from an existing seed:

```javascript
import { AccountFactoryECDSA } from '@ltonetwork/lto/raw/accounts';

const seed = "const seed = 'satisfy sustain shiver skill betray mother appear pupil coconut weasel firm top puzzle monkey seek';"
const account = new AccountFactoryED25519("T").createFromSeed(seed);
```

Your seed can be encrypted:

```javascript
const phrase = 'satisfy sustain shiver skill betray mother appear pupil coconut weasel firm top puzzle monkey seek';

const account = lto.seedFromExistingPhrase(phrase);

const password = 'verysecretpassword';
const encrypted = account.encrypt(password); 
console.log(encrypted); //U2FsdGVkX18tLqNbaYdDu5V27VYD4iSylvKnBjMmvQoFFJO1KbsoKKW1eK/y6kqahvv4eak8Uf8tO1w2I9hbcWFUJDysZh1UyaZt6TmXwYfUZq163e9qRhPn4xC8VkxFCymdzYNBAZgyw8ziRhSujujiDZFT3PTmhhkBwIT7FMs=
```

or

```javascript
const phrase = 'satisfy sustain shiver skill betray mother appear pupil coconut weasel firm top puzzle monkey seek';

const password = 'verysecretpassword';
const encrypted = lto.encryptSeedPhrase(phrase, password);
console.log(encrypted); // U2FsdGVkX18tLqNbaYdDu5V27VYD4iSylvKnBjMmvQoFFJO1KbsoKKW1eK/y6kqahvv4eak8Uf8tO1w2I9hbcWFUJDysZh1UyaZt6TmXwYfUZq163e9qRhPn4xC8VkxFCymdzYNBAZgyw8ziRhSujujiDZFT3PTmhhkBwIT7FMs=
```

### **Seed decryption**

To decrypt your seed:

```javascript
const encryptedSeed = 'U2FsdGVkX18tLqNbaYdDu5V27VYD4iSylvKnBjMmvQoFFJO1KbsoKKW1eK/y6kqahvv4eak8Uf8tO1w2I9hbcWFUJDysZh1UyaZt6TmXwYfUZq163e9qRhPn4xC8VkxFCymdzYNBAZgyw8ziRhSujujiDZFT3PTmhhkBwIT7FMs=';
const password = 'verysecretpassword';
const phrase = lto.decryptSeedPhrase(encryptedSeed);
console.log(phrase); // satisfy sustain shiver skill betray mother appear pupil coconut weasel firm top puzzle monkey seek
```
