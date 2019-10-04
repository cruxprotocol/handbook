# CRUX Protocol Risk Analysis


## Contents

* [1 Introduction](#1-introduction)
* [2 Risk Assets](#2-risk-assets)
    * [2.1 CRUX Identity Private Key](#21-crux-identity-private-key)
        * [2.1.1 Threat - Unauthorized access of Private Key at rest](#211-threat---unauthorized-access-of-private-key-at-rest)
        * [2.1.2 Threat - Memory disclosure attack](#212-threat---memory-disclosure-attack)
    * [2.2 CRUX ID Registration](#22-crux-id-registration)
        * [2.2.1 Threat - Hijacking of Names](#221-threat---hijacking-of-names)
        * [2.2.2 Threat - Frontrunning of IDs](#222-threat---frontrunning-of-ids)
    * [2.3 CRUX ID Name Resolution](#23-crux-id-name-resolution)
        * [2.3.1 Threat - Malicious Resolver](#231-threat---malicious-resolver)
    * [2.4 CRUXPay Public Addresses](#24-cruxpay-public-addresses)
        * [2.4.1 Threat - Unauthorized Modification of Public Addresses](#241-threat---unauthorized-modification-of-public-addresses)
    * [2.5 CRUXConnect Communication Messaging](#25-cruxconnect-communication-messaging)
 

## 1 Introduction

We recommend going through at least the [Problem](https://github.com/cruxprotocol/handbook#2-motivation--problem) and [Solution](https://github.com/cruxprotocol/handbook#3-solution) section to most effectively understand the contents of this document.
  
CRUX has been designed after extensive assessment of threats to various parts of the solution. That being said, we urge the community to probe deeper and ask more searching questions if need be.

The way we analyze Risks is by breaking the concept down into Assets, Threats and Vulnerabilities.

- An asset is what we’re trying to protect.
- A threat is what we’re trying to protect against.
- A vulnerability is a potential weakness or gap in our protection efforts.

Risk is the intersection of assets, threats, and vulnerabilities.

In the next section, we identify sensitive Assets in CRUX and try to understand the major new threat vectors they introduce into the picture. For each threat we'll go deeper into how CRUX's design deals with that threat.


## 2 Risk Assets

### 2.1 CRUX Identity Private Key  

CRUX protocol works around the concept of a User owned identity called the CRUX ID. This identity is represented by a ECDSA keypair and can be stored securely in any Wallet of the User's choice.

##### 2.1.1 Threat - Unauthorized access of Private Key at rest

Even if the User's device is not compromised, we need to ensure the security of the CRUX ID Private Key. To achieve this we need to make sure the Private Key is encrypted at rest. 

This is the same problem Wallets face when managing the User's cryptocurrency related keypairs. Wallets keep private keys encrypted at rest with the help of another secret provided by the User in the form of a PIN/Password/Fingerprint. 

The CRUX SDK requires wallets to implement a method which gives CRUX SDK an arbitrary key derived from such a User secret. The SDK uses this key to encrypt the private key at rest, and to decrypt it when required.

``` 
// example implementation

let getPassHashedEncryptionKey = () => {
    let user_password = getUserPassword();  // to be implemented by wallet
    return crypto.createHash('sha256').update(user_password).digest();
}

let cruxClient = new CruxClient({
    getEncryptionKey: getPassHashedEncryptionKey,     
    walletClientName: 'cruxdev'
}).init();

```

##### 2.1.2 Threat - Memory disclosure attack

In practical implementations of cryptosystems, the private keys are usually loaded into the memory as plaintext, and then used in the cryptographic algorithms. Therefore, the private keys are subject to memory disclosure attacks that read unauthorized data from RAM. Such attacks could be performed through software methods (e.g., Open SSL Heart bleed) even when the integrity of the victim system's executable binaries is maintained. They could also be performed through physical methods (e.g., Cold-boot attacks on RAM chips) even when the system is free of software vulnerabilities.

How do we ensure security in such an environment? 

The first way CRUX mitigates this is by not introducing any new trust entities into the User's workflow. The User can trust the same Wallet she trusts for managing her regular cryptocurrency private keys. We do not introduce any new attack vectors into the equation.

The second we this is mitigated is by adhering to a strict on-demand decryption policy in code which ensures the private key is in memory in a decrypted form for the shortest amount of time.

```
await this._payIDClaim.decrypt();
const result = sensitiveOperation()
await this._payIDClaim.encrypt();
```


### 2.2 CRUX ID Registration 

Users register a human readable CRUX name which they use in protocols such as CRUXPay and CRUXGateway. 

This ID is the User's representative in all CRUX Protocol participants in the Crypto ecosystem. This registration needs to be held at the highest security standards.



##### 2.2.1 Threat - Hijacking of Names

In DNS and on Social Media, names are globally unique and human-readable, but not strongly-owned. The system operator has the final say as to what each names resolves to. 

CRUX IDs are strongly-owned in the form of a keypair stored in any CRUX compatible Wallet of the User's choice. Without the User's private key it is impossible to hijack this identity from the User.

At the heart of CRUX IDs architecture is the Blockstack Naming Service which helps maintain this strongly owned mapping. Blockstack uses a standard secp256k1 keypair (identical to bitcoin) to represent ownership of Blockstack IDs.

How does a the private key ensure ownership and global uniqueness of the name? This is ensured by the Blockstack Naming Service. You can find out how BNS works [here](https://docs.blockstack.org/core/naming/introduction.html). In a nutshell, we can give instructions to BNS using transactions on the Bitcoin Blockchain. To register subdomains, we need to send a [NAME_UPDATE](https://docs.blockstack.org/core/wire-format.html) transaction on the Bitcoin Blockchain. This transaction contains very minimal information
- Proof of ownership of Subdomain & Domain (signatures of respective private keys)
- Hash of an entity known as 'ZoneFile'. 

For eg. - [e2029990fa75e9fc642f149dad196ac6b64b9c4a6db254f23a580b7508fc34d7](https://blockchair.com/bitcoin/transaction/e2029990fa75e9fc642f149dad196ac6b64b9c4a6db254f23a580b7508fc34d7)

'ZoneFile' can contain any arbitrary data. Only the hash of this Zonefile is stored on the Blockchain. The Zonefile itself is broadcasted to the BNS Network. The network waits for the relevant Bitcoin transaction to get confirmations, at which point it updates its indexes with the updated Zonefile hash and Zonefile against the name whose ownership was proved in the Bitcoin transaction.

This way the registration is secured by the Bitcoin Blockchain's hash power.



##### 2.2.2 Threat - Frontrunning of IDs

The registration process involves entities known as Registrars. The User (via their Wallets) ask the Registrars to register names on their behalf. 
The registrar needs to 'commit' the transaction to the Bitcoin Blockchain. This transaction needs to be mined in a block with at least '6' confirmations for it to go 'live' on the BNS Network.

If the registrar desires so, he can try to fool the User by registering its own public key with the User's human readable name in order to fool the user. 

Mitigation -  
- SDK will automatically check the ID registration against the local Public Key, once the registration is live. The user will not be able to use the ID before full registration i.e. 6 confirmations.
- CRUX provides a free to use Registrar for CRUX IDs. But we also encourage and empower Wallets to run their own Registrars to reduce the number of trust entities the User has to trust. In fact, tech savvy users can choose to run their own Registrars as well.
 

### 2.3 CRUX ID Name Resolution

Once a CRUX ID is registered with a human readable name, we expect anyone to be able to 'resolve' this name. We learnt that CRUX IDs map 1:1 to Blockstack IDs.  

What do we mean by resolve? Resolve means to translate a Blockstack ID to a Public Key. This is very similar to how DNS 'resolves' a easy to read website URL to an IP Address.

In the above section we mitigated threats to ID Registration, but the ID Resolution process is equally crucial to ensure security. One is useless without the other.

##### 2.3.1 Threat - Malicious Resolver

The Resolver is an entity which tells us the Public Key corresponding to a Blockstack ID. Asking the resolver for a name is a simple HTTP API call. How can we expect Users to trust our resolver? This is a key question.

In our case the resolvers are nodes part of the [Blockstack Naming Service ](https://docs.blockstack.org/core/naming/introduction.html) network known as BNS Nodes. The only way to mitigate this issue is by resolving the name from multiple independent parties. The resolution succeeds if and only if all parties return the same response. This is known as the Verifier Pool. It contains - 
1. A BNS Node run by Blockstack
2. A BNS Node run by CRUX creators
3. A BNS Node run by each individual Wallet. 

We will also provide options for tech savvy users to add their own trusted BNS Nodes as well. 

### 2.4 CRUXPay Public Addresses

CRUXPay allows Users to make chosen addresses of cryptocurrencies 'Public'. This means anyone in the world will be able to pay those cryptocurrencies straight to their human readable CRUX ID Name such as `emily@crux`. 



##### 2.4.1 Threat - Unauthorized Modification of Public Addresses 

We need to make sure that the User and only the User can make any changes to the Public Addresses associated to a CRUX ID. How can we make sure of that?

Blockstack provides a mechanism to do this known as [Gaia Hubs](https://github.com/blockstack/gaia). Gaia works by hosting data in one or more existing storage systems of the user's choice. These storage systems are typically cloud storage systems. There is existing support for S3 and Azure Blob Storage, but the driver model allows for other backend support as well. The point is, the user gets to choose where their data lives, and Gaia enables applications to access it via a uniform API.

Writes to Gaia requires an authentication token which needs to be signed with the User owned private key. Any data written is always written along with an [HMAC](https://en.wikipedia.org/wiki/HMAC); also generated using the same private key. The Auth token helps the Gaia Hub ensure write-time correctness and the content's HMAC sets others up to do read-time verification.

Any reads to the Gaia data from the CRUX SDK can verify the Authenticity and Integrity of the data using the HMAC. Verifying the data using HMAC requires a public key from a trusted source. We have already established a trusted Public Key Infrastructure system in the form of Blockstack IDs and the associated Blockstack Naming Service. We ask multiple independently run nodes for the public key. 


For example 

![Problem2](https://s3-ap-southeast-1.amazonaws.com/files.coinswitch.co/cruxpay/handbook_images/solution2.png)

Here we see User A uses his Wallet to make certain addresses public on Gaia. He is able to write to Gaia Hub because his CRUX private key is present on his Wallet. 


![Problem2](https://s3-ap-southeast-1.amazonaws.com/files.coinswitch.co/cruxpay/handbook_images/solution2b.png)

User B resolves is able to get User A's public key by querying the BNS Network. User B's Wallet asks multiple independent nodes in the network for User A's public key.  
User B is able to derive User A's Gaia record location from this information. User B fetches the Gaia record for User A's public addresses. His Wallet verifies the data's integrity and authenticity using the Public Key he had got earlier. 

This process ensures User B is paying exactly to the intended address.


### 2.5 CRUXConnect Communication Messaging

[upcoming]


