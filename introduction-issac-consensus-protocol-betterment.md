---
layout: post
permalink: /introduction-isaac-consensus-protocol-betterment/
---
---
# Preface

Since the beginning of January 2018, there have been many changes in the world of cryptography and blockchain. There is a worldwide interest in what are Bitcoins and what is Blockchain? 

Despite the steep decline in Bitcoin price, the BOScoin development team has released the ISAAC consensus protocol as open source at [github](https://github.com/bosnet) and released a mobile wallet.
Recently, we are doing our best to develop MainNet on our TestNet network, SEBAK.

At this time we are pleased to release the design of a more advanced version of the ISAAC consensus protocol.

# Why advance the design of our ISAAC consensus protocol? 

Since the successful release of the ISAAC consensus protocol, the BOScoin development team has been working hard to improve ISAAC.

We have been researching projects such as EOS, ONTOLOGY, TEZOS, and COSMOS, and have been thinking about improving the ISAAC consensus protocol.
We do not doubt the success of the mentioned projects, but they have not been able to give a good answer to the question of decentralization.

Therefore, we started to improve and develop on the existing ISAAC consensus protocol, and began to think about the blockchain network structure which is considered more decentralized, faster, and more scalable.

As a result, with the principles outlined below, we are developing a better BOScoin.

*****

## ISAAC's advanced design principles explanation 

*****

### 1. Decentralization

Ethereum's chief scientist Vitalik Buterin told us about the [meaning and necessity of decentralization of the blockchain in his blog.](https://medium.com/@VitalikButerin/the-meaning-of-decentralization-a0c92b76a274) He said;

* Blockchains are politically decentralized and structurally decentralized, but all nodes behave like one computer for a single state.
* Resistance to network or blockchain failures, resistance to (network) attackers, and (centralized) cartel-forming tolerance within (other than network participants) is why decentralization is necessary.

And also attempt to measure decentralization by using the Nakamoto coefficient index from ["Quantifying Decentralization"](https://news.earn.com/quantifying-decentralization-e39db233c28e) posted by Balaji.S. Srinivasan, CTO of Coinbase.

Recently, EOS has been criticized for their use of 21 Block Producers who are responsible for the validation of the entire blockchain, distorting the value of decentralization.

Decentralization is one of the most important factors in blockchain. Also, as the days go by, technical support and demand for decentralization of public blockchains are increasing.

So the BOScoin development team is designing our ISAAC protocol with decentralization as a key component.

*****

### 2. Gradual Open membership

Congress voting that follows Open Membership is an important part of the BOScoin ecosystem. For BOScoin, which is aiming at one-person voting, the blockchain infrastructure must be operated with stability to make the voting system work.
If the blockchain fails because of the nature of the blockchain, it should not roll-back or terminate the network.

Therefore, to increase the number of network nodes gradually in order to access the desired goal in a reliable and verifiable way, the principle of ensuring sufficient operational stability by constructing a quorum, which is a set of nodes, We've been working on this design.

*****

### 3. Network robustness -  Safety

Let's assume that nodes have different blocks each other in the blockchain network, Can we trust this blockchain system?

Safety, terms in blockchain, means that nodes will not reach consensus based on different results. And Liveness means node will make consensus based on a single value no matter how much takes time. ( [You can find more detail informations through previous AA](https://boscoin.io/introduction-of-isaac-consensus-protocol-for-bosnet/) .)
Liveness & Safety are essential factors for blockchain network robustness. Especially, Safety have to be existed for network dispatch prevention( a.k.a Hardfork ).

Consensus protocol of the bitcoin is concerned on liveness among safety and liveness. Accordingly, bitcoin network can't avoid hardfork to bitcoin-cash, bitcoin gold and etc.

In other words, BOScoin dev team thought that blockchain network should have safety for its sound blockchain network.

As a result, BOScoin dev team  apply safety factor on ISAAC consensus protocol design for keeping network robustness. This decision will help block's finality and prevent blockchain network dispatch.

*****
*****

# Now, Era of ISAAC consensus protocol.

## Feature of ISAAC consensus protocol

Assumptions: Transactions are sent by clients before consensus is started, and stored temporarily in the transaction pool of BOScoin network nodes.  

![ISAAC FLOW](../asset/01_image/isaac_flow.png)

*****

## How the consensus protocol works

### ISAAC protocol for consensus based on ballot.

* Ballot : set of valid transactions
* Four states make nodes reach agreement
  * INIT -  node will propose or receive a ballot.
  * SIGN - node will be checking ballot is valid by node itself.
  * ACCEPT - node will be checking ballot is valid by validators.
  * ALL CONFIRM - node will confirm block with transactions in ballot.
* Process
  * **Network start**
    * At the beginning of the network, the genesis block saved with block . It's height is 1 and round number is 0.
    * The node start with INIT state, height 2 and round 0.
  * **Init**
    * Timer sets up TIMEOUT_INIT.
    * The steps to propose ballot are as follows.
    * If node is proposer of this round broadcasts a ballot (INIT, YES),
      * Ballot includes valid transactions (only hashes) or empty transaction.
      * When it broadcasts ballot, node goes to the next state.
    * If node is not proposer, it waits for and receives ballot (INIT, YES).
      * When it receives the ballot, node goes to the next state.
    * When timer ends, node goes to the next state.
  * **SIGN**
    * Timer resets and sets up again TIMEOUT_SIGN
    * Node checks proposed ballot is valid.
    * Each node receives ballots and when number of B(SIGN, YES/NO) is greater than or equal to 2/3 of validators, the node broadcasts B(ACCEPT, YES/NO).
      * it is not possible to satisfy above two conditions, node broadcasts B(ACCEPT, EXP).
    * When the timer expires, node broadcasts B(ACCEPT, EXP).
    * Node goes to the next state.
  * **ACCEPT** 
    * Timer resets and sets up again TIMEOUT_ACCEPT.
    * Each node receives ballots and moves to the same process with SIGN.
  * **ALL-CONFIRM**
    * Node confirms and saves the block with proposed transactions, ballot, even though it is empty.
    * Node filters invalid transactions in the transaction pool.
    * Node waits for TIMEOUT_ALLCONFIRM.
    * It goes to INIT state with height + 1 and round 0.

### Transaction Protocol for sharing transactions to all nodes in the network

* All nodes must validate the transactions in the proposed ballot by itself and vote with the result.
* Increasing efficiency, only hash of the transaction is included in ballot.
* Process
  * A client sends a message, that it has a transaction, to the node.
  * Node receives the message and goes to the next process with the transaction inside of the message.
    * Node checks the format of the transactions whether they are well formed or not.
    * If the same transaction is already in the transaction pool, nodes stops this process.
    * Node validates transaction and checks whether transaction is valid or not.
  * Node broadcasts the message to all nodes except the sender.

### How to select Proposer

* Proposer will be selected from the algorithm based on block height and round.
* Block height is last confirm block.
* Round : A variable used within one block generation process. When some nodes run abnormally, the round needed to select intact node as proposer.
* Proposer selection process
  * Sorting validators in a validator's address by alphabetical order.
  * n = (block height + round number) mod len(validators)
  * The proposer is n'th validator.
* Proposer selection method

```golang

func CalculateProposer(blockHeight int, roundNumber int) string {
    candidates := sort.StringSlice(nr.connectionManager.RoundCandidates())
    candidates.Sort()

    return candidates[(blockHeight + roundNumber)%len(candidates)]
}
```

*****

## Which specific features has there been progress?

### **1. Improve Hard-fork resist & decentralization**

The blockchain projects, which use POW and emphasize liveness, are Bitcoin and Ethereum. These projects have huge support from the community, but nonetheless Bitcoin and Ethereum could not avoid Hardforking.

Under the principle that BOScoin network maintains safety, each node that participates in the network can prevent a hard fork and raise the decentralization factor through an algorithm that ensure each node is fairly elected to Proposer.

Let's compare it to existing cryptocurrency projects. For example, EOS is a DPOS algorithm, so it has hard fork resistance, but it is relatively insufficient for consensus participated as independent node. Stellar has successfully implemented decentralization and hard fork resistance through SCP, but it is difficult for new nodes to participate in the consensus due to the node network topology.

The BOScoin development team expects that the Proposer algorithm implemented in ISAAC will allow nodes to participate in fair settlement based on round number and block height.

### **2. Improve expanding open-source developer's ecosystem & concurrency performance**

Cryptocurrency projects have used many different languages for their implementation depending on the specification and background of the project.
The performance of the blockchain is also important, but the public blockchain should also focus on creating the developer ecosystem.

On September 6, 2018, the top 100 projects by market cap used c ++ (23) implementation languages ​​and the second most used language was Golang (18).
Golang has provided a variety of libraries for cryptocurrency projects and we decided that the syntax was simple enough to make the developer ecosystem relatively easy.

We have expanded the open-source global development ecosystem through Golang to encourage the participation of developers from around the world.
Golang also has the advantage of being able to handle concurrent programming with goroutine and handle many transactions.

### **3.Expansion improvement**

Trust contracts and Congress voting are currently under active development.
At this time, ISAAC development consideration is based on scalability to smoothly drive Trust Contract and Voting.
The current ISAAC consensus protocol supports transactions only, this next version will extending functionality to Trust Contracts and Congress voting.

## Future work

Next posting will show you about what is our plan for completion of the proposer election process, and what is the plan to make a testing enviroment for quality control on the BOScoin nework.
