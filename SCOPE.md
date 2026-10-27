# Scope

27 Oct 2026. Public sources only. No kaspanet repository was modified.

## Pin board

[STP-KAS/kaspa-master-file](https://github.com/STP-KAS/kaspa-master-file) `main` `b8371b72eb61e2d09ff50cb7e6f5cb885b06e240`.

The Now board was read from the top cell through the Do-not-weld cell. The cells that already contained a 26 Oct sentence are named in [HANDOFF.md](HANDOFF.md): Live (throttle, and the stale master tip `67686482`), consensus papers (tail, second term, parent cap, finality depth), Covenant-id and Not-live (master `4edcbbcc`), KIPs (kips `e4ae233`, issue #51 not checked there), KRC-20.

## tn-master

[STP-KAS/tn-master](https://github.com/STP-KAS/tn-master) `main` `153913aea045` (15 Oct 2026).

Read: `README.md`, `WHAT-WHY-HOW.md`, `DISCLAIMER.md`, `FINDINGS.md`, `ADVICE-TO-DEVELOPERS.md`, `docs/LESSONS.md`, `docs/OPEN-QUESTIONS.md`.

FINDINGS 4 and [F1](https://github.com/STP-KAS/tn10-storm-2026-09-25-r1/blob/main/findings/F1-mempool-cap-panic.md) are the crash report behind handoff item 3. FINDINGS 11 and ADVICE items 1 and 3 are the fee pages behind item 6. Storm headline counts were not re-added. Kaspa Pulse's counts stay his, as tn-master already says.

## Node files

Release tag v2.1.0 is commit `01b532e8b553523216471682649693af92f0fd16`. Master tip on 27 Oct 2026 was `4edcbbccb449f84832afb1ca157904b35f37d006`.

| File | Blob on v2.1.0 | Blob on `4edcbbcc` |
| --- | --- | --- |
| `mining/src/mempool/validate_and_insert_transaction.rs` | `d286938c637aaf67e0be0775a5dfa68b5f607340` | same |
| `protocol/flows/src/v10/txrelay/flow.rs` | `e16cb2ddf3f849526c4532062a58bca7d1a0328e` | same |
| `consensus/src/pipeline/virtual_processor/bounds.rs` | `53b6df8e16091a85e7e8f8b8b905766050ab70df` | same |
| `consensus/core/src/config/bps.rs` | `eb8982e9d01129a2c28e1d23b21767a9d83b434f` | `a1e523038b29a1c804ed23474915fa92de0ebc21` |
| `consensus/src/pipeline/virtual_processor/processor.rs` | `68eb49e12dff94a35006329d509b561f6fe5ab24` | `ce87653ea453fcd731a2a7d580d0a8546563d3e0` |

`bps.rs` differs by a `std` feature gate and `core::f64` inside `calculate_ghostdag_k`. The k table and the parent-cap comment match.

`processor.rs` differs. On 26 Oct the diff of that file was the `SeqCommit` rename. On 27 Oct the text of `are_pruning_points_violating_finality`, including the comment above it, was extracted from both blobs and matched. The rest of that file was not re-read line by line on 27 Oct.

KIP-21 sections 5.3 and 5.4 were read again from kips `e4ae2332117b5cb68bd6188e065ef885b6d17939`. The 5.3 predicate is `last_touch_blue_score + F <= B.blue_score`. Section 5.4 sets `target = B.blue_score - F - 1`.

The Poisson tail was recomputed on 27 Oct at mean 100. P(X > 123) = 0.011243536494169537. P(X > 124) = 0.008773598988627285. The pin board rounds these to 0.011244 and 0.008774.

## Issues re-checked on 27 Oct

| Item | State that morning |
| --- | --- |
| rusty-kaspa #1146 | open, updated 13 Oct |
| rusty-kaspa #1147 | open, not merged, head `239556a60d`, updated 15 Oct |
| rusty-kaspa #1073 | open, updated 13 Oct |
| kips #51 | open, updated 23 Oct |

## Opened, and not used for a sentence

- [grok-reasoning](https://github.com/STP-KAS/kaspa-master-file/tree/build/grok-reasoning-2026-10-15/grok-reasoning) on `build/grok-reasoning-2026-10-15`. The README there says the folder is not the Now board and not a claim that a note is true. Its one note, `2026-10-15-vertex-tn10-miner.md`, separates a coinbase version, a payout script, and an indexer age. That subject does not meet items 1–6, so the note was not folded in.
- [kaspa-builders](https://github.com/STP-KAS/kaspa-builders) `main` `c012d124b934` (27 Oct 2026). A code search for `1146`, `finality`, and `mempool panic` returned no file. No catalog row was added here.

## Not done

- No kaspad process was started. The 25 Sep panic was not repeated. The 1,644 sompi/gram quote was not measured.
- Subsidy division, duplicate-input rejection, and the coinbase hash check were read on 26 Oct and were not re-opened on 27 Oct. They are not in the handoff.
- The txscript opcode surface, the pruning-proof validator, the DAGKnight branch, SilverScript, and the KCC repos were not re-audited.
- No pull request was opened on kaspanet, on kips, or on the pin board.
