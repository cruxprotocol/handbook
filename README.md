
# CRUX Protocol


## 1 Abstract
CRUX is the User Experience Layer between Users and the world of cryptocurrencies & dApps.
CRUX is designed to drastically lower the barrier of entry for users into the blockchain ecosystem.

[diagram]

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

3. **Blockchain**  
Transmit value securely to another entity.  
Securely execute business logic.  

4. **Services & Applications**  
Any use case which involve interactions with the Blockchain.   
Broad classification can be understood as - dApps, exchanges, merchants, gaming etc.

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


## 3 Solution

This identity is used in two distinct protocols - the CRUXPay Protocol & the CRUXGateway Protocol.
CRUXPay tackles the problem of easy human readable identifiers for cryptocurrency addresses.
CRUXConnect aims to bridge the gap between Wallets & Applications with a secure end-to-end encrypted communication channel and establishes a language for Wallets & Applications to understand each other.
   


### 3.1 Universal Identity - Crux ID

Our solution to the above outlined problems is centered around the concept of a Universal Identity owned by the User - called a CRUX ID.
The CRUX ID is represented by a globally unique human readable name such as `emily@crux`. This human readable name is tied to a Private Key stored securely in any Wallet of the user's choice.

It's a way for Users to represent cryptocurrency addresses of their choice with an easy to remember human readable name.

A CRUX ID is required to be 
- Globally unique - Every User can have 1 or more unique CRUX ID
- Human readable - IDs must be easy to read and communicate
- Strongly owned - The proof of identity must reside securely only in any secure Wallet of User's choice 


### 3.2 P2P Crypto Payments for Humans - CRUXPay Protocol

With CRUX IDs we have a universal verifiable identity owned by Users in their Wallets.

CRUXPay Protocol is a layer which allows Users to bind cryptocurrency addresses of their choice - to this identity.

Matt should be able to pay to Emily by simply entering her CRUX ID `emily@crux` in his Wallet. Matt may or may not have a CRUX ID himself.
Matt should not be exposed to any other blockchain implementation details. The only things relevant to him are - 
- Emily's CRUX ID
- The cryptocurrency he wants to pay in
- How much he wants to pay

This needs to happen with Emily's privacy in mind. Only addresses that she consents to must be made public.
The CRUX ID to Address mapping powering this experience should be securely verifiable by Matt's Wallet. Matt must be guaranteed he is paying to the right address. 

The addresses which Emily explicitly consents to being made public against her CRUX ID are referred to as 'Public Addresses'




### 3.3 Ecosystem Interoperability - CRUXConnect Protocol
[diagram]

With CRUX IDs we have a universal verifiable identity owned by Users in their Wallets. 

CRUXConnect protocol establishes the following:

Firstly, an on-demand end-to-end encrypted communication channel between User's Wallet and any Application (decentralized or conventional)  
Secondly, A common language for Applications to 'talk' to Wallets. Applications want to interact with the Blockchain in a variety of ways **with the User's explicit consent**:
- Applications should be able to request payments from the User.
- Applications should be able to schedule recurring payment requests
- Applications should be able to access a smart contract's interface using a transaction
- Applications should be able to simply validate the identity of its User, using CRUX ID, similarly to how "Login With Google" works. CRUX should enable Applications to 'Login With CRUX'.


## 4 Architecture
### 4.1 CRUX ID

CRUX IDs are powered by the Blockstack Naming Service.

![Problem2](https://s3-ap-southeast-1.amazonaws.com/files.coinswitch.co/cruxpay/handbook_images/solution1.png)

![Problem2](https://s3-ap-southeast-1.amazonaws.com/files.coinswitch.co/cruxpay/handbook_images/solution1b.png)

BNS has two jobs -  

1. Reserving a Name for a PublicKey - AKA "Registration of CRUX ID"  
BNS represents ownership of each identity with a standard ECDSA keypair, same as what the Bitcoin network uses to represent ownership of Bitcoin.
Bitcoin gives us a way to establish global consensus on an append-only log in a network of trustless nodes. In the Bitcoin network, the network of nodes are incentivized to establish a global view of the append only log.   
BNS binds a KeyPair to a Name by storing it in this append only log. The log is secured by the same hash power and cryptography that secures value and ownership of money in the Bitcoin Network. 

2. Resolving a Name to a PublicKey - AKA "CRUX ID as Public Key Infrastructure"
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

Now that we have a secure way to bind the identity of a User, we need to store the user's chosen 'public addresses' securely.   
It is not feasible to store arbitrary amounts of data in the Bitcoin blockchain because of monetary and time cost. Blockstack introduces a layer of indirection here with the help of the Blockstack Network and data storage entities called ‘Gaia Hubs’. Gaia works by hosting data in one or more existing storage systems of the user's choice called a ‘Gaia Hub’. These storage systems are typically cloud storage systems. User gets to choose where their data lives, and Gaia enables applications to access it via a uniform API. Blockstack applications use the Gaia storage system to store data on behalf of a user. The Gaia hub authenticates writes to a location by requiring a valid authentication token, generated by a private key authorized to write at that location.

We can securely validate the Authenticity and Integrity of any data with the help of the User's CRUX ID. The CRUX ID can provide the User's Public Key. Once we have the Public Key, we can easily check for data Integrity and Authenticity using HMAC. 

So Three layers work together -  
1. Blockchain - The Bitcoin blockchain stores the hash of a ‘ZoneFile’
2. BNS Nodes - Hash of ZoneFile is mapped to the owner Public Key and the Zonefile itself.
3. Gaia Hub - ZoneFile contains URL to the User's Gaia Hub of choice.

An individual Gaia Hub may not be decentralized, and it does not need to be. CRUX provides all the necessary privacy and security guarantees by assuring the following -  

1. User owns keypair securely in the Wallet. No one else ever gets to see the private key.
2. The Wallet or the User can choose to use their own Gaia Hubs. There is no dependency on CRUX or the creators of CRUX.
3. Anyone trying to read data from a Gaia Hub can verify Authenticity and Integrity using a trusted PKI. The BNS Network acts as our PKI.


#### 4.2.2 Standardizing Crypto Payments
![Problem2](https://s3-ap-southeast-1.amazonaws.com/files.coinswitch.co/cruxpay/handbook_images/mappings.png)

There is lack of standardization in the Crypto industry, so much so that there is still no well established a consensus even on how to encode information in QR Codes.

We need to make the Payment process fool-proof. The vision is that no User should be confused about currency symbols, hard forks, soft forks, address formats, decimal precision, etc when making payments.
1. CRUXPay establishes a common format for representing addresses  `{addressHash, secondaryIdentifier}`
2. Establish a reference 'Global Asset List' with each currency represented as an 'Asset' each with its own unique 'Asset Idenfier'. The Global Asset List is expressed in as unopiniated and unambiguous terms as possible. 
3. Wallets implementing CRUXPay can map their currency identifiers against specific Assets in the Global Asset List.

With such a standardized one-time setup, diverse Wallets across the ecosystem can seamless pay to each other's Users. 

The Global Asset List also standardizes decimal precision so that currency amounts can be communicated across members of the ecosystem in an unambiguous manner.

### 4.3 CRUXConnect Protocol


## Appendices
### I. Background Study
### II. Risk Analysis
### III. L3 UX Protocols
### IV. Roadmap
