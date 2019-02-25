---
layout: post
permalink: /how-to-sync-blocks-with-validators/
---
---
## Node State

1. `BOOTING`   - The node is on start but not ready to consensus or sync.
1. `CONSENSUS` - The node has all blocks and ready to proceed consensus.
1. `SYNC`      - The node is not ready for consensus because in sync.
1. `WATCH`     - The node watches it's validators and try to sync. This node does not participate in the consensus.

## Node State Change
* `BOOTING` -> `CONSENSUS`
The status changes from `BOOTING` to `CONSENSUS` when node running and a sufficient number of validator nodes are connected.
* `CONSENSUS` -> `SYNC`
The node state changes from `CONSENSUS` to `SYNC` when its block height is lower than the agreed block height through voting.
   1. A node receives `ACCEPT` and `YES` ballots only.
   1. When the number of ballots exceeds the threshold,
   1. And when the height of this agreed ballot is different with `Last Confirmed Height + 1`.
   1. The status changes from `CONSENSUS` to `SYNC`.
* `SYNC` -> `CONSENSUS`
The status changes from `SYNC` to `CONSENSUS` in case of,
   * The node is in same height and round with the other validators.
   * The node has all blocks, transactions and operations.
   * All blocks the node has are valid.
   * The node agrees on the proposer's proposing block and save it.
* `WATCH`
The node sets as a watcher is always `WATCH` state. It does not change to another node state.

## How does a node determine if itself needs to sync?
1. In [`BallotCheckSYNC`](https://github.com/bosnet/sebak/blob/master/lib/node/runner/checker.go#L181) method, a node receives B(`ACCEPT`, `YES`) only.
1. When a node receives the ballots based same height `h` and round from greater than or equal to 67% validators, it asks validators for blocks up to `h`.
1. But in this timing, the other nodes confirms the new block with `h+1` because it also receives enough B(`ACCEPT`, `YES`).
1. Therefore, a node saves ballot with `h` like cache, then in next height it saves two blocks(`h` and `h+1`) at the same time.

### Example
1. There are 4 nodes(`A`, `B`, `C`, `D`) and threshold is 3. And B(`ACCEPT`, `YES`, 10) means a ballot with `ACCEPT` state , `YES` vote and based height 10.
   1. `A`, `B`, `C` are in height 10, but `D` is in height 5 yet.
   1. `A`, `B`, `C` keep voting next height so `D` receives three B(`ACCEPT`, `YES`, 10) from them.
   1. `D` requests the block up to 10 to the validators and save B(`ACCEPT`, `YES`, 10) as a cache.
   1. In next height, `D` receives three B(`ACCEPT`, `YES`, 11) from validators as above.
   1. The node confirms two blocks based on the ballots
      1. Based on B(`ACCEPT`, `YES`, 10), `D` confirms the block with height 11.
      1. Based on B(`ACCEPT`, `YES`, 11), `D` confirms the block with height 12.
   1. It can be participate in consensus voting from 13 height.
