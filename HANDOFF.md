# What is worth handing to Kaspa core

28 Oct 2026. The node commit is the same one named on 27 Oct. Where a sentence below says the source is unchanged, that means `master` is still `4edcbbccb449f84832afb1ca157904b35f37d006`.

Each item has four lines. **Hand** is the sentence this re-read would still give core. **Already written** is where the pin board or tn-master already says it. **Earlier** is the 26 or 27 Oct sentence this re-read changed or kept. **Wrong if** is what would make the hand false.

Certainty, using tn-master's labels: **A** was measured on TN10 by the page linked, and this pass did not repeat the measurement. **Read** means the source file was opened. **Open** means the GitHub issue was still open on 28 Oct.

## 1. Say whether live 10 BPS is already inside the parent-cap comment

**Hand.** In `consensus/core/src/config/bps.rs` at v2.1.0, `max_block_parents` returns 16 when `ghostdag_k / 2` is above 16. For 10 blocks per second, `ghostdag_k` is 124, so `k/2` is 62, and the function returns 16. The comment in that branch says that as BPS might grow beyond 10, blocks will reference fewer parents than the average number of DAG tips, and that mainnet then needs a short security analysis or parameterless DAGKnight. Worth handing: one public sentence, next to KIP-14, on whether the live network (10 blocks per second, delay bound 5 seconds, k = 124, parent cap 16) is already that case.

**Already written.** The pin board's consensus-papers cell, still the same text on `40bdc98` as on `b8371b7`, already says: the code follows the Poisson tail only; at mean 100, P(X>123) is 0.011244 and P(X>124) is 0.008774; the paper's other term at k = 124 is 100/224; the full f is not below 0.01; `max_block_parents` caps at 16. tn-master does not discuss k or the parent cap.

**Earlier.** The 26 Oct draft asked core to publish the attacker fraction under the full theorem with the parent cap included. The 27 Oct re-read withdrew that ask. This pass did not derive an attacker fraction, and it did not derive the average number of tips. The comment places the tip sentence at "BPS might grow beyond 10." Quoting the comment as if it had already declared the live rate inside that case would go past the words.

**Wrong if.** Core has already published that sentence and this pass missed it. Or `ghostdag_k()` for 10 is not 124 in v2.1.0. The const table in the file read on 26 Oct says `10 => 124`, and that file's commit is still the master tip.

**28 Oct.** Consensus-papers was not one of the six cells the sweep rewrote. The tail figures were not recomputed today. The `bps.rs` difference recorded on 27 Oct, a `std` feature gate and `core::f64` inside `calculate_ghostdag_k`, is still the difference inside this same tip. A search of commits authored by `biryukovmaxim` on kaspanet since 15 Oct returned the #1060 merge, "Consensus core nostd" (16 Oct). The tip itself, #1065, lists him as a co-author. Both are already inside `4edcbbcc`. This pass did not re-diff `bps.rs`.

## 2. Put the IBD finality comment next to the 12-hour number

**Hand.** `are_pruning_points_violating_finality` in `consensus/src/pipeline/virtual_processor/processor.rs` says it can only detect a finality violation at a depth of `2 * finality_depth`, and that it can give a false negative for a smaller violation. IBD in `protocol/flows/src/ibd/flow.rs` disconnects the peer when that function returns true. `finality_depth` at 10 blocks per second is 432,000 blocks, the 43,200-second constant in `consensus/core/src/config/constants.rs`. Two finality depths is about 24 hours. Worth handing: the 12-hour depth is already public; the false-negative comment is not on the pin board. Print the comment's limit beside the 12-hour figure.

**Already written.** The pin board states finality depth 432,000, merge depth 36,000, coinbase maturity 1,000 blocks, and pruning depth 1,080,000. A search of the 28 Oct README for `are_pruning` returned no hit. tn-master does not discuss this function.

**Earlier.** The 26 Oct draft said a partition that lasts through the finality point does not heal, and that exchanges which credit on a short confirmation are inside the reorg window on purpose. The 27 Oct re-read withdrew both. Sink search does ignore a candidate that is not a chain ancestor of the finality point, and it logs `Finality Violation Detected`. That is the code as read on 26 Oct. A partition test was not run. No exchange policy was read. Coinbase maturity of 100 seconds is a delay on spending the subsidy. The file does not call it economic finality.

**Wrong if.** The function text on a master commit after `4edcbbcc` differs from the tag. There is no such commit. On 27 Oct the function body was extracted from both blobs and the text matched.

