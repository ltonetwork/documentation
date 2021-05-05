# Accounts

## Creation

### **Create an account from seed**

```php
$seedText = "manage manual recall harvest series desert melt police rose hollow moral pledge kitten position add";

$factory = new LTO\AccountFactory('T'); // 'T' for testnet, 'L' for mainnet
$account = $factory->seed($seedText);
```

### **Create an account from private sign key**

```php
$secretKey = 'wJ4WH8dD88fSkNdFQRjaAhjFUZzZhV5yiDLDwNUnp6bYwRXrvWV8MJhQ9HL9uqMDG1n7XpTGZx7PafqaayQV8Rp';

$factory = new LTO\AccountFactory('T'); // 'T' for testnet, 'L' for mainnet
$account = $factory->create($secretKey);
```

### **Create an account from full info**

```php
$accountInfo = [
  'address' => '3PLSsSDUn3kZdGe8qWEDak9y8oAjLVecXV1',
  'sign' => [
    'secretkey' => 'wJ4WH8dD88fSkNdFQRjaAhjFUZzZhV5yiDLDwNUnp6bYwRXrvWV8MJhQ9HL9uqMDG1n7XpTGZx7PafqaayQV8Rp',
    'publickey' => 'FkU1XyfrCftc4pQKXCrrDyRLSnifX1SMvmx1CYiiyB3Y'
  ],
  'encrypt' => [
    'secretkey' => 'BnjFJJarge15FiqcxrB7Mzt68nseBXXR4LQ54qFBsWJN',
    'publickey' => 'BVv1ZuE3gKFa6krwWJQwEmrLYUESuUabNCXgYTmCoBt6'
  ]
];

$factory = new LTO\AccountFactory('T'); // 'T' for testnet, 'L' for mainnet
$account = $factory->create($accountInfo);
```

Properties that are specified will be verified. Properties that are omitted will be generated where possible.

## Signing \(ED25519\)

### **Sign a message**

```php
$signature = $account->sign('hello world'); // Base58 encoded signature
```

### **Verify a signature**

```php
if (!$account->verify($signature, 'hello world')) {
    throw new RuntimeException('invalid signature');
}
```

## Encryption \(X25519\)

### **Encrypt a message for another account**

```php
$message = 'hello world';

$recipientPublicKey = "HBqhfdFASRQ5eBBpu2y6c6KKi1az6bMx8v1JxX4iW1Q8"; // base58 encoded X25519 public key
$recipient = $factory->createPublic(null, $recipientPublicKey);

$cyphertext = $account->encryptFor($recipient, $message); // Raw binary, not encoded
```

You can use `$account->encryptFor($account, $message)` to encrypt a message for yourself.

### **Decrypt a message received from another account**

```php
$senderPublicKey = "HBqhfdFASRQ5eBBpu2y6c6KKi1az6bMx8v1JxX4iW1Q8"; // base58 encoded X25519 public key
$sender = $factory->createPublic(null, $senderPublicKey);

$message = $account->decryptFrom($sender, $cyphertext);
```

You can use `$account->decryptFrom($account, $message)` to decrypt a message from yourself.

