# Compute Substrate anchors

Public records of what was published, and when, written to
[Compute Substrate](https://computesubstrate.org). An anchor proves that exact bytes
existed by a given block height. It says nothing about whether the content is correct.

## openai/math

On October 6, 2026, OpenAI published AI-generated mathematics manuscripts at
[github.com/openai/math](https://github.com/openai/math) and revised the repository
on October 7. We hashed every tracked file of the current release and recorded the
hashes on Compute Substrate: one record per manuscript, plus one root record that
commits to all 133,955 files.

| | |
|---|---|
| Commit | [`fd4aeeb2ee4f`](https://github.com/openai/math/tree/fd4aeeb2ee4fc729c18d98444fed42fd0529eeeb) (2026-10-07) |
| Manifest | [`openai-math/manifest-2026-10-07.json`](openai-math/manifest-2026-10-07.json) |
| Root payload hash | `0x19d9f7945074ac0e71b4864b4a00fb5e097b952677e52586bd37db121479a1bb` |
| Root proposal | `ROOT_TXID` |
| Block heights | `HEIGHT_RANGE` |
| Domain | `math/openai-math` |
| Records | 758 (757 units: 749 manuscript directories + 8 top-level items, plus 1 root) |
| Per-paper proposal IDs | [`openai-math/anchor-state.json`](openai-math/anchor-state.json) |

The first public release (`adc7f1241b42`, 2026-10-06) is anchored by its root
record: `ROOT_TXID_1006` ([manifest](openai-math/manifest-2026-10-06.json)). Its
manifest holds the hash of every original manuscript, including the three that were
later withdrawn.

### What changed between the two releases

[`diff-10-06-to-10-07.json`](openai-math/diff-10-06-to-10-07.json):

- 719 of the 722 original manuscripts are byte-identical
- 27 revised editions were added as new directories next to the originals they revise
- 3 manuscripts were withdrawn. Their directories now hold only a withdrawal notice;
  the original files were removed from the current version, and the notices link to
  them in the October 6 commit
- `history.md` was added; `README.md`, `CONTENTS.md`, `overview.*` and `lean/` were updated

This matches OpenAI's own [history.md](https://github.com/openai/math/blob/main/history.md).
The withdrawn originals now live only in git history, which the repository owner
controls. The October 6 anchor records their exact hashes, so anyone holding a copy
can prove it is the original.

### Verify it yourself

You need git and Python 3, plus a Compute Substrate node for the on-chain step.

```bash
git clone https://github.com/openai/math.git
curl -LO https://raw.githubusercontent.com/GITHUB_OWNER/compute-substrate-anchors/main/openai-math/manifest-2026-10-07.json
curl -LO https://raw.githubusercontent.com/GITHUB_OWNER/compute-substrate/main/anchor/verify_manifest.py
python3 verify_manifest.py --repo math --manifest manifest-2026-10-07.json
```

It recomputes every hash from git and prints the root payload hash. Then check that
hash against the chain:

```bash
curl -s http://<any-node>:8789/proposal/ROOT_TXID
```

The `payload_hash` field should equal the root payload hash. The hash spec is in
the [anchor README](https://github.com/GITHUB_OWNER/compute-substrate/blob/main/anchor/README.md).

### Respond to a specific manuscript

Every manuscript has its own proposal ID in
[`openai-math/anchor-state.json`](openai-math/anchor-state.json).

**Support it: attest.** An attestation carries no text. It is a support signal
(score and confidence), weighted by the fee paid:

```bash
csd attest --proposal-id <id> --score <n> --confidence <n> --fee 5000000 --auto-input
```

**Review, critique or check it: propose.** Write up your review, gap, counterexample
or Lean result, publish it anywhere, and record its hash and link as a new proposal.
State in the write-up which manuscript proposal ID it responds to:

```bash
csd propose --domain math/openai-math/review --payload-hash <sha256d of your file> \
  --uri <link to your write-up> --fee 25000000 --auto-input
```

Your review is then fixed on chain in its own right, and others can attest to it.

Nothing recorded here can be removed, including by us. Compute Substrate does not
judge any of it; reading and weighing it is up to you.
