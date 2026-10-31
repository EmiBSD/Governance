# G-0006: `unsafe` is confined to one named crate per repository, counted and capped

Date: 2026-10-16 · Status: accepted · Binds: Kernel, Userland

## Context
Version 1 of the zero-unsafe plan listed a core of modules allowed `unsafe`; the list grew and
`#[forbid]` per module was hard to see. The thesis needs a boundary the compiler enforces.

## Decision
Kernel: four crates, `kernel` and `abi` with `#![forbid(unsafe_code)]` at their roots, `hal` the
only crate with `unsafe`, each site with a `// SAFETY:` proof and a ceiling only the maintainer
raises, `bsd` the inherited tree deleted at N9 (Kernel's decisions 20-24). Userland: every
program and `cli` forbid; `unsafe` only in `lib/crates/sys`, `lib/libc` and `libexec/rtld`, each
site proven.

## Consequences
`cargo xtask unsafe-report --check` in CI; the figures on the Website's Status page; progress
measured first as `unsafe` removed. `docs/invariants.md`, 4.

## Sources
`Kernel/docs/ZERO_UNSAFE.md` section 0; `Kernel/CLAUDE.md`, Non-negotiables; `Userland/.claude/rules/rust.md`.
