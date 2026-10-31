# EmiBSD workspace (`EmiBSD/Governance`)

`Governance/` is the clone of **`EmiBSD/Governance`**: the rules, decisions and plans that
govern every EmiBSD repository. The directory above it is the workspace that holds every
clone side by side; it is not a repository, and it reads this file and `.claude/` through the
symbolic links `../CLAUDE.md` and `../.claude`. EmiBSD is an
experiment by Emilio Navarrete Lineros (enavarre@outlook.com): the OpenBSD kernel carried into
Rust, first as a faithful file-by-file port and then redesigned so that the compiler, not a
reviewer, rules out memory errors; then a userland written from scratch under one contract.
The code is written by Claude Code agents under written rules; the maintainer takes every
decision and approves every commit. Pre-alpha: boots only in QEMU on arm64 and runs OpenBSD's
own userland unmodified. Public site: https://www.emibsd.org. GitHub: https://github.com/EmiBSD.

The user is answered in **Spanish**. Everything inside the repositories, this one included
(code, comments, docs, commits), is in **English**.

## What this repository holds

| Path | What |
|---|---|
| `CLAUDE.md`, `.claude/` | This file and the shared agent settings; `settings.local.json` is personal and ignored. |
| `README.md` | What EmiBSD is and the map of repositories, for people. |
| `docs/invariants.md` | The rules every repository obeys, with their reasons and sources. |
| `docs/process.md` | How the work is done: agents write, the maintainer decides and approves. |
| `docs/architecture.md` | The repositories, the forks and the flows between them, as diagrams; the DevTools and OS plan. |
| `docs/roadmap.md` | The milestones of every repository on one page. |
| `decisions/` | The decision log: one file per cross-repository decision, numbered and dated; repositories cite them as `decision G-0007`. |
| `policies/` | Licences, dependencies, the disclosure of the AI process. |

What is decided here binds the other repositories; each repository's own `CLAUDE.md`,
`.claude/rules/` and `README.md` are the rules inside it and may be stricter, never looser.

## The other clones, beside this one

The clones below sit next to `Governance/` in the workspace, each with its own git. Read a
repository's `CLAUDE.md` before touching it; its rules apply inside it.

| Folder | GitHub | What it is |
|---|---|---|
| `LandingZone/` | `EmiBSD/LandingZone` (formerly EmiBSD.LZ) | Phase 1: the 1:1 port of the OpenBSD kernel, one C file to one Rust file, keeping OpenBSD's structure and names. The C under `reference/openbsd-src/` is the specification and is read-only. Milestones M0..Mn. Keeps porting after Kernel split from it. |
| `Kernel/` | `EmiBSD/Kernel` | Phase 2: started from LandingZone's tree on 2026-10-09 and redesigns it subsystem by subsystem into four crates: `kernel` and `abi` (`#![forbid(unsafe_code)]`), `hal` (the only crate with `unsafe`, each site with a `// SAFETY:` proof) and `bsd` (the inherited tree, deleted at N9). Milestones N0..N12; `lineage.toml` records where every file and function came from. Every LandingZone commit is triaged here (`lz-sync:` commits). amd64 is frozen until N10; arm64 only. |
| `Userland/` | `EmiBSD/Userland` | Phase 3: EmiBSD's own userland, written from scratch in Rust under one contract (`docs/PROGRAM.md`): a program is a query over records with a schema, `-o` picks the renderer, `--request` takes JSON. Two classes: *written* (from scratch, never a port or translation) and *imported* (ssh, LibreSSL, the compilers, ... compiled from their upstream pin, never rewritten). `catastro.md` lists every program and its state. |
| `Portal/` | `EmiBSD/Portal` | The public site www.emibsd.org: plain Jekyll on GitHub Pages behind Cloudflare, no theme. `_data/status.json` holds the milestone and `unsafe` figures; `_data/news.json` is the devlog. |
| `DevTools/` | `EmiBSD/DevTools` | **Planned** (`docs/architecture.md`): the toolchains, clang/lld/libc++ from OpenBSD's LLVM and rustc/cargo/std from the pinned stable Rust, built once and released as artifacts the other repositories consume. Today its definitions live in `Userland/compiler/` and its only working build is Kernel's `just comp`. |
| `OS/` | `EmiBSD/OS` | **Planned**, created at the first release: the pins of Kernel, Userland and DevTools, the install sets, the installer and media, the smokes of the whole system, the `YEAR.MONTH` tag. |

Forks in the organisation (`OpenBSD-src`, `rust`, `llvm-project` from rust-lang, `cargo`) are
read-only mirrors: no commit of ours lands on them, only the upstream tags the pins name; our
patches live in the consuming repository's `patches/`.

## Loose files in the workspace

Beside the clones, not part of any repository, not to be committed anywhere: `presentaciones/` and `EmiBSD.pptx` (decks),
`documentacion/` (external reports), the OWASP ZAP scans of www.emibsd.org (see the
`portal-security-scans` memory), `rust-osdev-homepage/` (the newsletter fork). `zuliprc` is the API credential for
rust-osdev.zulipchat.com: never read its key into a reply, never commit it, never send it
anywhere. `renta4.zip` is unrelated to EmiBSD; leave it alone.

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
- LandingZone must never learn that Kernel exists: Kernel reads LandingZone, never the reverse.
- EmiBSD is not affiliated with or endorsed by the OpenBSD project, and says so wherever it
  presents itself.

## Working here

- Work inside one repository at a time, from its own root, with its own `just` recipes; the
  workspace root has no build, no tests and no git. This repository has no build either; its
  check is that every link and every cited decision resolves.
- A decision that crosses repositories is a new file in `decisions/` (`decisions/README.md`
  has the format), proposed by the agent, accepted by the maintainer, then cited from the
  repositories it binds. A repository-internal decision stays in that repository's log.
- Commit on the user's "dale", push on "súbelo"; never push otherwise. Commits:
  `<area>: <imperative summary>`, the body says what and why, then the session's
  `Co-Authored-By:` line.
- If the project is ever announced on the Rust users forum (users.rust-lang.org), the post is
  written by the user, not generated; it leads with the verification (smokes, `diff-openbsd`,
  the `unsafe` report), states the AI process in its first lines, and asks a concrete technical
  question. Its rules treat pasted LLM text as spam, and its readers judge AI projects by their
  checks, not their size.
