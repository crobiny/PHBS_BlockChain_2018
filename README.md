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
![](coinjoin.png)
*Fig 1. Merge two user transactions into one Coinjoin transaction*

In this transaction, 0.05BTC are sent out using **Coinjoin**. In order to track the source of the funds, you only need to add up the amount on the right and match the amount on the left.

If we reconstitute the transaction, we will find that：
* 0.05 + 0.0499 + 0.0001(fee) = 0.10BTC
* 0.0499 + 0.05940182 + 0.0001(fee) = 0.10940182BTC

As more users join the **Coinjoin** process, the difficulty of getting results will increase exponentially. But it is still possible to trace back all transactions.


#### 3.1.2 Improved Coinjoin: Darksend

Darksend
* Merge multiple transactions into one transaction
* Use the same face value of 0.1DASH, 1DASH, 10DASH and 100DASH
* Involve at least 3 transactions
![](darksend.png)

*Fig 2. When 3 users'funds are merged into one common transaction, users will export funds in a new disrupted form.*

In order to deal with Denial-of service(DOS) attacks. Darksend stipulates that the user submits the transaction in the form of a deposit to the mine, so the user will provide the miners with high remuneration. That is to say, a deposit is required at the beginning of a user's request to the mixing pool. If the user does not cooperate at some time, such as refusing to sign, the deposit transaction will automatically broadcast over the whole network. The cost of persistent attacks on anonymous networks is extremely high.

The mixing limit of Darksend is 1000 DASH per round, and multiple rounds of mixing can mix a considerable amount of money anonymously. Each round of the Draksend process can be considered as an independent event to enhance the anonymity of user funds.

| Depth of the Chain(r)|Possible users(<a href="https://www.codecogs.com/eqnedit.php?latex=n^{r}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?n^{r}" title="n^{r}" /></a>, n=3)|
|:------:|:------:|
|1   | 3   |
|3 |  27  |
|5  |  243   |
|7  |  2187   |

*Table 1. The number of users who may participate in r mixing sessions*

Through Darksend's multi-round mixing technology, the probability of tracking a single transaction decreases exponentially with the increase of the number of rounds. Besides, 2 tools also helps to enhance anonymity。

* **Chaining**：One transaction of a user will randomly chooses multiple Masternodes, and then mixes in these Masternodes in turn, and finally delivers one final output.

* **Blinding**：Instead of sending the input and output addresses directly to the mixing pool, the user randomly selects a Masternode to transfer the input and output to a designated Masternode, which makes it difficult for the latter Masternode to obtain the real identity of the user.

In this way, unless the attacker controls most Masternodes, it is almost impossible to trace back a specified transaction.


**Drawback**

In Dash Coin, there is still the risk that the master node will be controlled and the malicious users will participate in the currency mixing, which will lead to the leakage of user privacy to a certain extent.

### 3.2 Monero

Monero Coin proposes a hybrid encryption scheme independent of the central node. There are 3 key technologies of Monero Coin: **stealth addresses** , **ring signature** and **ringCT**

#### 3.2.1 Stealth addresses

**Stealth addresses** is to solve the problem of the relevance of the output address of the input. Each time a sender wants to initiate a transaction, he first calculates a one-time temporary intermediate address by using the public key information of the receiver, and then sends the amount to the intermediate address. The receiver then finds the transaction by using his own public and private key information, and then carries on the expense. In this way, other users on the network, including miners, can not determine who the intermediate address belongs to, but the validity of the transaction can still be verified, and since the address is one-time, each time it is randomly generated, the attacker can not make any association with the real sender and receiver.

To make the whole process clearer, here is an example as following. Suppose Alice wants to transfer money to Bob

1. Bob chooses 2 numbers *a* and *b* as his private key. According to the ECC curve, Bob calculates the corresponding public key *A=aG* and *B=bG*. G is a common base point on the curve. Bob broadcasts these 2 public keys *A* and *B* on the whole network.
1. Alice chooses a random integer *r* as another private key, and calculates <a href="https://www.codecogs.com/eqnedit.php?latex=P=H_{s}(rA)G&plus;b" target="_blank"><img src="https://latex.codecogs.com/gif.latex?P=H_{s}(rA)G&plus;b" title="P=H_{s}(rA)G+b" /></a>
1.



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



## 4.Comparison

## 5.Expectation
