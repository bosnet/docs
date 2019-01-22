# Introduction

Documentation for [BOScoin](http://boscoin.io)'s projects, mainly Sebak, its node implementation with the [ISAAC consensus protocol](http://devteam.blockchainos.org/introduction-isaac-consensus-protocol-betterment/).

# ISAAC

The ISAAC is a consensus protocol based on PBFT. In the ISAAC, all nodes will vote two times to determine which transactions to include in this block. For each voting unit, the nodes have the role proposer or validator. The proposer gathers the transactions for this vote and puts them in a ballot and passes it to the validators. The validator checks that the transactions in the ballot are valid and then votes. If the voting determines that the ballot is valid, all nodes confirm the transactions in the ballot. For more information about ISAAC, you can see [ISAAC consensus protocol for BOSNet](http://devteam.blockchainos.org/introduction-isaac-consensus-protocol-betterment/).

This documentation is also [available online](http://devteam.blockchainos.org).
