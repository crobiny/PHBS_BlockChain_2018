# PHBS_BlockChain_2018


## 1.Introduction

Block chain is a distributed bookkeeping technology in P2P network. It is a supporting technology behind many cryptographic currencies such as BTC and ETC. Because the books in block chain can only be written by additions and shared by all nodes in the network, even the nodes that do not trust each other can easily verify the data and rely on a certain consensus mechanism to achieve consistency. Block chain technology can significantly reduce the cost of trust among multiple nodes, so it has a wide range of application scenarios and value in cross-border payment, voucher business, supply chain and some financial fields.

Because different nodes need to compute and verify the same data, data on block chains are required to be public. This increases data transparency and credibility, but also brings another problem-data privacy. In block chains, some nodes may not want their own transaction data to be public, including the identity of both sides of the transaction, transaction funds. Quantity, contract content and so on. This is not only important for customers who pay attention to personal privacy, but also for financial system and supply chain system, data privacy is one of the key points of their profits. Therefore, in order to make the block chain have a wider use space, we need to take into account the multi-node trust characteristics of the block chain and ensure data privacy.

In response to this demand, a lot of research work has been proposed in recent years: in the field of cryptography currency, Dash, Monero, ZCash and so on, they can solve the privacy problem of transaction data to a certain extent. The principles of privacy protection in the above studies are different. Some studies use a number of technologies to protect privacy, and the scenarios of block chains are different. We plan to summarize the privacy protection technologies of block chains.

## 2.Privacy Problem of Block Chain

Bitcoin trading does not require the user's real identity, but block-chain technology such as Bitcoin is not an **anonymous** system. Strictly speaking, Bitcoin is an **pseudonym** system-- the so-called pseudonym, that is, an identity unrelated to the real identity we use in the network. 

But anonymity is different from pseudonym. In computer science, anonymity refers to the so-called unrelated pseudonym, that is, from the perspective of an attacker, it is impossible to correlate any two interactions between users and the system. In Bitcoin, because users repeatedly use public key hash values as transaction identifiers, it is obvious that transactions can be correlated. Bitcoin is not anonymous

## 3.Typical Block Chain Privacy Protection Methods

### 3.1 Dash

To achieve anonymity, we need to make currency fully interchangeable. Interchangeability is the property of currency, which decides that all units of currency should be equal. Currency should not have any connection with the transaction records made with that currency, so that all currencies are equal. At the same time, any user ensures that every transaction in public accounts is honest without affecting the privacy of others.

Dash coin uses a key technology called Coinjoin. Simply speaking, coin technology is a technology that mixes multiple transactions of multiple users (at least three) through some master nodes to form a single transaction. In a coin, each user provides an input and output address, and then sends it to the main node for mixing (i.e., arbitrary exchange). Input and output addresses). Trading can only be done with 0.1, 1, 10, 100 Dash Coins, which makes it more difficult for attackers to guess the degree of transaction correlation from the perspective of amount.

#### 3.1.1 A simple way to mix all currencies: Coinjoin

A simple way to mix all currencies is to simply merge all transactions based on current BTC.
![][coinjoin.png]

**Drawback**

In Dash Coin, there is still the risk that the master node will be controlled and the malicious users will participate in the currency mixing, which will lead to the leakage of user privacy to a certain extent.

### 3.2 Monero

Monero Coin proposes a hybrid encryption scheme independent of the central node. There are two key technologies of Monero Coin: **stealth addresses** and **ring signature**.

#### 3.2.1 Stealth addresses

**Stealth addresses** is to solve the problem of the relevance of the output address of the input. Whenever the sender wants to send a sum of money to the recipient, he first calculates a one-time public key by using elliptic curve encryption through the recipient's address (which is regenerated each time). Then the sender sends the public key along with an additional information to the block chain, and the recipient can root it. Each transaction block is detected by its own private key to determine whether the sender has sent the amount. When the recipient wants to use the amount, it can calculate a signature private key based on its own private key and transaction information, and use this private key to sign the transaction. This private key is used to sign a transaction, because the public key is one-time. Private keys are also disposable

Different from BTC which only has one pair of keys(public key and private key), Menero has two pairs of keys(**spend key** and **view key**)

**Spend key**: In spend key, the public key is used to join ring transaction and verify key image, and the private key is used to create a key image.

**View key**: In view key, the public key is used to create **Stealth addresses**, and the private key is used to scan the whole blockchain and check whether there are some transactions sent to itself.

The sender calculates a temporary one-time stealth address by using the public key of the receiver's view key, and sends the money to the address. Then the receiver scans the block chain and finds that the transaction can take the money away by using the private key of his view key. Others on the network do not know who the transaction is sent to, only the recipient himself. Knowing ensures anonymity of transactions

#### 3.2.2 Ring Signature

Whenever the sender wants to establish a transaction, he will sign the transaction with his own private key and several public keys randomly selected from the public keys of other users. When verifying the signature, he also needs to use other people's public keys and parameters in the signature. At the same time, the sender's signature must also provide key image to provide proof of identity. Both the private key and key image are used. One secret at a time to ensure untraceability

But how to avoid double spend attack?

Use key image generated from private spend key


**Drawback**

Although **stealth addresses** can ensure that the addresses of the recipients change every time, so that the external attackers can not see the address correlation, they can not guarantee the anonymity between the sender and the recipient. Therefore, Monero coin proposes a ring signature scheme.

Ring signatures still need to be mixed with other users'public keys, so they may encounter malicious users to expose their privacy. In addition, in 90% of cases, the size of the ring is between 2 and 4, so anonymity is greatly reduced.

### 3.3 ZCash

#### 3.3.1 Zero Knowledge Proof and zk-SNARK

**Zero Knowledge Proof**：The prover can convince the verifier that an assertion is correct without providing any useful information to the verifier。In the design of ZCash, a technique called zk-SNARK was adopted.

zk-SNARK needs a NP problem as a basic problem. It is not feasible to guess the solution of a NP complete problem (NPC problem, hereinafter referred to as NP problem) with a large input by force, but it is faster to verify that a solution is corresponding to NP problem.

#### 3.3.2 Operating principle of ZCcsh
The system uses zk-SNARK to construct a decentralized mixing pool. Anonymity can be achieved by **mint** and **pour** operations.

**Mint**： The user writes commitment to a list using a certain amount of `ZCash` to exchange the equivalent `commitment`. The commitment must be calculated by a one-time serial number and the user's private key, and it is irreversible.





## 4.Comparison

## 5.Expectation
