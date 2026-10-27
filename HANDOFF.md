# What is worth handing to Kaspa core

Each item has four lines. **Hand** is the sentence this re-read would still give core. **Already written** is where the pin board or tn-master already says it. **26 Oct** is the sentence from the earlier draft that this re-read changed or kept. **Wrong if** is what would make the hand false.

Certainty, using tn-master's labels: **A** was measured on TN10 by the page linked, and this pass did not repeat the measurement. **Read** means the source file was opened on 27 Oct 2026. **Open** means the GitHub issue was still open that morning.

## 1. Say whether live 10 BPS is already inside the parent-cap comment

**Hand.** In `consensus/core/src/config/bps.rs` at v2.1.0, `max_block_parents` returns 16 when `ghostdag_k / 2` is above 16. For 10 blocks per second, `ghostdag_k` is 124, so `k/2` is 62, and the function returns 16. The comment in that branch says that as BPS might grow beyond 10, blocks will reference fewer parents than the average number of DAG tips, and that mainnet then needs a short security analysis or parameterless DAGKnight. Worth handing: one public sentence, next to KIP-14, on whether the live network (10 blocks per second, delay bound 5 seconds, k = 124, parent cap 16) is already that case.

**Already written.** The pin board's consensus-papers cell, on `b8371b7`, already says: the code follows the Poisson tail only; at mean 100, P(X>123) is 0.011244 and P(X>124) is 0.008774; the paper's other term at k = 124 is 100/224; the full f is not below 0.01; `max_block_parents` caps at 16. This pass recomputed the two probabilities and got the same digits. tn-master does not discuss k or the parent cap.

**26 Oct.** The draft asked core to publish the attacker fraction under the full theorem with the parent cap included. This re-read withdraws that ask. This pass did not derive an attacker fraction, and it did not derive the average number of tips. The comment places the tip sentence at "BPS might grow beyond 10." Quoting the comment as if it had already declared the live rate inside that case would go past the words.

**Wrong if.** Core has already published that sentence and this pass missed it. Or `ghostdag_k()` for 10 is not 124 in v2.1.0. The const table in the file that was read says `10 => 124`.

Master `4edcbbcc` changes `bps.rs`. The diff is a `cfg(feature = "std")` gate and `core::f64` in place of `std::f64` inside `calculate_ghostdag_k`. The k table and the parent-cap comment are the same text.

## 2. Put the IBD finality comment next to the 12-hour number

**Hand.** `are_pruning_points_violating_finality` in `consensus/src/pipeline/virtual_processor/processor.rs` says it can only detect a finality violation at a depth of `2 * finality_depth`, and that it can give a false negative for a smaller violation. IBD in `protocol/flows/src/ibd/flow.rs` disconnects the peer when that function returns true. `finality_depth` at 10 blocks per second is 432,000 blocks, the 43,200-second constant in `consensus/core/src/config/constants.rs`. Two finality depths is about 24 hours. Worth handing: the 12-hour depth is already public; the false-negative comment is not on the pin board. Print the comment's limit beside the 12-hour figure.

**Already written.** The pin board states finality depth 432,000, merge depth 36,000, coinbase maturity 1,000 blocks, and pruning depth 1,080,000. tn-master does not discuss this function.

**26 Oct.** The draft said a partition that lasts through the finality point does not heal, and that exchanges which credit on a short confirmation are inside the reorg window on purpose. This re-read withdraws both. Sink search does ignore a candidate that is not a chain ancestor of the finality point, and it logs `Finality Violation Detected`. That is the code. A partition test was not run. No exchange policy was read. Coinbase maturity of 100 seconds is a delay on spending the subsidy. The file does not call it economic finality.

**Wrong if.** The function text on master `4edcbbcc` differs from the tag. It does not. The function body was extracted from both blobs on 27 Oct and the text matched. The function starts at line 1612 on the tag and line 1606 on master, because earlier lines in the file were edited. The edit above it renames `accepted_id_digests` to `SeqCommit`. It does not change this function.

**Read.** Comment only. No two-node partition.

## 3. The mempool assert is still in the release

**Hand.** `mining/src/mempool/validate_and_insert_transaction.rs` asserts, after eviction, that the pool count is under the cap and that the byte size can take the new transaction. The error `RejectMempoolIsFull` already exists and is what `limit_transaction_count` returns when it cannot free a slot. The assert is the path that exits the process instead. The blob id of that file is `d286938c637aaf67e0be0775a5dfa68b5f607340` on tag v2.1.0 and on master `4edcbbcc`.

