> **Experimental. This page does not claim the truth.** A sentence here is a reading of public files on 28 Oct 2026. It can be wrong. [What would make a sentence wrong](HANDOFF.md) is part of the sentence.

# Kaspa core note

## What

The 27 Oct list was checked again on 28 Oct 2026, after the day's pin-board sweep, because core had pushed more and because Maxim Biryukov's rusty-kaspa work had to be counted.

The node tip did not move. [kaspanet/rusty-kaspa](https://github.com/kaspanet/rusty-kaspa) `master` is still `4edcbbccb449f84832afb1ca157904b35f37d006` (19 Oct 2026, 15:05:13Z). The repo `pushedAt` is that same time. The release is still v2.1.0, commit `01b532e8b553523216471682649693af92f0fd16` (published 22 Sep 2026). The five source blobs named in [SCOPE.md](SCOPE.md) are the same objects as on 27 Oct, because they sit in that same commit.

The pin board did move. [STP-KAS/kaspa-master-file](https://github.com/STP-KAS/kaspa-master-file) `main` is `40bdc9811f92e4db11e1e78c218f66d94fb8738d` (28 Oct 2026, 07:06:31Z), linking the sweep `b6d26ce`. The sweep rewrote six cells. It left the Live, consensus-papers, Covenant-id, KIPs, and Not-live cells in place. Its snapshot row says the rusty-kaspa release pin holds at v2.1.0.

[HANDOFF.md](HANDOFF.md) is the list this re-read would still hand to Kaspa core. One 27 Oct sentence was wrong: the KIPs cell already quotes the KIP-21 mismatch. That item is no longer handed as an unchecked finding.

No issue, review, or comment was opened on kaspanet.

## Why

A hand-off written against the 19 Oct tip is false if a later master commit changes the file, or if the morning sweep already prints the sentence. Both had to be fetched before any 27 Oct line was kept.

Maxim's name is on a large open pull, [#1152](https://github.com/kaspanet/rusty-kaspa/pull/1152), and on the 16 Oct merge that is already inside `4edcbbcc`. Neither one changes the six source questions. The open pull was already on the Covenant-id cell. Counting his commits, and then leaving the sentences as they were, is the result.

## How

1. Fetched `kaspanet/rusty-kaspa` `master`, its `pushedAt`, and the latest release.
2. Listed open and recent pulls, including every pull by `biryukovmaxim`. Read the file list of #1152, #1160, and #1156. Read `MempoolUtxoSet::add_transaction` at master for #1160.
3. Fetched kaspa-master-file `main` and diffed it against `b8371b7`. Read the six rewritten cells far enough to name them, and the surviving cells far enough to confirm the pins this note uses.
4. Re-fetched rusty-kaspa #1146, #1147, and #1073, and kips #51. Fetched kips `master` and tn-master `main`.
5. Looked at the 28 Oct kaspa-builders sweep files and searched that repo for `1146`, `finality`, and `mempool panic`. Confirmed the grok-reasoning branch tip had not moved since 15 Oct.

No node was started. No transaction was sent. The Poisson tail was not recomputed. The 25 Sep panic was not run again. The fee quote of 1,644 sompi/gram was not measured again. `bps.rs`, `processor.rs`, `bounds.rs`, the mempool assert file, and the relay file were not downloaded again: their commit is unchanged.

## Where the sentences are

| Page | What it is |
| --- | --- |
| [HANDOFF.md](HANDOFF.md) | What is still worth handing, what the 28 Oct fetch changed, and what would make each sentence wrong |
| [SCOPE.md](SCOPE.md) | Commits, blob ids, pulls, and what this pass did not do |

---

> Experimental. Not advice. Not a product. Not an oracle.
>
> Intentions are good; thought process is questionable. STP remains delusional. Si vis pacem, para bellum.
>
> X: https://x.com/StppStp · GitHub: https://github.com/STP-KAS
