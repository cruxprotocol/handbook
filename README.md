
# CRUX Protocol


## Abstract
CRUX is the User Experience Layer between Users and the world of cryptocurrencies & dApps.
CRUX is designed to drastically lower the barrier of entry for users into the blockchain ecosystem.

[diagram]

CRUX achieves this with 3 components:  
1. CRUX Identity - A globally unique, human-readable secure identity stored in a User's wallet.  
2. CRUXPay Protocol - Pay to a human readable name. Unified experience across cryptocurrencies.   
3. CRUXGateway Protocol - Seamless interoperability between Wallets, dApps and Conventional Services.   



## Motivation & Problem

### Stakeholders
[diagram]

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

### Analysis

Now that we've established the stakeholder vocabulary, we'll try to break down how these stakeholders interact with each other.
We'll broadly classify our problem space into two categories - 
1. User to User (P2P) Interactions
2. Interactions with Services & Applications

We'll try to stick very close to the user and try to see each interaction from the user's perspective.

#### Scenario 1 - P2P Interactions

P2P interactions involving Blockchain are almost wholly composed of User to User payments of cryptocurrencies.   

[diagram]

When A wants to send money to B, a public address is required as the identifier of B. 
On most blockchains it is a long string which is not practical to memorize, recognize, or communicate. This is a big mental shift from other identifiers Users are used to, such as email addresses, twitter handles and phone numbers.  
This itself becomes a big barrier for entry into the cryptocurrency ecosystem.

Secondly, a several technical implementation details of the underlying Blockchain technology regularly leak out to the Users since. 
- A single Blockchain may have several different Address formats. 
- A users Wallet may or may not support all address formats.
- Hard forks cause confusion.
- Lack of symbol standardization
- Decimal precision related issues 
- Confusion about Secondary address identifiers (eg. destination tags in XRP)
- Confusion about token contract addresses & malicious actors taking advantage of the situation

We need to insulate the User from these implementation details if we want mass adoption. CRUX is the layer which aims to push these responsibilities of consistency, and reliability onto the CRUX protocol.


As members of the Blockchain ecosystem we need to make sure we are communicating to our Users in a language that they understand.


As a young fast growing ecosystem, there are many different use cases that a multitude of talented teams is trying to solve in their own way. As a result there are many Blockchains and many cryptocurrencies many with their own unique architecture, strengths and weaknesses. We believe this will continue to be the case as the industry finds its feet.  
This diversity exacerbates the cognitive load a User needs to go through when introduced to new cryptocurrencies.

#### Scenario 2 - Interactions with Applications

Lets try to understand the user experience when a User tries to use Services & Applications in the Blockchain ecosystem. 

[diagram]


The primary mental shift Users are expected to undergo has to do with Private Key management. The concept of a Private Key is very powerful yet very dangerous if not handled correctly.  
Which is why this responsibility is delegated to Wallets. The primary responsibility of a Wallet is to keep Private Keys secure.

The Wallet ecosystem has matured to a point that individual Wallets by themselves are able to give a good user experience by abstracting out the security of private keys into common well understood UX patterns. Today's wallets are reasonably easy to explain and understand.

But a large chunk of a User's interaction with Wallets happens in context of an **Application**. Such as 
- A User trying to exchange cryptocurrencies
- A User playing a game dApp
- A User buying goods from a marketplace with cryptocurrencies

These Applications need to interact with the Blockchain via the User's Wallet. The problem is, how does the Application 'talk' to the Wallet? 

- The most basic way of doing this is manually. Which means, offloading the burden onto the User. For example, while buy a TShirt, I may have to manually enter the exact amount & address I need to pay in my wallet. Payment gateways may not accept payments if they're not exact. 
- Scanning a QR Code - this is possible only if the Wallet device has a camera **plus** the wallet and application are on 2 different devices. This prevents a fluid experience for Users since their options are always limited.
- More sophisticated interactions which require smart contract function calls require a special Wallet as a browser extension to bridge the gap. This introduces more complications in the User flow. This forces Users to store cryptocurrencies in several different wallets on different platforms depending on the application.  


## Solution

This identity is used in two distinct protocols - the CRUXPay Protocol & the CRUXGateway Protocol.
CRUXPay tackles the problem of easy human readable identifiers for cryptocurrency addresses.
CRUXConnect aims to bridge the gap between Wallets & Applications with a secure end-to-end encrypted communication channel and establishes a language for Wallets & Applications to understand each other.
   


### I. Universal Identity - Crux ID
[diagram]

Our solution to the above outlined problems is centered around the concept of a Universal Identity owned by the User - called a CRUX ID.
The CRUX ID is represented by a globally unique human readable name such as `emily@crux`. This human readable name is tied to a Private Key stored securely in any Wallet of the user's choice.

It's a way for Users to represent cryptocurrency addresses of their choice with an easy to remember human readable name.

A CRUX ID is required to be 
- Globally unique - Every User can have 1 or more unique CRUX ID
- Human readable - IDs must be easy to read and communicate
- Strongly owned - The proof of identity must reside securely only in any secure Wallet of User's choice 



#### Architecture



### II. P2P Crypto Payments for Humans - CRUXPay Protocol

With CRUX IDs we have a universal verifiable identity owned by Users in their Wallets.

CRUXPay Protocol is a layer which allows Users to bind cryptocurrency addresses of their choice - to this identity.

[diagram]

Matt should be able to pay to Emily by simply entering her CRUX ID `emily@crux` in his Wallet. Matt may or may not have a CRUX ID himself.
Matt should not be exposed to any other blockchain implementation details. The only things relevant to him are - 
- Emily's CRUX ID
- The cryptocurrency he wants to pay in
- How much he wants to pay

This needs to happen with Emily's privacy in mind. Only addresses that she consents to must be made public.
The CRUX ID to Address mapping powering this experience should be securely verifiable by Matt's Wallet. Matt must be guaranteed he is paying to the right address. 

#### Architecture
### III. Ecosystem Interoperability - CRUXConnect Protocol
[diagram]

With CRUX IDs we have a universal verifiable identity owned by Users in their Wallets. 

CRUXConnect protocol establishes the following:

Firstly, an on-demand end-to-end encrypted communication channel between User's Wallet and any Application (decentralized or conventional)  
Secondly, A common language for Applications to 'talk' to Wallets. Applications want to interact with the Blockchain in a variety of ways **with the User's explicit consent**:
- Applications should be able to request payments from the User.
- Applications should be able to schedule recurring payment requests
- Applications should be able to access a smart contract's interface using a transaction
- Applications should be able to simply validate the identity of its User, using CRUX ID, similarly to how "Login With Google" works. CRUX should enable Applications to 'Login With CRUX'.

#### Architecture

## Appendices
### I. Background Study
### II. Risk Analysis
### III. L3 UX Protocols
### IV. Roadmap