**Already written.** tn-master FINDINGS 4, label **A**, links [F1](https://github.com/STP-KAS/tn10-storm-2026-09-25-r1/blob/main/findings/F1-mempool-cap-panic.md). That page says a TN10 node running kaspad 2.1.0 with `--ram-scale=0.1` panicked once on 25 Sep 2026 at that file, line 123, with `Transactions in mempool: 100001, max: 100000`, and that an upstream issue was not filed. Advice item 6 says not to run a public node at that scale. This pass did not re-run the node and did not open the issue F1 left unfiled.

**26 Oct.** The draft described the assert and asked for a test that fills a small cap. That hand stays. The draft had not pointed at F1. The crash report is tn-master's measurement, not a measurement from 26 or 27 Oct.

**Wrong if.** A commit after `4edcbbcc` returns `RejectMempoolIsFull` from that assert. The master tip on 27 Oct was still `4edcbbcc`. Or F1's log is not a panic of that assert. The log line F1 quotes names the file and the line.

## 4. The throttle floor is already pinned, and still unfixed

**Hand.** In `protocol/flows/src/v10/txrelay/flow.rs`, `request_transactions` divides by `elapsed_time.as_millis().max(1)`. `check_tx_throttling`, in the same file, divides by `as_millis()` with no floor. A zero-millisecond sample with a count of at least 1 becomes `u64::MAX` through `unwrap_or`, and throttling turns on. It stays on until a later sample is under half the threshold. The check is on a 10-second cadence. Relay policy. The blob id `e16cb2ddf3f849526c4532062a58bca7d1a0328e` is the same on the tag and on master `4edcbbcc`.

**Already written.** The pin board's Live cell already says this, on `01b532e8`: `check_tx_throttling` divides by a raw millisecond count, so a sample under 1 ms can turn throttling on. tn-master does not mention it.

**26 Oct.** The draft handed it as one of six findings. This re-read keeps the one-line fix and withdraws any claim that the pin board had missed it.

**Wrong if.** The two divisions are not the ones just described. They were re-read on 26 Oct in this file, and the blob id matched again on 27 Oct.

## 5. Change KIP-21 section 5.3, not the node

**Hand.** KIP-21 at kips `e4ae2332117b5cb68bd6188e065ef885b6d17939`, section 5.3, purges a lane when `last_touch_blue_score + F <= B.blue_score`. Section 5.4 sets `target = B.blue_score - F - 1`. `inclusive_prune_cutoff` in `bounds.rs` is `blue_score - F - 1`. The test next to it says the active window at blue score 100 with F = 10 is `[90, 100]`, so 90 is still live, and a cutoff of 90 would delete it. The node matches section 5.4. A second implementation that follows section 5.3 will drop the boundary score the node keeps. [kips #51](https://github.com/kaspanet/kips/issues/51) asks to make 5.3 match the implementation. It was still open on 27 Oct (last update 23 Oct). Worth handing: edit the KIP. Leave the node.

**Already written.** The pin board's KIPs cell, dated 24 Oct on the same `b8371b7` page, records #51 and says that check against the code was not done there. `bounds.rs` blob `53b6df8e16091a85e7e8f8b8b905766050ab70df` is the same on the tag and on master.

**26 Oct.** The draft said the same predicate mismatch. This re-read kept it, and checked the KIP lines again on 27 Oct. Guest proofs were not run.

**Wrong if.** Section 5.3 on `e4ae233` does not use `<=`, or `inclusive_prune_cutoff` is not `blue_score - F - 1`. Both were read again on 27 Oct.

## 6. #1146 and #1147 are still open. The builder fee rule is already in tn-master

**Hand.** [Issue #1146](https://github.com/kaspanet/rusty-kaspa/issues/1146) was open on 27 Oct: a synced TN10 node keeps already-accepted transactions in the mempool. [Pull #1147](https://github.com/kaspanet/rusty-kaspa/pull/1147) was open, not merged, head `239556a60d`, updated 15 Oct. Worth handing as the status of that pair. Nothing more.

**Already written.** tn-master FINDINGS 11, label **upstream, not tested by us**, describes a node that finishes IBD and can keep transactions the synced blocks already accepted, and quotes a comment on #1147: one non-mining TN10 node at priority 1,644 sompi/gram against 100 on a public endpoint. tn-master ADVICE items 1 and 3 already tell a builder to read more than one node, to drop outliers, and to cap the fee. OPEN-QUESTIONS 6 says they have not checked that quote against their own fee logs, and asks whether #1147 covers a node that falls behind without a full IBD.

**26 Oct.** The draft told core to remove a transaction from the ready set when consensus has accepted it, and pointed at `on_new_block` plus a comment in `mining/src/manager.rs`. That comment is a smaller window: a transaction already accepted in consensus and not yet passed through `handle_new_block_transactions`. tn-master's finding is the post-IBD retention. This pass did not show that the manager comment is #1146. It did not re-measure 1,644 sompi/gram. `ALPHA = 3` in `selectors.rs` was read on 26 Oct and was not the thing that changed. The hand that survives is the open issue and the open pull.

**Wrong if.** #1146 or #1147 merged after the read on the morning of 27 Oct. They had not.

## Withdrawn, or not handed

- **Mass ceiling.** tn-master FINDINGS 1, label **A**: about 3,040 to 3,080 ordinary payments per second at 10 blocks per second, from a 500,000 compute-mass block. That is the block limit in `params.rs`. It is not a defect to hand core.
- **Mempool not saved across restart.** tn-master FINDINGS 5, label **A**. The advice there is to re-broadcast. This pass did not read a comment that claims the mempool is saved.
- **KRC-20.** The pin board's KRC-20 cell says it was not an L1 exploit. L1 accepted the transactions as script-valid. This pass did not reopen that.
- **#1073** (covenant rejected, "script units exceeded", compute budget set) was still open. Not traced against a transaction in either pass.
- **Live cell tip.** On `b8371b7` the Live cell still names rusty-kaspa master `67686482` (14 Oct, the #1074 merge). The Covenant-id cell and the Not-live cell on the same page name `4edcbbcc` (19 Oct). The release tag is v2.1.0. This note uses the tag for the node and `4edcbbcc` for unreleased master. That is a pin-board mismatch, not a node bug, and it is not something to hand Kaspa core.
- **grok-reasoning** on `build/grok-reasoning-2026-10-15` is a 15 Oct note about a TN10 coinbase script and an indexer age. It does not change items 1–6. [SCOPE.md](SCOPE.md).
- **kaspa-builders** `main` `c012d124b934` (27 Oct). A search for `1146`, `finality`, and `mempool panic` returned no row. The catalog was not used.
