## black-hole-sum
black hole sum is a privacy preserving p2p rating system. This is the repo where it's implementation will live.

The architecture is described [in this medium post](medium link). It is based off of this protocol: [Decentralized Multi-Client Functional Encryption](https://eprint.iacr.org/2017/989.pdf)

The intention is to implement this on Ethereum. 

As far as I can see, the following components need to be built:  

* We will need to do the computations for encryption and decryption. All computations are done using type-3 asymmetric elliptic curve pairings. The pairing chosen must work with two groups where the external Diffie-Hellman assumption holds, as this protocol is secure under the SXDH assumption. The Optimal Ate Pairing over a Barreto-Naehrig curve is one such candidate. [Adjoint has a library in Haskell](https://github.com/adjoint-io/pairing), implementing a BN-128 curve that seems like a good candidate to use. 
* Once cipher texts and decryption keys are generated, they need to actually be sent to other participants. This will be implemented on Ethereum, so we need a library that wraps JSON-RPC. One candidate is [this haskell library for web3](https://hackage.haskell.org/package/web3)
* The architecture requires a multi party computation setup  phase. For this, we will need the ability to encrypt values with ECIES. [This is a library in Haskell for ECIES](http://hackage.haskell.org/package/cryptonite-0.25/docs/Crypto-PubKey-ECIES.html)
* Bulletproofs will be needed, so that inputs can be verified. [Bulletproof library in haskell will be used for this](https://github.com/adjoint-io/bulletproofs)
* We will also need to write a smart contract to coordinate registering people into the rating system, determining who is getting rated, and also for the MPC setup. 

I've decided to use Haskell because I believe that cryptography is best done with pure functional programming. Haskell has a natural fit with cryptographic concepts that I think will prove beneficial. 
