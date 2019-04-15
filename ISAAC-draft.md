
# Abstract
# Status of This Draft
# Copyright Notice
# Table of Contents
## Introduction
## `ISAAC` Protocol
The `ISAAC` is a consensus protocol based on [`PBFT`](http://pmg.csail.mit.edu/papers/osdi99.pdf).
In the `ISAAC`, all nodes will vote two times to determine which transactions to include in this block.
For each voting unit, the nodes have the role `proposer` or` validator`.
The `proposer` gathers the transactions for this vote and puts them in a ballot and passes it to the `validator`s.
The `validator` checks that the transactions in the ballot are valid and then votes.
If the voting determines that the ballot is valid, all nodes confirm the transactions in the ballot.

## State Diagram

                                          'Height + 1'
      ┌──────────────────────────────────────────────────────────────────────────────────┐
      ╎                                                                                  ╎
      ╎                                                                                  ╎
      ╎    'get ballot'               'get 67% of YES'           'get 67% of YES'        ╎
      ▼                                                                                  ╎
     INIT ────────────────────> SIGN ──────────────────> ACCEPT ──────────────────> ALL_CONFIRM
      ▲                          ╎                         ╎
      ╎                          ╎                         ╎
      ╎  'get 34% of NO or EXP'  ╎  'get 34% of NO or EXP' ╎
      ╎                          ╎                         ╎
      └────────────────────────────────────────────────────┘
                           'Round + 1'

## `ISAAC` States
1. `INIT` - Proposed or received a ballot with transactions.

1. `SIGN` - Checked that the ballot is valid by itself.

1. `ACCEPT` - Checked that the ballot is valid by validators.

1. `ALL_CONFIRM` - Confirmed block with the transactions in the ballot.

## Voting Hole

1. `YES` - Agreed

1. `NO` - Disagreed

1. `EXP` - Expired

## Terms

* TIMEOUT_INIT - The timeout for `INIT` state. The default value is 2 sec.

* TIMEOUT_SIGN - The timeout for `SIGN` state. The default value is 2 sec.

* TIMEOUT_ACCEPT - The timeout for `ACCEPT` state. The default value is 2 sec.

* B(ISAAC State, Voting Hole) - A ballot with ISAAC state and voting hole.

### Examples

* B(`INIT`, `YES`) - A ballot with ISAAC state `INIT` and voting hole `YES`.

* B(`SIGN`, `NO`) - A ballot with ISAAC state `INIT` and voting hole `NO`.

* B(`ACCEPT`, `EXP`) - A ballot with ISAAC state `INIT` and voting hole `EXP`.

## Voting Process

### Network Start
* At the beginning of the network, the genesis block is saved with block height 1 and round 0.
* The node start with `INIT` state, height 2 and round 0.

### `INIT`
1. The timer is set to TIMEOUT_INIT.
1. The steps to propose transactions are as follows.
   * If the node is [proposer](how_to_select_the_proposer.md) of this round, it broadcasts a B(`INIT`, `YES`),
      * The ballot includes valid transactions(only hashes) or empty(If the transaction pool is empty).
      * When the node broadcasts the ballot, the node goes to the next state.
   * If the node is not `proposer`, it waits until receiving the B(`INIT`, `YES`).
      * When it receives the ballot, the node goes to the next state.
   * When the timer expires, the node goes to the next state.

### `SIGN`
1. The timer is reset to TIMEOUT_SIGN.
1. The node checks [the proposed ballot is valid](how_to_check_a_ballot_is_valid.md).
   * If the proposed ballot is valid(or empty), the node broadcasts B(`SIGN`, `YES`).
   * If the proposed ballot is invalid, the node broadcasts B(`SIGN`, `NO`).
   * When the timer expires, the node hasn't sent a ballot in `SIGN` state yet, it broadcasts B(`SIGN`, `EXP`).
1. Each node receives ballots and goes to the next state after,
   * the number of B(`SIGN`, `YES`) is greater than or equal to 67% of validators, the node broadcasts B(`ACCEPT`, `YES`).
   * the number of B(`SIGN`, `NO`) or B(`SIGN`, `EXP`) is greater than 33% of validators,
      the node goes back to `INIT` state with same height and round + 1.

### `ACCEPT`
1. The timer is reset to TIMEOUT_ACCEPT.
1. When the timer expires, the node hasn't sent a ballot in `ACCEPT` state yet, it broadcasts B(`ACCEPT`, `EXP`).
1. Each node receives ballots and,
   * if the number of B(`ACCEPT`, `YES`) is greater than or equal to 67% of validators, the node goes to the next state.
   * if the number of B(`ACCEPT`, `NO`) or B(`ACCEPT`, `EXP`) is greater than 33% of validators,
      the node goes back to `INIT` state with same height and round + 1.

### `ALL-CONFIRM`
1. the node confirms and saves the block with proposed transactions even though it is empty.
1. the node filters [invalid transactions](how_to_check_a_ballot_is_valid.md#transaction-validation) in the transaction pool.
1. It waits for [block time buffer](tech_how_to_calculate_block_time_buffer.md).
1. It goes to `INIT` state with height + 1 and round 0.

## Types
### Ballot
```go
// Before being transmitted, the ballot is converted to a []byte for efficiency by the json.Marshal (v interface {}) method of the golang built-in package encoding/json.
type Ballot struct {
	H BallotHeader
	B BallotBody
}

type BallotHeader struct {
	Version           string // version of `BallotBody`
	Hash              string // hash of `BallotBody`
	Signature         string // signed by source node of <networkID> + `Hash`
	ProposerSignature string // signed by proposer of <networkID> + `Hash` of `BallotBodyProposed`
}

type BallotBody struct {
	Confirmed string            // created time, ISO8601
	Proposed  BallotBodyProposed
	Source    string            // Sender Address
	State     State             // ISAAC States
	Vote      voting.Hole       // Voting Result
}

type BallotBodyProposed struct {
	Confirmed           string              // created time, ISO8601
	Proposer            string
	VotingBasis         Basis
	Transactions        []string            // Transaction Hashes
	ProposerTransaction ProposerTransaction // Transaction always included in the block
}

type Basis struct {
	Round     uint64 // round sequence number
	Height    uint64 // last block height
	BlockHash string // hash of last block
	TotalTxs  uint64
	TotalOps  uint64
}
```

## Signing
### Purpose
All ballots and transactions are signed for verifying sender with the KP.Sign() method of the "github.com/stellar/go/keypair" package.

### Sign
```go
func MakeSignature(kp stellar.KP, networkID []byte, hash string) ([]byte, error)
```
It returns signature which is used for verifying contents.

### Verify
```go
func (kp stellar.KP) Verify(input []byte, signature []byte) error
```
Usually, input includes NetworkID and Hash. Therefore, we can verify that this signature is valid with keypair's publicKey, networkID, and hash.