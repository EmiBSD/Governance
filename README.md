# EmiBSD Governance

<p align="center">
  <img src="docs/Ferffy.jpg" alt="Ferffy, the EmiBSD mascot: a centolla, the spiky orange crab of the southern seas, in front of a shield, above the EmiBSD name" width="380">
</p>

<p align="center"><em>Ferffy, the EmiBSD mascot: a <a href="https://en.wikipedia.org/wiki/Lithodes_santolla">centolla</a>, the child of Ferris (Rust's crab) and Puffy (OpenBSD's pufferfish).</em></p>

<p align="center"><strong>The rules, decisions and plans that bind every EmiBSD repository.</strong></p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-ISC-007ec6" alt="License: ISC"></a>
  <a href="decisions/"><img src="https://img.shields.io/badge/decisions-G--0001..G--0014-007ec6" alt="Decisions: G-0001 to G-0014"></a>
  <a href="https://www.emibsd.org"><img src="https://img.shields.io/badge/website-emibsd.org-9f9f9f" alt="Website: www.emibsd.org"></a>
</p>

The rules, decisions and plans that govern every repository of [EmiBSD](https://www.emibsd.org),
and the workspace that holds their clones.

EmiBSD is an experiment by Emilio Navarrete Lineros: the OpenBSD kernel carried into Rust,
first as a faithful file-by-file port ([LandingZone](https://github.com/EmiBSD/LandingZone)) and
then redesigned so that the compiler, not a reviewer, rules out memory errors
([Kernel](https://github.com/EmiBSD/Kernel)); then a userland written from scratch under one
contract ([Userland](https://github.com/EmiBSD/Userland)). The code is written by Claude Code
agents under written rules; the maintainer takes every decision and approves every commit.
Pre-alpha: boots only in QEMU on arm64 and runs OpenBSD's own userland unmodified.

EmiBSD is not affiliated with or endorsed by the OpenBSD project.

## The repositories

```mermaid
flowchart LR
    subgraph forks["Forks: read-only mirrors"]
        direction TB
        obsd["OpenBSD-src"]
        rust["rust"]
        llvmrs["llvm-project (rust-lang)"]
        cargo["cargo"]
    end
    subgraph repos["Repositories"]
        direction LR
        dev["DevTools (planned)"]
        lz["LandingZone"]
        ul["Userland"]
        k["Kernel"]
        os["OS (planned)"]
    end
    obsd --> dev
    rust --> dev
    llvmrs --> dev
    cargo --> dev
    obsd --> lz
    obsd --> ul
    lz --> k
    lz -. pin .-> ul
    dev --> ul
    dev --> k
    k --> os
    ul --> os
    dev --> os
```

[`docs/architecture.md`](docs/architecture.md) explains every arrow. [Website](https://github.com/EmiBSD/Website)
is the public site and [`.github`](https://github.com/EmiBSD/.github) holds the organisation
profile and the community health files (security policy, contributing).

## What is here

| Path | What |
|---|---|
| [`docs/invariants.md`](docs/invariants.md) | The rules every repository obeys, with their reasons. |
| [`docs/process.md`](docs/process.md) | How the work is done: agents write, the maintainer decides and approves. |
| [`docs/architecture.md`](docs/architecture.md) | The repositories, the forks and the flows between them; the DevTools and OS plan. |
| [`docs/roadmap.md`](docs/roadmap.md) | The milestones of every repository on one page. |
| [`decisions/`](decisions/) | The decision log: one file per cross-repository decision, numbered `G-NNNN` and dated. |
| [`policies/`](policies/) | [Licences](policies/licences.md), [dependencies](policies/dependencies.md), [AI disclosure](policies/ai-disclosure.md). |
| `CLAUDE.md`, `.claude/` | The instructions the agents read when working from this directory. |

## How a decision is taken

1. An agent, or the maintainer, finds a question that crosses repositories: a name, a boundary,
   a licence, a tool, a phase.
2. The agent writes it as a proposal in `decisions/` with the options and a recommendation,
   status `proposed`. It never takes the decision.
3. The maintainer decides, in conversation; the file records the decision, its date and its
   consequences, status `accepted`.
4. The repositories it binds cite it by number in their rules, and follow it.
5. A later decision that replaces it sets it to `superseded` and points forward; nothing is
   deleted from the log.

Repository-internal decisions stay in that repository's own log (Kernel's `docs/PHASE2.md`,
Userland's `docs/PROGRAM.md`); this log is for what crosses them.

## Licence

ISC, see [LICENSE](LICENSE).
