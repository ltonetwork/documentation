# Accounts

## **Creation**

You can create a new random seed with keypair \(ed25519\):

```javascript
const account = lto.createAccount();
console.log(account.phrase); // 'satisfy sustain shiver skill betray mother appear pupil coconut weasel firm top puzzle monkey seek'
console.log(account.sign); // { privateKey: '4iZ5a5Qx2Utd1432omPUsKXifctCnUr25PjYoR7ohLbnXgG6sazdBg2iXbywzuh6VNWPiFPCudSV2du9HxGxT8mV', publicKey: 'EUkmkWG6TRbsZdQ9UjGySTzkMJq9eaKAjwJpW3Wv6DDH' }
```

## **Recovery**

It's also possible to recover a keypair from an existing seed:

```javascript
const phrase = 'satisfy sustain shiver skill betray mother appear pupil coconut weasel firm top puzzle monkey seek';

const seed = lto.seedFromExistingPhrase(phrase);
console.log(account.phrase); // 'satisfy sustain shiver skill betray mother appear pupil coconut weasel firm top puzzle monkey seek'
console.log(account.sign); // { privateKey: '4iZ5a5Qx2Utd1432omPUsKXifctCnUr25PjYoR7ohLbnXgG6sazdBg2iXbywzuh6VNWPiFPCudSV2du9HxGxT8mV', publicKey: 'EUkmkWG6TRbsZdQ9UjGySTzkMJq9eaKAjwJpW3Wv6DDH' }
```

## **Seed Encryption**

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

