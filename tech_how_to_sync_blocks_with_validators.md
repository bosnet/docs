## Node State

1. `BOOTING` - The node is on start but not ready to consensus or synchronize.
1. `SYNC` - The node is ready to synchronize block but not ready for consensus.
1. `CONSENSUS` - The node has all blocks and ready to proceed with the consensus.
1. `TERMINATING` - The node terminates.

## How to check the node state is in `SYNC` or `CONSENSUS`
All node states are checked by the flag in NodeRunner basically.
But the points are; 
1. Cases that node is in `SYNC` state mention below;
   * After start, node ready to receive messages or ballots from validators.
   * The node was in `CONSENSUS` state but when it receives `ACCEPT` ballots, block height is different with `Last Confirmed + 1`.
2. Conditions that node is ready to `CONSENSUS` mention below;
   * Having same height with validators.
   * Having all blocks and transactions.
   * All blocks have been validated.

## When Node is in `SYNC` state,
* Something is proceeded at top, middle, and bottom in blockchain simultaneosly.
   * At the top of the blockchain,
      1. Node gathers `ACCEPT` ballots but it doesn't validate ballots because not all previous blocks and transactions have been received at this time.
      2. When `ACCEPT` ballots exceeds the threshold, Node saves the confirmed block in the `BLOCK POOL`.
      3. Gathering transactions from transaction protocol.
   * In the middle of the blockchain,
      1. Gathering `BLOCK`s from validators.
   * From the bottom of the blockchain,
      1. Validation from genesis block to latest block sequentially.
* The `SYNC` process ends and the node state will change to `CONSENSUS` mention below conditions, 
   * having same height with validators.
   * having all blocks and transactions.
   * all blocks have been validated.

