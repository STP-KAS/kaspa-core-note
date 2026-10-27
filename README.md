> **Experimental. This page does not claim the truth.** A sentence here is a reading of public files on 27 Oct 2026. It can be wrong. [What would make a sentence wrong](HANDOFF.md) is part of the sentence.

# Kaspa core note

## What

Six sentences from a 26 Oct 2026 read of rusty-kaspa **v2.1.0** (`01b532e8b553523216471682649693af92f0fd16`), checked again on 27 Oct 2026 against three public sources:

- The pin board, [STP-KAS/kaspa-master-file](https://github.com/STP-KAS/kaspa-master-file) `main` `b8371b72eb61e2d09ff50cb7e6f5cb885b06e240` (27 Oct 2026, 08:02 +0100).
- The TN10 map, [STP-KAS/tn-master](https://github.com/STP-KAS/tn-master) `main` `153913aea045` (15 Oct 2026). Testnet-10 only. tKAS has no value.
- The node files named in [SCOPE.md](SCOPE.md), at the v2.1.0 tag and at master `4edcbbccb449f84832afb1ca157904b35f37d006` (19 Oct 2026). That master commit was still the tip on 27 Oct 2026. The release tag was still v2.1.0.

[HANDOFF.md](HANDOFF.md) is the list this re-read would still hand to Kaspa core, and the 26 Oct sentences it would withdraw.

No issue, review, or comment was opened on kaspanet.

## Why

The 26 Oct draft treated several sentences as new. The pin board already prints the Poisson tail, the second term of the paper's f, the parent cap of 16, the 12-hour finality depth, and the sub-millisecond throttle. tn-master already reports the mempool crash and already tells builders to cap fees and to read more than one node. Handing those again, as if they had just been found, would be a worse note.

What is left is smaller: one comment in the node that the pin board does not quote, one assert that tn-master already saw crash a TN10 node and that is still in the release, one KIP sentence the pin board had not checked against the code, and two open mempool pulls.

## How

1. Read the Now board on kaspa-master-file `b8371b7`, cell by cell, and marked every place it already says one of the six sentences.
2. Read tn-master `README.md`, `WHAT-WHY-HOW.md`, `FINDINGS.md`, `ADVICE-TO-DEVELOPERS.md`, `docs/LESSONS.md`, and `docs/OPEN-QUESTIONS.md` at `153913ae`.
3. Compared GitHub blob ids for five source files between tag v2.1.0 and master `4edcbbcc`. Where a blob differed, read the diff.
4. Recomputed the Poisson tail at mean 100. P(X > 123) = 0.011244. P(X > 124) = 0.008774. Same figures as the pin board's consensus-papers cell.
5. Re-read kips issue #51, rusty-kaspa issue #1146, pull #1147, and issue #1073. All four were still open.
6. Opened `grok-reasoning/README.md` on `build/grok-reasoning-2026-10-15`. Searched kaspa-builders for `1146`, `finality`, and `mempool panic`. Neither changed a sentence. Detail is in [SCOPE.md](SCOPE.md).

No node was started. No transaction was sent. The 25 Sep panic was not run again. The fee quote of 1,644 sompi/gram was not measured again.

## Where the sentences are

| Page | What it is |
| --- | --- |
| [HANDOFF.md](HANDOFF.md) | What is still worth handing, what the 26 Oct draft overstated, and what would make each sentence wrong |
| [SCOPE.md](SCOPE.md) | Files opened, blob ids, and what this pass did not do |

---

> Experimental. Not advice. Not a product. Not an oracle.
>
> Intentions are good; thought process is questionable. STP remains delusional. Si vis pacem, para bellum.
>
> X: https://x.com/StppStp · GitHub: https://github.com/STP-KAS