**28 Oct.** The function was not extracted again. Open pull #1156 ("Improved pruning") touches `consensus/src/model/stores/reachability.rs` and `database/src/set_access.rs`. It does not touch `processor.rs`. It is not merged.

**Read.** Comment only. No two-node partition.

## 3. The mempool assert is still in the release

**Hand.** `mining/src/mempool/validate_and_insert_transaction.rs` asserts, after eviction, that the pool count is under the cap and that the byte size can take the new transaction. The error `RejectMempoolIsFull` already exists and is what `limit_transaction_count` returns when it cannot free a slot. The assert is the path that exits the process instead. The blob id of that file is `d286938c637aaf67e0be0775a5dfa68b5f607340` on tag v2.1.0 and on master `4edcbbcc`.

**Already written.** tn-master FINDINGS 4, label **A**, links [F1](https://github.com/STP-KAS/tn10-storm-2026-09-25-r1/blob/main/findings/F1-mempool-cap-panic.md). That page says a TN10 node running kaspad 2.1.0 with `--ram-scale=0.1` panicked once on 25 Sep 2026 at that file, line 123, with `Transactions in mempool: 100001, max: 100000`, and that an upstream issue was not filed. Advice item 6 says not to run a public node at that scale. This pass did not re-run the node and did not open the issue F1 left unfiled. tn-master `main` is still `153913aea045`.

**Earlier.** The 26 Oct draft described the assert and asked for a test that fills a small cap. That hand stays. The crash report is tn-master's measurement, not a measurement from 26, 27, or 28 Oct.

**Wrong if.** A commit after `4edcbbcc` returns `RejectMempoolIsFull` from that assert. The master tip on 28 Oct was still `4edcbbcc`. Or F1's log is not a panic of that assert. The log line F1 quotes names the file and the line.

## 4. The throttle floor is already pinned, and still unfixed

**Hand.** In `protocol/flows/src/v10/txrelay/flow.rs`, `request_transactions` divides by `elapsed_time.as_millis().max(1)`. `check_tx_throttling`, in the same file, divides by `as_millis()` with no floor. A zero-millisecond sample with a count of at least 1 becomes `u64::MAX` through `unwrap_or`, and throttling turns on. It stays on until a later sample is under half the threshold. The check is on a 10-second cadence. Relay policy. The blob id `e16cb2ddf3f849526c4532062a58bca7d1a0328e` is the same on the tag and on master `4edcbbcc`.

**Already written.** The pin board's Live cell already says this, on `01b532e8`: `check_tx_throttling` divides by a raw millisecond count, so a sample under 1 ms can turn throttling on. The 28 Oct sweep did not rewrite that cell. tn-master does not mention it.

**Earlier.** The 26 Oct draft handed it as one of six findings. The 27 Oct re-read kept the one-line fix and withdrew any claim that the pin board had missed it.

**Wrong if.** The two divisions are not the ones just described. They were re-read on 26 Oct in this file, and the blob id matched again on 27 Oct. The commit is unchanged on 28 Oct.

## 5. KIP-21 section 5.3 is already on the board. Nothing new to hand

**Hand.** Nothing beyond the open issue. [kips #51](https://github.com/kaspanet/kips/issues/51) was still open on 28 Oct (last update 23 Oct). It already asks to make section 5.3 match the implementation. kips `master` is still `e4ae2332117b5cb68bd6188e065ef885b6d17939`.

**Already written.** The KIPs cell on `40bdc98`, which the 28 Oct sweep did not rewrite, quotes #51: section 5.3 purges a lane when `last_touch_blue_score + F <= B.blue_score`, rusty-kaspa purges it one blue score later (`bounds.rs`), and section 5.4 matches the code. The issue has 0 comments on that cell's reading.

**Earlier.** The 27 Oct note said the KIPs cell recorded #51 and said the check against the code was not done there. That second clause is not in the cell. The cell quotes the issue, and the issue states the mismatch. The 27 Oct read of the KIP lines and of `inclusive_prune_cutoff` (`blue_score - F - 1`) still sits on these same commits. It agrees with the issue. It is not a new hand. Guest proofs were not run.

**Wrong if.** Section 5.3 on `e4ae233` does not use `<=`, or #51 is closed. The predicate was read on 27 Oct. The issue was still open on 28 Oct.

## 6. #1146 and #1147 are still open. The builder fee rule is already in tn-master

**Hand.** [Issue #1146](https://github.com/kaspanet/rusty-kaspa/issues/1146) was open on 28 Oct: a synced TN10 node keeps already-accepted transactions in the mempool. [Pull #1147](https://github.com/kaspanet/rusty-kaspa/pull/1147) was open, not merged, head `239556a60d`, last update 15 Oct. Worth handing as the status of that pair. Nothing more.

**Already written.** The pin board's TN10 public API cell was rewritten on 28 Oct and still says both are open, head `239556a6`, not merged, and that the public endpoint answered as kaspad 2.1.0. tn-master FINDINGS 11, label **upstream, not tested by us**, describes a node that finishes IBD and can keep transactions the synced blocks already accepted, and quotes a comment on #1147: one non-mining TN10 node at priority 1,644 sompi/gram against 100 on a public endpoint. tn-master ADVICE items 1 and 3 already tell a builder to read more than one node, to drop outliers, and to cap the fee. OPEN-QUESTIONS 6 says they have not checked that quote against their own fee logs, and asks whether #1147 covers a node that falls behind without a full IBD.

**Earlier.** The 26 Oct draft told core to remove a transaction from the ready set when consensus has accepted it, and pointed at `on_new_block` plus a comment in `mining/src/manager.rs`. That comment is a smaller window: a transaction already accepted in consensus and not yet passed through `handle_new_block_transactions`. tn-master's finding is the post-IBD retention. This pass did not show that the manager comment is #1146. It did not re-measure 1,644 sompi/gram. The hand that survives is the open issue and the open pull.

**Wrong if.** #1146 or #1147 merged after this read. At the fetch on 28 Oct they had not. #1146 `updatedAt` was still 13 Oct. #1147 `updatedAt` was still 15 Oct. `mergedAt` was null.

## Read on 28 Oct, already written, not handed

- **Maxim, #1152.** Open, not merged, head still `6a68b291` (26 Oct, "perf(indexes): guard covenant bucket scan with tail partition_point"), 22 commits, 42 files, +3060/−80. Review rollup is still `CHANGES_REQUESTED` (IzioDev, 21 Oct). The file list does not include `bps.rs`, `processor.rs`, `bounds.rs`, `validate_and_insert_transaction.rs`, or `txrelay/flow.rs`. The Covenant-id cell already records this head, the file count, IzioDev's request, and no approval. This pass did not re-review the 22 commits. His earlier open pulls (#1104, #1103, #1066, #1051) were not re-read. #1060 is already inside the tip, as item 1 says.
- **#1160.** Open, head `fc6b57f6`, one file. On master the blob of `mining/src/mempool/model/utxo_set.rs` is `ea439e36c5e86a119e87929f80a8e43f37d4eb39`, the same on v2.1.0. `add_transaction` removes `(this transaction id, input index)` from `pool_unspent_outputs`. The comment above that line says the spent output. `outpoint_owner_id` uses `input.previous_outpoint`. Nothing in that file reads `pool_unspent_outputs`. The Not-live cell already quotes the pull, including the author's "write-only" line. The new test in the pull was not run.
- **#1161** is a math-test name swap. Not read past the board's quote. Not handed.
- **Mass ceiling.** tn-master FINDINGS 1, label **A**. Design of the block limit. Not a defect to hand.
- **Mempool not saved across restart.** tn-master FINDINGS 5, label **A**. The advice there is to re-broadcast.
- **KRC-20.** The pin board says it was not an L1 exploit. This pass did not reopen that cell. The sweep did not rewrite it.
- **#1073** was still open (updated 13 Oct). Not traced against a transaction.
- **Live cell tip.** The Live cell still says the 14 Oct master tip is `67686482`. The Covenant-id cell and the Not-live cell still name `4edcbbcc`. The release tag is v2.1.0. The 28 Oct snapshot says the release pin holds. That is a pin-board mismatch, not a node bug, and it is not something to hand Kaspa core.
- **grok-reasoning** on `build/grok-reasoning-2026-10-15` is still `f424e557` (15 Oct). It was not re-read today. The 27 Oct note stands: the vertex page does not meet items 1–6.
- **kaspa-builders** `main` `4e725c609228` (28 Oct) is a catalog sweep of Kastle, Kaspire, go-krc20d, and KOB. A search for `1146`, `finality`, and `mempool panic` returned no file. The catalog was not used.
