# CRUX Protocol Risk Analysis

## Preface

We recommend going through at least the [Problem](https://github.com/cruxprotocol/handbook#2-motivation--problem) and [Solution](https://github.com/cruxprotocol/handbook#3-solution) section to most effectively understand the contents of this document.
  
CRUX has been designed after extensive assessment of threats to various parts of the solution. That being said, we urge the community to probe deeper and ask more searching questions if need be.

In the next section, we identify the risks associated with each part of the CRUX Protocol. Along with each identified risk, we explain how CRUX's architecture mitigates each risk. We also clearly lay out the areas of potential improvement.  

https://www.threatanalysis.com/2010/05/03/threat-vulnerability-risk-commonly-mixed-up-terms/

A good way to analyze Risks is to break them down into Assets, Threats and Vulnerabilities.
- An asset is what we’re trying to protect.
- A threat is what we’re trying to protect against.
- A vulnerability is a potential weakness or gap in our protection efforts.

Risk is the intersection of assets, threats, and vulnerabilities.

## Risk Assets

### CRUX Identity Private Key  

CRUX protocol works around the concept of a User owned identity called the CRUX ID. This identity is represented by a ECDSA keypair and can be stored securely in any Wallet of the User's choice.

#### Threats & Mitigation
##### Unauthorized access of Private Key at rest

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

##### Memory disclosure attack

In practical implementations of cryptosystems, the private keys are usually loaded into the memory as plaintext, and then used in the cryptographic algorithms. Therefore, the private keys are subject to memory disclosure attacks that read unauthorized data from RAM. Such attacks could be performed through software methods (e.g., Open SSL Heart bleed) even when the integrity of the victim system's executable binaries is maintained. They could also be performed through physical methods (e.g., Cold-boot attacks on RAM chips) even when the system is free of software vulnerabilities.

How do we ensure security in such an environement? 

The first way CRUX mitigates this is by not introducing any new trust entities into the User's workflow. The User can trust the same Wallet she trusts for managing her regular cryptocurrency private keys. 

The second we this is mitigated is by adhering to a strict on-demand decryption policy which ensures the private key is in memory in a decrypted form for the shortest amount of time.

```
await this._payIDClaim.decrypt();
const result = sensitiveOperation()
await this._payIDClaim.encrypt();
```


### CRUX Identity Registration 

Users register a human readable name  

#### Threats


### CRUX Name Resolution

### CRUXPay Public Addresses

### CRUXConnect Communication Messaging
