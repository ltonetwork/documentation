# Anchoring Guide

Anchoring on a blockchain is a simple and cheap way to notarize documents and timestamp data. Authorization levels and ACLs offer limited protection against manipulation by those in charge or tasked to maintain the system. You can provide indisputable proof of existence by securing a hash on the LTO Network public blockchain: [https://anchor-demo.lto.network/demo](https://anchor-demo.lto.network/demo/)

### Hashing

An anchor transaction accepts any 64 bytes \(512 bits\) as the hash. This allows you to choose almost any hashing algorithm. Based on your requirements choose a fast algorithm like SHA-2 or BLAKE2b or a slow algorithm like BCrypt. The algorithm isn’t stored in the transaction, it’s up to you pick and use one consistently or to store which algorithm was used.

To hash a document, read it to apply the hashing function on the binary data. Structured data should be serialized \(e.g. as JSON\). Beware that serialization process should be deterministic; in other words, if you provide the same data, you should always get the same serialized data.

### **Plain hash**

For public records, simply apply the hashing function on the data and use that for the anchor transaction. This could be applicable for public archives, allowing them to prove the authenticity of stored documents.

This should not be used for private data as the hash could leak information. For instance, if an organization would create a hash from client-data existing of the customer name, address, etc, an unauthorized party might be able to recreate the hash and find out if a person is a client with this company.

[_See hashing examples in several languages_](https://gist.github.com/jasny/2200f68f8109b22e61863466374a5c1d#file-sha256-md)

### **Peppered hash \(HMAC\)**

With a peppered hash, a secret key is used in addition to the data to create the hash. You need both the key and data to recreate and thus verify a hash. This makes it impossible to recreate a hash, based only on publicly available data.

The secret key can be a password used by the application for all hashes or a string of random bytes, which is stored together with the data. It’s recommended that the secret is \(at least\) 32 bytes long.

HMAC is a standardized method to create a peppered hash. It utilizes a pseudorandom function rather than simply concatenating the pepper and the data.

> **If the data contains personal data of an individual, you MUST use a 32-byte random secret key in order to comply with GDPR regulations.**

[_See HMAC examples in several languages_](https://gist.github.com/jasny/2200f68f8109b22e61863466374a5c1d#file-sha256-hmac-md)

### **Signature hash**

Rather than hashing the data, it’s also an option to create a hash of a cryptographic signature. This proofs that not only that the document or data existed on a specific time, but also that is was authorized by a specific party.

Signatures should be created using an asymmetric algorithm with a private key for signing and a public key for verification. To verify authenticity, the document, signature and public key must be provided.

### **Salt**

A salt is a string of random bytes added to the data. Unlike a pepper, a salt is visible for everybody as it’s appended to the hash.

Salting a hash prevents recreating a hash and then trying to find it by scanning all anchor transactions on the blockchain as can be done through LTO history nodes.

Some algorithms like BCrypt will always salt the hash. In other cases, you can choose to add the salt manually. Due to the limit of 64 bytes, it’s not possible to add a salt to a 512 bit hash.

For 256 bit algorithms prepend 32 random bytes to the data. Then append this 32-byte string to the hash.

### Certificate

An alternative method is to create a certificate that contains the hash of the document as well as other metadata. The hash of the certificate is used for anchoring.

[Yaml](https://yaml.org/) is a recommended format at it’s easy to read for both humans and computers.

```text
certkey: 87XN8jg78YfwTtP+m+YFASxd0j4aKfN/JmmJDDXnXAk=
document:
  name: blank.txt
  hash: 47DEQpj8HBSa+/TImW+5JCeuQeRkm5NMpJWZG3hSuFU=
owner:
  name: Acme Corp.
  public_key: QIhH7QuVkc6/xY+fFzEJt6v9BSZBzDTH6mA29HbsLlE=
date: 2018-03-06T20:12:00+0100
signature: Pnuj1viOLFNtyV1sLialluW4jDj1ZWf6+KjZrazhEq0U028PCOBulc…
```

In this case, `certkey` is a random value that functions as pepper. All values are base64 encoded, but you could also choose base58 encoding or hexadecimal representation.

The document is connected to the certificate via the document hash. The certificate can be verified on the LTO public blockchain.

### Anchoring

The LTO node comes in several flavors. Anchoring nodes expose an HTTP REST interface allowing you to easily submit anchor transactions. The transactions are signed by the node and \(thus\) paid by the account associated with the node.

To anchor send a POST request to [_http://my-lto-node.example.com/hash_](http://my-lto-node.example.com/hash) with a JSON body \(replace `my-lto-node.example.com` with the domain name or IP of your anchoring node\).

```text
{
  "hash": "47DEQpj8HBSa+/TImW+5JCeuQeRkm5NMpJWZG3hSuFU=",
  "encoding": "base64"
}
```

The following encodings are accepted; _hex_, _base58,_ and _base64_.

If the node is set up correctly, you’ll also need to provide the API key for each request as `X-LTO-Key` request header. This key has been configured as environment variable `LTO_API_KEY` during set up.

The node exposes a Swagger UI which you can use to try out all the available HTTP endpoints.

![Swagger interface LTO Network Anchoring node](https://cdn-images-1.medium.com/max/1600/1*-tuVnK4w9JuAxc5HP2l9Ag.png)

