# Decision log

One file per decision that crosses repositories, `G-NNNN-<slug>.md`, numbered in the order
they were recorded, never renumbered, never deleted. Repository-internal decisions stay in
that repository's log (Kernel numbers its own in `docs/PHASE2.md` and `docs/ZERO_UNSAFE.md`;
Userland records its in `docs/PROGRAM.md` and its rules); a repository cites a decision here
as `decision G-0007`.

The first ten were recovered on 2026-10-31 from the repositories' rule files, where they had
been recorded as "the user's decision of <date>"; the source lines are cited in each. The
date is the day the decision was taken, not the day it was written here.

## Format

```
# G-NNNN: <title>

Date: YYYY-MM-DD · Status: proposed | accepted | superseded by G-MMMM · Binds: <repositories>

## Context      what was the question, and the options
## Decision     what was decided, by the maintainer, in one or two sentences
## Consequences what it changes, what it rules out
## Sources      where it was first recorded, or the conversation
```

## Index

| Id | Date | Status | Title |
|---|---|---|---|
| G-0001 | 2026-10-07 | accepted | Three stages: the port, the native system, the userland |
| G-0002 | 2026-10-09 | accepted | Kernel splits from LandingZone; LandingZone never learns of it |
| G-0003 | 2026-10-09 | accepted | Authorship: the author's ISC block first, every original notice kept whole |
| G-0004 | 2026-10-10 | accepted | A session drives the work and launches the roles one by one; no batch workflow |
| G-0005 | 2026-10-16 | accepted | Fable plans and designs as a subagent; the main session stays on Opus |
| G-0006 | 2026-10-16 | accepted | `unsafe` is confined to one named crate per repository, counted and capped |
| G-0007 | 2026-10-18 | accepted | arm64 first; amd64 frozen until N10 |
| G-0008 | 2026-10-22 | accepted | The repositories are named for what they hold, under the EmiBSD organisation |
| G-0009 | 2026-10-23 | accepted | The userland is written from scratch; two classes; LLVM and Rust imported, nothing GPL |
| G-0010 | 2026-10-25 | accepted | The reference is read to write the specification and to settle gaps, never copied |
| G-0011 | 2026-10-31 | accepted | A Governance repository holds what crosses repositories |
| G-0012 | 2026-10-31 | proposed | DevTools and OS as repositories |
| G-0013 | 2026-10-31 | proposed | Userland inherits OpenBSD's C in our layout; the Rust rewrite moves to `experimental` |
| G-0014 | 2026-10-31 | proposed | LandingZone may name what grows from it, in its README only |
| G-0015 | 2026-10-31 | accepted | One LLVM, upstream's, from the fork; nothing of OpenBSD in DevTools |
| G-0016 | 2026-10-31 | accepted | EmiBSD's own target triple, `aarch64-unknown-emibsd`; the toolchains carry it as patches of ours |
