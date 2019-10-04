# CRUX Protocol Background Study

## 1 Introduction

Earlier we analyzed the core [Problem](https://github.com/cruxprotocol/handbook#2-motivation--problem) and learnt what the [Solution](https://github.com/cruxprotocol/handbook#3-solution) would be like. We will now dive into a background study of the relevant protocols and tools in the crypto ecosystem relevant to our problem space. This will include other attempts to solve the same problem, attempts to solve different problem, but from whom we can learn nonetheless.
This document should give the reader a comfortable insight into the thought process that went into the [Architecture](https://github.com/cruxprotocol/handbook#4-architecture-overview) of CRUX. 

We end the document with a short summary of our design choices as described in the [Architecture Section](https://github.com/cruxprotocol/handbook#4-architecture-overview).  


## 2 Research

### 2.1 Crypto Payments to human readable names


https://medium.com/tokendaily/handshake-ens-and-decentralized-naming-services-explained-2e69a1ca1313
https://blog.goodaudience.com/crypto-alias-comparison-opencap-vs-openalias-3e94f9daef94?gi=e3e96f2be689

We have the following requirements:

**REQ.A1:** User Experience must be simple and fast. 

**REQ.A2:** User A must be able to register a globally unique human readable name.

**REQ.A3:** The name must be strongly owned i.e. the User and only the User should be able to make changes to the name.

**REQ.A4:**  We must be able to store some arbitrary data against the human readable name 

**REQ.A5:**  Must have a good mechanism of resolve-time verification of data.  

**REQ.A6:**  Project must have a lively developer community.  

#### 2.1.1 OpenAlias

OpenAlias helps us register and resolve names by using the same name resolution method used on the Web - DNS. 

Arbitrary data can be stored in TXT records of any conventional web domain. The domain itself becomes the easy-to-read payment identifier.

##### The Good

- Architecture is very easy to understand. 
- DNS is baked into operating system - guaranteed platform support.
- DNSSEC support for resolve-time verification.

##### The Bad

- It is not practical to expect each user to own a domain. The whole point is to make an experience for everyone, not just tech savvy users with domains.
- If we assume delegated ownership of domains (say, wallets owning subdomains on users' behalf), users will not end up actually owning their identities. The domain owner can make any changes he likes.
- We all hope DNSSEC becomes ubiquitous some day, because of the new applications it will enable. But the truth is that today, DNSSEC is all cost, no benefit, and with high risks. We still don't get strong security, the client is still expected to know a trusted root public key to validate the trust chain. 
 


#### 2.1.2 Namecoin

Namecoin implements a decentralized naming service on top of the Bitcoin blockchain.


##### The Good
- Strong ownership - ownership of name is proved by Private Key. 
- Creating keypair is easy on any device. It can be done transparently.
- Ownership is secured by the hashpower of the most trusted blockchain network - Bitcoin 

##### The Bad
- Namecoin’s merged mining with Bitcoin regularly placed it under the de facto control of a single miner. 



#### 2.1.3 GNS - GNU Naming Service

???

#### 2.1.4 OpenCAP

The OpenCAP protocol at the end of the day is a REST API specification. There are two tiers to the API, "Address Query" and "Alias Management". An OpenCAP server MUST support the "Address Query" endpoints and protocol.

1. We start with an OpenCAP ID eg. `foo$bar.com`.
2. SRV record on `bar.com` points to location where REST API Server is running
3. A GET request is made to the server `GET /v1/addresses?alias={alias}&address_type={address_type}`



##### The Good
- Minimal implementation, easy to understand. 
- Doesn't try to solve too many problems at once. 


##### The Bad
- Brings DNS and Domain ownership into the picture again. We don't believe it's a practical UX if we expect our Users to make and manage their own domains.
- It's just an API Spec. OpenCAP provides a useful starting point for solving our problem, but doesn't bring too much more to the table. 

#### 2.1.5 Handshake


##### The Good
- Big vision, good development, good documentation, decent community 
- Claims the strongest resolve-time security guarantees

##### The Bad
- Main purpose is to get rid of root TLD
- Very slow registrations - not practical
- Testnet


#### 2.1.6 ENS - Ethereum Naming Service

##### The Good
- Ethereum ecosystem 
- Fast and cheap subdomain registrations
- Strong ownership with private key 

##### The Bad
- Ethereum not as secure at Bitcoin
- Storing arbitrary data is possible, but changing it incurs cost every time.


#### 2.1.6 Blockstack


##### The Good
- Ecosystem, community
- Good abstraction of secure storage - Gaia
- Bitcoin Blockchain
- BNS Network - makes querying fast

##### The Bad
- Name resolution requires additional measures to meet standards of security.
- Verification of name resolution from Bitcoin blockchain requires lots o


### 2.2 Ecosystem Interoperability


## 3 Learnings
[upcoming]
