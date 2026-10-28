# This is `sailantis/hyperium-h3`, a fork of `hyperium/h3`

**It carries no diff against `hyperium/h3` @ `43745ad87e95b0c035ac9c547aad467303213425` as of
2026-10-28** — in the crate sources. The only additions are this file and one banner paragraph in
`README.md`; nothing under `h3/`, `h3-quinn/`, `h3-datagram/` or `h3-webtransport/` differs from
that commit. Re-verify with:

```console
$ git fetch upstream && git diff --stat upstream/master master -- h3 h3-quinn h3-datagram h3-webtransport
```

The command prints nothing when the claim holds.

It exists for one reason: to be a *build source* for Volucer if an `h3` fix is needed before it is
released upstream. Creating it costs nothing and keeps the upstream pull-request path open, which a
fork created in a hurry on the day of an incident would not.

## Rules this fork follows

1. **Upstream PR first, fork in parallel — same day.** The PR is the contribution. A fork with a
   fix and no PR is a *private* fork: it silently accumulates drift and helps nobody.
2. **We fork this repository, never a contributor's PR branch.** A PR branch can be force-pushed,
   rebased or deleted; a tag we control cannot.
3. **On adoption, delete the `[patch]` line — do not merge.** Once the fix is on crates.io this
   fork and upstream are the same code, so "merging back" is a no-op. The reconciliation step is
   deleting one line in Volucer's root `Cargo.toml` and bumping the version.
4. **A fork left patched after adoption is the drift we are avoiding.** `[patch.crates-io]` is
   added only while a diff is actually carried.

## Naming, fixed up front

- **Branches:** `volucer/<topic>` — one branch per carried fix, rebased on `upstream/master`.
- **Tags:** annotated, `volucer/<topic>-<date>`. Volucer pins a tag, never a branch, so a
  contributor's rebase cannot reach a build.
- **Baseline tags** mark a known-good upstream commit: `volucer/baseline-<short-sha>`.

## Remotes

| Remote     | Fetch                                    | Push     |
| ---------- | ---------------------------------------- | -------- |
| `origin`   | `https://github.com/sailantis/hyperium-h3` | yes      |
| `upstream` | `https://github.com/hyperium/h3`           | disabled |

`upstream`'s push URL is `DISABLED` deliberately: a stray `git push upstream` must fail rather
than attempt a write to a repository we do not own.

## What lives in this repository

`h3`, `h3-quinn`, `h3-datagram` and `h3-webtransport` are all members of this one repository —
`h3-quinn` is not a separate project. So one fork covers both crates Volucer depends on.

`h3-quinn` is pinned at `h3 = "^0.0.8"`, so **a patched `h3` of the same version resolves through
it untouched** — no `h3-quinn` fork is needed. Keep the two in lockstep if `h3`'s version ever
changes.

## Which commit to build from

`master` is **27 commits and 49 files ahead of the released `h3-v0.0.8` tag** (`22c1aa3`,
2025-05-06). If a patch ever pins this fork, decide explicitly which one it means:

- the `h3-v0.0.8` tag — byte-for-byte what crates.io serves, so the patch changes only our fix;
- `master` — also adopts everything upstream has merged since the release.

Prefer the tag for a minimal carried diff; prefer `master` only if a fix we need depends on
post-release work.

## Related documents

- Volucer's policy: `docs/carrying-an-upstream-patch.md` in `sailantis/volucer`.
- Security advisory this fork was created alongside: `hyperium/h3#379` (QPACK header
  amplification), fixed by `hyperium/h3#378`.
