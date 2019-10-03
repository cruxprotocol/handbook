# CRUX Protocol Risk Analysis

## Preface

We recommend going through at least the [Problem](https://github.com/cruxprotocol/handbook#2-motivation--problem) and [Solution](https://github.com/cruxprotocol/handbook#3-solution) section to most effectively understand the contents of this document.
  
CRUX has been designed after extensive assessment of threats to various parts of the solution. That being said, we urge the community to probe deeper and ask more searching questions if need be.

In the next section, we identify the risks associated with each part of the CRUX Protocol. Along with each identified risk, we explain how CRUX's architecture mitigates each risk. We also clearly lay out the areas of potential improvement.  

A good way to analyze Risks is to break them down into Assets, Threats and Vulnerabilities.
- An asset is what we’re trying to protect.
- A threat is what we’re trying to protect against.
- A vulnerability is a potential weakness or gap in our protection efforts.

Risk is the intersection of assets, threats, and vulnerabilities.

## Risk Assets

### CRUX Identity Private Key  

CRUX protocol works around the concept of a User owned identity called the CRUX ID. This identity is represented by a ECDSA keypair and can be stored securely in any Wallet of the User's choice.

##### Threat - Unauthorized access of Private Key at rest

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

##### Threat - Memory disclosure attack

In practical implementations of cryptosystems, the private keys are usually loaded into the memory as plaintext, and then used in the cryptographic algorithms. Therefore, the private keys are subject to memory disclosure attacks that read unauthorized data from RAM. Such attacks could be performed through software methods (e.g., Open SSL Heart bleed) even when the integrity of the victim system's executable binaries is maintained. They could also be performed through physical methods (e.g., Cold-boot attacks on RAM chips) even when the system is free of software vulnerabilities.

How do we ensure security in such an environment? 

The first way CRUX mitigates this is by not introducing any new trust entities into the User's workflow. The User can trust the same Wallet she trusts for managing her regular cryptocurrency private keys. 

The second we this is mitigated is by adhering to a strict on-demand decryption policy which ensures the private key is in memory in a decrypted form for the shortest amount of time.

```
await this._payIDClaim.decrypt();
const result = sensitiveOperation()
await this._payIDClaim.encrypt();
```


### CRUX ID Registration 

Users register a human readable CRUX name which they use in protocols such as CRUXPay and CRUXGateway. 

This ID is the User's representative in all CRUX Protocol participants in the Crypto ecosystem. This registration needs to be held at the highest security standards.



##### Threat - Hijacking of Names

In DNS and on Social Media, names are globally unique and human-readable, but not strongly-owned. The system operator has the final say as to what each names resolves to. 

CRUX IDs are strongly-owned in the form of a keypair stored in any CRUX compatible Wallet of the User's choice. Without the User's private key it is impossible to hijack this identity from the User.

At the heart of CRUX IDs architecture is the Blockstack Naming Service which helps maintain this strongly owned mapping. Blockstack uses a standard secp256k1 keypair (identical to bitcoin) to represent ownership of Blockstack IDs.


##### Threat - Frontrunning of IDs

The registration process involves entities known as Registrars. The User (via their Wallets) ask the Registrars to register names on their behalf. 
The registrar needs to 'commit' the transaction to the Bitcoin Blockchain. This transaction needs to be mined in a block with at least '6' confirmations for it to go 'live' on the BNS Network.

If the registrar desires so, he can try to fool the User by registering its own public key with the User's human readable name in order to fool the user. 

Mitigation -  
- SDK will automatically check the ID registration against the local Public Key, once the registration is live. The user will not be able to use the ID before full registration i.e. 6 confirmations.
- CRUX provides a free to use Registrar for CRUX IDs. But we also encourage and empower Wallets to run their own Registrars to reduce the number of trust entities the User has to trust. In fact, tech savvy users can choose to run their own Registrars as well.
 

### CRUX ID Name Resolution

Once a CRUX ID is registered with a human readable name, we expect anyone to be able to 'resolve' this name. We learnt that CRUX IDs map 1:1 to Blockstack IDs.  

What do we mean by resolve? Resolve means to translate a Blockstack ID to a Public Key. This is very similar to how DNS 'resolves' a easy to read website URL to an IP Address.

In the above section we mitigated threats to ID Registration, but the ID Resolution process is equally crucial to ensure security. One is useless without the other.

##### Threat - Malicious Resolver

The Resolver is an entity which tells us the Public Key corresponding to a Blockstack ID. Asking the resolver for a name is a simple HTTP API call. How can we expect Users to trust our resolver? This is a key question. Lets see how we mitigate this issue - 
1.  By making sure they resolve any name from multiple independent parties is called the Verifier Pool. The Verifier Pool contains
    * A BNS Node run by Blockstack
    * A BNS Node run by CRUX creators
    * A BNS Node run by the Wallet itself
2. By empowering tech savvy users to run and trust their own BNS Nodes. 

### CRUXPay Public Addresses

##### Threat - Unauthorized Modification of Public Addresses 

##### Threat - Unintended leakage of private address information




### CRUXConnect Communication Messaging

##### Threat - Communicating with malicious/wrong/unintended parties

##### Threat - Unintended leakage of private communication data
