# EmiBSD Governance (`EmiBSD/Governance`)

`Governance/` is the clone of **`EmiBSD/Governance`**: what binds the rest. The invariants
every repository obeys, the process, the architecture of the repositories, the roadmap across
them, the decision log and the policies. Nothing else: what happens inside a repository is
that repository's, written in its own files. The directory above this clone is the workspace
that holds every clone side by side; it is not a repository and has no rules of its own.

EmiBSD is an experiment by Emilio Navarrete Lineros (enavarre@outlook.com): the OpenBSD
kernel carried into Rust, first as a faithful file-by-file port and then redesigned so that
the compiler, not a reviewer, rules out memory errors; then a userland of its own. The code is
written by Claude Code agents under written rules; the maintainer takes every decision and
approves every commit. Pre-alpha: boots only in QEMU on arm64 and runs OpenBSD's own
userland unmodified. Public site: https://www.emibsd.org. GitHub: https://github.com/EmiBSD.

The user is answered in **Spanish**. Everything inside the repositories, this one included
(code, comments, docs, commits), is in **English**.

## What this repository holds

| Path | What |
|---|---|
| `docs/invariants.md` | The rules every repository obeys, with their reasons and sources. |
| `docs/process.md` | How the work is done: agents write, the maintainer decides and approves. |
| `docs/architecture.md` | The repositories, the forks and the flows between them, as a diagram. |
| `docs/roadmap.md` | The milestones of every repository on one page, read from the repositories. |
| `decisions/` | The decision log: one file per decision that crosses repositories, `G-NNNN`, numbered and dated. |
| `policies/` | Licences, dependencies, the disclosure of the AI process. |
| `CLAUDE.md`, `.claude/` | This file and the agent settings; `settings.local.json` is personal and ignored. |

What is decided here binds the other repositories. Each repository's own `CLAUDE.md`,
rules and README are the rules inside it and may be stricter, never looser. A repository
does not read, link or cite this one: a decision reaches it as text of its own, carried
there only on the maintainer's explicit OK (G-0017, proposed). DevTools keeps its own
log, `docs/DECISIONS.md`, numbered `D-NNNN`.

## The repositories

The clones sit next to `Governance/` in the workspace, each with its own git and its own
rules. A session that works in one starts from its root and reads its files alone; only a
session started here reads across them. `docs/architecture.md` has the flows.

| Folder | GitHub | One line |
|---|---|---|
| `LandingZone/` | `EmiBSD/LandingZone` | Phase 1: the 1:1 port of the OpenBSD kernel to Rust, file by file. Milestones M. |
| `Kernel/` | `EmiBSD/Kernel` | Phase 2: the redesign of that port into `kernel`, `abi`, `hal` and the inherited `bsd`. Milestones N. |
| `Userland/` | `EmiBSD/Userland` | Phase 3: the base system above the kernel. Milestones U. |
| `DevTools/` | `EmiBSD/DevTools` | The toolchains, built from the forks and released as artifacts. Milestones T. |
| `OS/` | `EmiBSD/OS` | The assembly and the releases: a manifest of pins, the sets, the media, the `YEAR.MONTH` tag. Milestones R. |
| `Website/` | `EmiBSD/Website` | The public site www.emibsd.org. |

Forks in the organisation (`OpenBSD-src`, `rust`, `llvm-project`, `cargo` and the other
mirrors DevTools pins) are read-only: no commit of ours lands on them; our patches live in
the consuming repository.

## Loose files in the workspace

Beside the clones sit files of the maintainer's that belong to no repository (decks, external
reports, scans, credentials for the project's accounts). They are listed in
`../workspace.local.md`, which is not in any repository. None of them is ever committed,
read into a reply or sent anywhere; a credential file is never opened.

## Invariants, compressed (`docs/invariants.md` has the reasons)

- OpenBSD's behaviour is the specification. When the Rust differs from OpenBSD, the Rust is wrong.
- What userland sees never changes: the system-call ABI, sysctl names, ioctls and device numbers
  stay byte-identical to OpenBSD's.
- C is never copied verbatim; every derived file keeps the original copyright and licence
  notices whole, preceded by the author's ISC block. New code is ISC. Nothing GPL.
- `unsafe` is counted by tooling and capped by budgets only the maintainer raises; it lives in
  one named crate per repository. Progress is measured first as `unsafe` removed.
- Stable Rust only, pinned in each `rust-toolchain.toml`. New crate dependencies, downloads
  and installs need the maintainer's OK.
- Green `just ci` is the definition of done; milestones close with the scripted QEMU boots
  (the smokes) and `diff-openbsd` against a real OpenBSD.
- Kernel reads LandingZone, never the reverse: LandingZone's code, rules and agents never name
  Kernel or adapt to it; only its README may say what grows from it.
- EmiBSD is not affiliated with or endorsed by the OpenBSD project, and says so wherever it
  presents itself.

## Working here

- This repository has no build; its check is that every link and every cited decision
  resolves. Nothing here is edited from a session working in another repository.
- A decision that crosses repositories is a new file in `decisions/` (`decisions/README.md`
  has the format), proposed by the agent, accepted by the maintainer. A decision internal to
  one repository stays in that repository and is not recorded here. The maintainer is asked
  before any decision is written as accepted, before any status changes, and before anything
  is carried into another repository.
- When this repository and a repository disagree about that repository, the repository is
  right and this one is updated.
- Commit on the user's "dale", push on "súbelo"; never push otherwise. Commits:
  `<area>: <imperative summary>`, the body says what and why, then the session's
  `Co-Authored-By:` line.
- If the project is ever announced on the Rust users forum (users.rust-lang.org), the post is
  written by the user, not generated; it leads with the verification (smokes, `diff-openbsd`,
  the `unsafe` report), states the AI process in its first lines, and asks a concrete technical
  question. Its rules treat pasted LLM text as spam, and its readers judge AI projects by their
  checks, not their size.
