# CRUX Protocol

### Contents

  * [1 What is CRUX?](#1-what-is-crux)
  * [2 Motivation &amp; Problem](#2-motivation--problem)
     * [2.1 Stakeholders](#21-stakeholders)
     * [2.2 Analysis](#22-analysis)
        * [2.2.1 Scenario 1 - P2P Interactions](#221-scenario-1---p2p-interactions)
        * [2.2.2 Scenario 2 - Interactions with Applications](#222-scenario-2---interactions-with-applications)
  * [3 Solution](#3-solution)
     * [3.1 Universal Identity - CRUX ID](#31-universal-identity---crux-id)
     * [3.2 P2P Crypto Payments for Humans - CRUXPay Protocol](#32-p2p-crypto-payments-for-humans---cruxpay-protocol)
     * [3.3 Ecosystem Interoperability - CRUXGateway Protocol](#33-ecosystem-interoperability---cruxgateway-protocol)
  * [4 Architecture Overview](#4-architecture-overview)
     * [4.1 CRUX ID](#41-crux-id)
        * [4.1.1 Reserving a Name for a PublicKey - AKA "Registration of CRUX ID"](#411-reserving-a-name-for-a-publickey---aka-registration-of-crux-id)
        * [4.1.2 Resolving a Name to a PublicKey - AKA "CRUX ID as Public Key Infrastructure"](#412-resolving-a-name-to-a-publickey---aka-crux-id-as-public-key-infrastructure)
     * [4.2 CRUXPay Protocol](#42-cruxpay-protocol)
        * [4.2.1 Secure Storage of Address Mapping](#421-secure-storage-of-address-mapping)
        * [4.2.2 Standardizing Crypto Payments](#422-standardizing-crypto-payments)
     * [4.3 CRUXGateway Protocol](#43-cruxgateway-protocol)
  * [5 Appendices](#5-appendices)
     * [5.1 <a href="https://github.com/cruxprotocol/handbook/blob/master/background.md">Background Study</a>](#51-background-study)
     * [5.2 <a href="https://github.com/cruxprotocol/handbook/blob/master/riskanalysis.md">Risk Analysis</a>](#52-risk-analysis)


## 1 What is CRUX?
We propose CRUX - A User Experience Layer between Users and the world of cryptocurrencies & dApps.  
  
CRUX makes cryptocurrency payments easier by providing users with easy to remember identifiers such as `emily@crux`.  
CRUX allows any dApp to talk to any Wallet securely with the User's consent. No QR codes, no browser extensions, no special apps. 

CRUX is designed to drastically lower the barrier of entry for new users into the blockchain ecosystem.

CRUX achieves this with 3 components:  
1. CRUX Identity - A globally unique, human-readable secure identity stored in a User's wallet.  
2. CRUXPay Protocol - Pay to a human readable name. Unified experience across cryptocurrencies.   
3. CRUXGateway Protocol - Seamless interoperability between Wallets, dApps and Conventional Services.   



## 2 Motivation & Problem

### 2.1 Stakeholders
![Stakeholders](https://s3-ap-southeast-1.amazonaws.com/files.coinswitch.co/cruxpay/handbook_images/actors.png)

1. **Users**    
Users who want to send cryptocurrencies to others  
Users who want to use a dApp  
Users who want to use an exchange or a merchant

2. **Wallets**  
Users trust Wallets with their Private Keys.   
Wallets are the gateway to the Blockchain.

3. **Services & Applications**  
Any use case which involve interactions with the Blockchain.   
Broad classification can be understood as - dApps, exchanges, merchants, gaming etc.

4. **Blockchain**  
Transmit value securely to another entity.  
Securely execute business logic.  

### 2.2 Analysis

Now that we've established the stakeholder vocabulary, we'll try to break down how these stakeholders interact with each other.
We'll broadly classify our problem space into two categories - 
1. User to User (P2P) Interactions
2. Interactions with Services & Applications

We'll try to stick very close to the user and try to see each interaction from the user's perspective.



#### 2.2.1 Scenario 1 - P2P Interactions

P2P interactions involving Blockchain are almost wholly composed of User to User payments of cryptocurrencies.   

![Problem1](https://s3-ap-southeast-1.amazonaws.com/files.coinswitch.co/cruxpay/handbook_images/problem_1.png)

When A wants to send money to B, a public address is required as the identifier of B. 
On most blockchains it is a long string which is not practical to memorize, recognize, or communicate. This is a big mental shift from other identifiers Users are used to, such as email addresses, twitter handles and phone numbers.  
This itself becomes a big barrier for entry into the cryptocurrency ecosystem.

Secondly, a several technical implementation details of the underlying Blockchain technology regularly leak out to the Users, such as -  
- A single Blockchain may have several different Address formats. 
- A users Wallet may or may not support all address formats.
- Hard forks cause confusion.
- Lack of symbol standardization
- Decimal precision related issues 
- Confusion about Secondary address identifiers (eg. destination tags in XRP)
- Confusion about token contract addresses & malicious actors taking advantage of the situation

We need to insulate the User from these implementation details if we want mass adoption. CRUX is the layer which aims to push these responsibilities of consistency, and reliability onto the CRUX protocol.

As members of the Blockchain ecosystem we need to make sure we are communicating to our Users in a language that they understand.

As a young fast growing ecosystem, there are many different use cases that many talented teams across the world are trying to solve in their own way. As a result there are many Blockchains and many cryptocurrencies many with their own unique architectures, strengths and weaknesses. We believe this will continue to be the case as the industry finds its feet.  
This diversity exacerbates the cognitive load a User needs to go through when introduced to new cryptocurrencies.
#### 2.2.2 Scenario 2 - Interactions with Applications

Lets try to understand the user experience when a User tries to use Services & Applications in the Blockchain ecosystem. 

![Problem2](https://s3-ap-southeast-1.amazonaws.com/files.coinswitch.co/cruxpay/handbook_images/problem2.png)


A large chunk of a User's interaction with Wallets happens in context of an **Application**. Such as 
- A User trying to exchange cryptocurrencies
- A User playing a game dApp
- A User buying goods from a marketplace with cryptocurrencies

These Applications need to interact with the Blockchain via the User's Wallet. The problem is, how does the Application 'talk' to the Wallet? 

- The most basic way of doing this is manually. Which means, offloading the burden onto the User. For example, while buy a TShirt, I may have to manually enter the exact amount & address I need to pay in my wallet. Payment gateways may not accept payments if the amount is not exact. 
- Scanning a QR Code - this is possible only if the Wallet device has a camera *and* if the wallet and application are on 2 different devices. This prevents a fluid experience for Users since their options are always limited.
- More sophisticated interactions which require smart contract function calls require a special Wallet as a browser extension to bridge the gap. Or a specific App. This introduces more complications in the User flow. This forces Users to store cryptocurrencies in several different wallets on different platforms depending on the application. eg MetaMask, TronLink.   

Today's Blockchain applications are forced to impose requirements on Wallet platforms due to this communication gap. Managing wallets securely itself is a big mental shift from the modern world, now our Users are forced to maintain multiple Wallets. We envision a world where any Application can work with any Wallet.

## 3 Solution

We propose a solution centered around a user owned identity called CRUX ID. This identity is used in two distinct protocols - the CRUXPay Protocol & the CRUXGateway Protocol.
CRUXPay tackles the problem of easy human readable identifiers for cryptocurrency addresses.
CRUXGateway aims to bridge the gap between Wallets & Applications with a secure end-to-end encrypted communication channel and establishes a language for Wallets & Applications to understand each other.
   


### 3.1 Universal Identity - CRUX ID

Our solution to the above outlined problems is centered around the concept of a Universal Identity owned by the User - called a CRUX ID.
The CRUX ID is represented by a globally unique human readable name such as `emily@crux`. This human readable name is tied to a Private Key stored securely in any Wallet of the user's choice.

It's a way for Users to represent cryptocurrency addresses of their choice with an easy to remember human readable name.

A CRUX ID is required to be 
- Globally unique - Every User can have 1 or more unique CRUX ID
- Human readable - IDs must be easy to read and communicate
- Strongly owned - The proof of identity must reside securely only in any secure Wallet of User's choice 


CRUX IDs simply establish the ownership of a private key to a human readable name. This ownership must be held to the highest level of security standards and must be cryptographically verifiable by anybody.

CRUX IDs are independent - they do not depend on its creators. CRUX does not introduce any specific platform requirement. Users can choose any CRUX compatible Wallet, and any Wallet is free to implement support for CRUX IDs.
 

### 3.2 P2P Crypto Payments for Humans - CRUXPay Protocol

With CRUX IDs we have a universal verifiable identity owned by Users in their Wallets.

CRUXPay Protocol is a layer which allows Users to bind cryptocurrency addresses of their choice - to this identity.

Lets say Matt is trying to send cryptocurrencies to Emily. With CRUXPay the scenario looks like this -   
Matt should be able to pay to Emily by simply entering her CRUX ID `emily@crux` in his Wallet. Matt may or may not have a CRUX ID himself.
Matt should not be exposed to any other blockchain implementation details. The only things relevant to him are - 
- Emily's CRUX ID
- The cryptocurrency he wants to pay in
- How much he wants to pay

Matt must have the same experience across all cryptocurrencies. He should not be at risk of incorrect payments due to ongoing forks or migrations of the underlying technology powering the cryptocurrency.  

This needs to happen with Emily's privacy in mind. Only addresses that she consents to must be made public. The addresses which Emily explicitly consents to being made public against her CRUX ID are referred to as 'Public Addresses'

The CRUX ID to Address mapping powering this experience should be securely verifiable by Matt's Wallet. Matt must be guaranteed he is paying to the right address. 

### 3.3 Ecosystem Interoperability - CRUXGateway Protocol

With CRUX IDs we have a universal verifiable identity owned by Users in their Wallets. 

CRUXGateway protocol establishes the following:

Firstly, an on-demand end-to-end encrypted communication channel between User's Wallet and any Application (decentralized or conventional)  
Secondly, A common language for Applications to 'talk' to Wallets. Todays Applications want to interact with the Blockchain in a variety of ways. 
- Applications should be able to request payments from the User.
- Applications should be able to schedule recurring payment requests
- Applications should be able to access a smart contract's interface using a transaction
- Applications should be able to simply validate the identity of its User, using CRUX ID, similarly to how "Login With Google" works. CRUX should enable Applications to 'Login With CRUX'.

CRUXGateway lets any application securely 'talk' to any Wallet supporting CRUX. That means, Applications can support interactions with multiple cryptocurrencies through a uniform API. They will automatically support any old or new Wallets that support CRUX. Anyone is free to integrate CRUX. 


## 4 Architecture Overview

### 4.1 CRUX ID

CRUX IDs are powered by the Blockstack Naming Service. Each CRUX ID has a 1:1 mapping to a Blockstack ID. The SDK defines rules of converting between CRUX IDs and Blockstack IDs. 

BNS has two jobs -  

##### 4.1.1 Reserving a Name for a PublicKey - AKA "Registration of CRUX ID"
![Problem2](https://s3-ap-southeast-1.amazonaws.com/files.coinswitch.co/cruxpay/handbook_images/solution1.png)
  
BNS represents ownership of each identity with a standard ECDSA keypair, same as what the Bitcoin network uses to represent ownership of Bitcoin.
Bitcoin gives us a way to establish global consensus on an append-only log in a network of trustless nodes. In the Bitcoin network, the network of nodes are incentivized to establish a global view of the append only log.   
BNS binds a KeyPair to a Name by storing it in this append only log. The log is secured by the same hash power and cryptography that secures value and ownership of money in the Bitcoin Network. 

##### 4.1.2 Resolving a Name to a PublicKey - AKA "CRUX ID as Public Key Infrastructure"
![Problem2](https://s3-ap-southeast-1.amazonaws.com/files.coinswitch.co/cruxpay/handbook_images/solution1b.png)
Now that the Name->PublicKey mapping is stored securely in the Bitcoin blockchain, we can ‘resolve’ the easy to remember name to a public key, similar to how DNS helps resolve an easy to remember domain name to an IP address.  
If we rely solely on the Bitcoin Blockchain for this purpose, each lookup would be very slow since the entire Bitcoin Blockchain would need to be parsed to determine the result.  
BlockStack helps to offload these lookups to entities known as BNS Nodes which make up the BNS Network. Each BNS Node keeps continuously monitoring its independent view of the Bitcoin blockchain and indexes any new name registrations or modifications to old ones. Applications can then simply ask a trusted BNS Node for a quick answer.
### 4.2 CRUXPay Protocol

There are two major parts to CRUXPay - 
1) Securely storing the customer's public addresses
2) Standardizing Crypto Payments 

#### 4.2.1 Secure Storage of Address Mapping
![Problem2](https://s3-ap-southeast-1.amazonaws.com/files.coinswitch.co/cruxpay/handbook_images/solution2.png)

![Problem2](https://s3-ap-southeast-1.amazonaws.com/files.coinswitch.co/cruxpay/handbook_images/solution2b.png)

With CRUX IDs we have a secure way to bind the identity of a User. Now we need to store the user's chosen 'public addresses' securely.   
It is not feasible to store arbitrary amounts of data in the Bitcoin blockchain because of monetary and time cost. Blockstack introduces a layer of indirection here with the help of the Blockstack Network and data storage entities called ‘Gaia Hubs’. Gaia works by hosting data in one or more existing storage systems of the user's choice called a ‘Gaia Hub’. These storage systems are typically cloud storage systems. User gets to choose where their data lives, and Gaia enables applications to access it via a uniform API. Blockstack applications use the Gaia storage system to store data on behalf of a user. The Gaia hub authenticates writes to a location by requiring a valid authentication token, generated by a private key authorized to write at that location.

We can securely validate the Authenticity and Integrity of any data with the help of the User's CRUX ID. The CRUX ID can provide the User's Public Key. Once we have the Public Key, we can easily check for data Integrity and Authenticity using HMAC. 

So Three layers work together -  
1. Blockchain - The Bitcoin blockchain stores the hash of a ‘ZoneFile’
2. BNS Nodes - Hash of ZoneFile is mapped to the owner Public Key and the Zonefile itself.
3. Gaia Hub - ZoneFile contains URL to the User's Gaia Hub of choice.

An individual Gaia Hub may or may not be decentralized, and it does not need to be. CRUX provides all the necessary privacy and security guarantees by assuring the following -  

1. User owns keypair securely in the Wallet. No one else ever gets to see the private key.
2. The Wallet or the User can choose to use their own Gaia Hubs. There is no dependency on CRUX or the creators of CRUX.
3. Anyone trying to read data from a Gaia Hub can verify Authenticity and Integrity of a file's using a trusted PKI. The BNS Network acts as our PKI. This means any data being written must contain an HMAC.


#### 4.2.2 Standardizing Crypto Payments


The cryptocurrency industry is at a nascent stage, with tens of thousands of blockchain projects that use diverse platforms with multiple coding languages, protocols, consensus mechanisms, and privacy measures. Enhancing standards and interoperability is key to unlocking the next phase of growth that lies ahead of us. 

We consider payment and currency interoperability to be a big part of the User experience of cryptocurrencies which requires standardization efforts.

 
We need to make the Payment process fool-proof. The vision is that no User should be confused about currency symbols, hard forks, soft forks, address formats, decimal precision, etc when making payments.

![Problem2](https://s3-ap-southeast-1.amazonaws.com/files.coinswitch.co/cruxpay/handbook_images/mappings.png)


##### Global Asset List & Client Asset Mapping

We establish a reference 'Global Asset List' with each currency represented as an 'Asset' each with its own unique 'Asset Identifier'. The Global Asset List is expressed in as unopinionated and unambiguous terms as possible. 

Clients may refer to each asset in their own way. Some might call it `bitcoin`, some `btc`, some `BTC`. At the time of onboarding, Wallets need to explicitly map CRUX Assets to identifiers of their choice. ≠

An Asset is represented by a UUID - known as an `AssetID`. Each Asset contains several fields which aim to express information about the Asset in as unambigious terms as possible.

**An Asset never changes**. The Global Asset is an append only log. For example: 
- When a blockchain has a hard fork, there will be two new Assets created. The older one does not change. Wallets aware of the hard fork can explicitly map the newly created assets to the corresponding so that Users are never exposed to unintended side effects of the hard fork. 
- When a cryptocurrency migrates from a Blockchain to another, again, lead to a new Asset in the Global Asset List. Whenever Wallets decide to support the migrated cryptocurrency, they can update their Client Asset Mapping.

Example:
```
{
    "assetId":"73b1618a-d61f-4dd4-87c3-853a967d4490",
    "symbol":"XYZ",
    "name":"XYZ Token",
    "assetType":"ERC20",
    "decimals":18,
    "assetIdentifierName":"Contract Address",
    "assetIdentifierValue":"0xB97048628DB6B6........833e95Dbe1A905B280",
    "parentAssetId":"4e4d9982-3469-421b-ab60-2c0c2f05386a"
}
```



### 4.3 CRUXGateway Protocol

A securely stored identity in a User's Wallet allows anyone to easily establish a secure end-to-end encrypted communication channel with the Wallet.   
This can be achieved because anyone can now resolve a human readable CRUX ID to the Public Key of the ID owner. Elliptic-curve Diffie–Hellman (ECDH) helps the Application and the Wallet both derive the same shared secret.

This means, independent of the transport layer used, at CRUX's level we can implement secure communication between Application and Wallet.
We use WebRTC as the transport layer for CRUXGateway. WebRTC allows two peers to exchange data without intermediaries. The peers need the assistance of an entity generalized as the 'Bridge Server' which helps the peers discover each other on the network. Once discovered, the two peers can independantly speakto each other without need the Bridge Server, until network conditions change such that they can no longer find each other. 

![Cconect](https://s3-ap-southeast-1.amazonaws.com/files.coinswitch.co/cruxpay/handbook_images/CRUXGateway.png)

Users can whitelist individual Applications in their Wallet to whom they want to grant communication rights to. 

Applications can connect to User's CRUX ID residing in the User's Wallet and communicate with the wallet in three ways - 
1. RawTransactionRequest - Applications can request the user to sign and send any raw Transaction - like [Web3 standard](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3ethsendrawtransaction)
2. PaymentRequest - For financial transactions, Applications can request a specific standardized currency, and a specific amount. The User will get a Payment Request notification on their Website.
3. IdentityProofRequest - The Application can ask the User to prove the ownership of the identity with this. This can be used to authenticate users securely for any use case.

CRUXGateway can be consumed as a Web3Provider, which means any dApp using web3 and web3-like standards can integrate CRUX as a web3 provider with a few lines of code change. That means any existing Ethereum, TRON, or EOS dApp will be able to connect to any Wallet which supports CRUX protocol, and make the desired smart contract or currency transfer transactions.


## 5 Appendices
### 5.1 [Background Study](https://github.com/cruxprotocol/handbook/blob/master/background.md)
### 5.2 [Risk Analysis](https://github.com/cruxprotocol/handbook/blob/master/riskanalysis.md)
