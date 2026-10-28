# Scope

28 Oct 2026. Public sources only. No kaspanet repository was modified. The pin board was fetched and not edited.

## What moved, and what did not

| Source | 27 Oct pin | 28 Oct pin |
| --- | --- | --- |
| rusty-kaspa `master` | `4edcbbccb449f84832afb1ca157904b35f37d006` | same. `pushedAt` `2026-10-19T15:05:13Z` |
| rusty-kaspa release | v2.1.0 `01b532e8b553523216471682649693af92f0fd16` | same. Published `2026-09-22T13:55:36Z` |
| kaspa-master-file `main` | `b8371b72eb61e2d09ff50cb7e6f5cb885b06e240` | `40bdc9811f92e4db11e1e78c218f66d94fb8738d` |
| tn-master `main` | `153913aea045` | same, `153913aea045387647e80064523889fc0dc55798` |
| kips `master` | `e4ae2332117b5cb68bd6188e065ef885b6d17939` | same |
| kaspa-builders `main` | `c012d124b934` | `4e725c609228184baf1b041eb55d830cd8151146` |

The pin-board compare `b8371b7...40bdc98` is two commits: `b6d26ce` "Daily watch sweep 28 Oct 2026" and `40bdc98` "Link 28 Oct sweep row to b6d26ce." Files: `README.md` 6 lines replaced, `master.json` 5 notes, `SNAPSHOT-HISTORY.md` one new row. The six rewritten README rows are Referee lag, KGI v2 design, vProgs, Argent, KaspaDollar, and TN10 public API. Live, consensus papers, Covenant-id, KIPs, Not live, and KRC-20 incident were not in that diff.

## Node files

The five blobs were not downloaded again. They are contents of `4edcbbcc`, which was re-fetched and matched the 27 Oct tip.

| File | Blob on v2.1.0 | Blob on `4edcbbcc` |
| --- | --- | --- |
| `mining/src/mempool/validate_and_insert_transaction.rs` | `d286938c637aaf67e0be0775a5dfa68b5f607340` | same |
| `protocol/flows/src/v10/txrelay/flow.rs` | `e16cb2ddf3f849526c4532062a58bca7d1a0328e` | same |
| `consensus/src/pipeline/virtual_processor/bounds.rs` | `53b6df8e16091a85e7e8f8b8b905766050ab70df` | same |
| `consensus/core/src/config/bps.rs` | `eb8982e9d01129a2c28e1d23b21767a9d83b434f` | `a1e523038b29a1c804ed23474915fa92de0ebc21` |
| `consensus/src/pipeline/virtual_processor/processor.rs` | `68eb49e12dff94a35006329d509b561f6fe5ab24` | `ce87653ea453fcd731a2a7d580d0a8546563d3e0` |

One more file was opened on 28 Oct, because pull #1160 names it. `mining/src/mempool/model/utxo_set.rs` blob `ea439e36c5e86a119e87929f80a8e43f37d4eb39` is the same on the tag and on `4edcbbcc`. The `add_transaction` loop is quoted in [HANDOFF.md](HANDOFF.md).

`bps.rs` and the finality function were compared on 27 Oct. That comparison is not repeated here. The Poisson tail was not recomputed. The 27 Oct digits remain P(X > 123) = 0.011243536494169537 and P(X > 124) = 0.008773598988627285. The consensus-papers cell still rounds these to 0.011244 and 0.008774.

## Issues re-fetched on 28 Oct

| Item | State at this fetch |
| --- | --- |
| rusty-kaspa #1146 | open, `updatedAt` 2026-10-13T10:48:54Z |
| rusty-kaspa #1147 | open, not merged, head `239556a60db4de0233b079a9a6611a543eccbe61`, `updatedAt` 2026-10-15T05:41:42Z |
| rusty-kaspa #1073 | open, `updatedAt` 2026-10-13T13:44:49Z |
| kips #51 | open, `updatedAt` 2026-10-23T16:12:43Z |

## Pulls counted on 28 Oct

`master` has no merge after #1065 (`4edcbbcc`, 19 Oct). #1154 and #1153 merged on 19 Oct and are inside that tip. #1060, Maxim, "Consensus core nostd", merged 16 Oct and is inside that tip. The `bps.rs` gate was not re-diffed against #1060.

Still open, and not a change to the five blobs:

- #1152, biryukovmaxim, covenant-id UTXO index. Head `6a68b2917b7c022185d3975ea9c8178247aacae0`. Updated 26 Oct. 42 files. None of the five. `CHANGES_REQUESTED` by IzioDev on 21 Oct. The Covenant-id cell already names this head.
- #1160, parthod0x, `utxo_set.rs` only. The function was read. See the handoff.
- #1156, supertypo, `reachability.rs` and `database/src/set_access.rs`. Not `processor.rs`.
- #1161, #1159, #1158, #1157, #1150, #1141, and the older open Maxim pulls were listed. Their diffs were not read. None of them is merged.

## Opened, and not used for a sentence

- [grok-reasoning](https://github.com/STP-KAS/kaspa-master-file/tree/build/grok-reasoning-2026-10-15/grok-reasoning). Branch tip `f424e55751149e3c8adf982f4dd476afcd55887c` (15 Oct 2026). Not re-read. The 27 Oct exclusion stands.
- [kaspa-builders](https://github.com/STP-KAS/kaspa-builders) compare `c012d124...4e725c60`: two commits, files `entries/krc20-mechanics.md`, `entries/rossku-kob.md`, `entries/wallets-kcc20.md`, `builders.json`, `SNAPSHOT-HISTORY.md`. A code search for `1146`, `finality`, and `mempool panic` returned no file.

## Not done

- No kaspad process was started. The 25 Sep panic was not repeated. The 1,644 sompi/gram quote was not measured. The #1160 test was not run.
- Subsidy division, duplicate-input rejection, and the coinbase hash check were not re-opened. They are not in the handoff.
- The txscript opcode surface, the pruning-proof validator, the DAGKnight branch, SilverScript, and the KCC repos were not re-audited. The sweep's KGI, vProgs, Argent, KaspaDollar, and Referee-lag rewrites were named and not reviewed as core hand-offs.
- No pull request was opened on kaspanet, on kips, or on the pin board.
- The local kaspa-master-file checkout was left on `build/rusty-1074-2026-10-14`. `origin/main` was fetched. The worktree was not checked out onto `main`.
