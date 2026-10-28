# Kaspa core note

A reading of public files on 28 Oct 2026. This page does not claim the truth.

## What

Six items still worth handing to a Kaspa contributor. "Handing" means a concrete ask: write one sentence, change one line, or pick up an open pull.

The node is [rusty-kaspa](https://github.com/kaspanet/rusty-kaspa) **v2.1.0** (`01b532e8`) and **master** `4edcbbcc` (19 Oct 2026). Master was still that commit on 28 Oct 2026. The release was still v2.1.0.

Read against the pin board [kaspa-master-file](https://github.com/STP-KAS/kaspa-master-file) `40bdc98` and [tn-master](https://github.com/STP-KAS/tn-master) `153913ae`.

## Why

The pin board and tn-master already record several of the numbers below. Repeating them as a new discovery would waste a contributor's time. What remains is the ask that is still open.

## How

On 28 Oct 2026 the pin board, tn-master, the v2.1.0 tag, and master were read again. Issues [#1146](https://github.com/kaspanet/rusty-kaspa/issues/1146), [#1147](https://github.com/kaspanet/rusty-kaspa/pull/1147), and [kips #51](https://github.com/kaspanet/kips/issues/51) were fetched again. No node was started. No transaction was sent. No issue was opened on kaspanet.

## Still worth handing

### 1. Is live 10 blocks per second already inside the parent-cap comment?

**Ask.** One public sentence, next to KIP-14.

In `consensus/core/src/config/bps.rs`, `max_block_parents` returns 16 when `ghostdag_k / 2` is above 16. At 10 blocks per second, k is 124, so k/2 is 62, and the function returns 16. The comment says that as blocks per second grow beyond 10, blocks reference fewer parents than the average number of DAG tips, and mainnet then needs a short security analysis or parameterless DAGKnight.

The sentence: is the live network (10 blocks per second, delay bound 5 seconds, k = 124, parent cap 16) already that case?

The pin board already prints the cap of 16, the Poisson tail, and the paper's other term (100/224 at k = 124). This page has no attacker fraction.

### 2. Say how far the IBD finality check can see

**Ask.** Print the limit next to the public 12-hour figure.

`finality_depth` at 10 blocks per second is 432,000 blocks (43,200 seconds, about 12 hours). `are_pruning_points_violating_finality` in `consensus/src/pipeline/virtual_processor/processor.rs` says a violation is detectable at `2 * finality_depth` (about 24 hours), and that a shallower violation can pass unseen. IBD in `protocol/flows/src/ibd/flow.rs` disconnects the peer when that function returns true.

The 12-hour depth is on the pin board. The 24-hour limit in that comment is not.

### 3. Return "mempool full" instead of asserting

**Ask.** On that path, return `RejectMempoolIsFull`. The error already exists.

`mining/src/mempool/validate_and_insert_transaction.rs` asserts, after eviction, that the pool is under its count cap and can take the new transaction's bytes. `limit_transaction_count` already returns `RejectMempoolIsFull` when it cannot free a slot. The assert is the path that exits the process.

tn-master FINDINGS 4 records one panic on TN10, 25 Sep 2026, kaspad 2.1.0 with `--ram-scale=0.1`, at that file, line 123: mempool count 100001 against a max of 100000. That report says an upstream issue was not filed. This page did not run the node again. The assert is in v2.1.0 and in master `4edcbbcc`.

### 4. Give the relay throttle the same millisecond floor

**Ask.** In `check_tx_throttling`, divide by `as_millis().max(1)`, as `request_transactions` already does.

Both are in `protocol/flows/src/v10/txrelay/flow.rs`. With no floor, a sample under 1 ms and a count of at least 1 becomes `u64::MAX`, and throttling turns on. It stays on until a later sample is under half the threshold. The check runs about every 10 seconds. This is relay policy. The same code is in v2.1.0 and in master. The pin board's Live cell already describes it. The line is still unfixed.

### 5. Make KIP-21 section 5.3 match the node

**Ask.** Edit the KIP. [kips #51](https://github.com/kaspanet/kips/issues/51) is that edit. It was open on 28 Oct 2026. The node already follows section 5.4.

Section 5.3 drops a lane when `last_touch_blue_score + F <= B.blue_score`. Section 5.4 sets the cutoff to `B.blue_score - F - 1`. `inclusive_prune_cutoff` in `consensus/src/pipeline/virtual_processor/bounds.rs` is `blue_score - F - 1`. A second implementation that follows 5.3 as written will drop the boundary score the node keeps. The pin board's KIPs cell already quotes this mismatch.

### 6. Finish the mempool cleanup after IBD

**Ask.** [Issue #1146](https://github.com/kaspanet/rusty-kaspa/issues/1146) and [pull #1147](https://github.com/kaspanet/rusty-kaspa/pull/1147) were open on 28 Oct 2026. #1147 was not merged. Its head was `239556a60d` (15 Oct 2026).

#1146 reports a synced TN10 node that keeps transactions the chain has already accepted. #1147 revalidates the mempool once IBD completes. tn-master already tells a builder to read more than one node and to cap the fee. The fee figure quoted there was not measured again for this page.
