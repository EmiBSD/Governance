# G-0009: The userland is written from scratch; two classes; LLVM and Rust imported, nothing GPL

Date: 2026-10-23 · Status: accepted; the two-LLVMs clause superseded by G-0015 · Binds: Userland, DevTools, OS

## Context
With the kernel running OpenBSD's userland unmodified, the question was whether EmiBSD's own
userland would be a port of OpenBSD's C, as the kernel was, or something of its own.

## Decision
"No vamos a migrar": the userland is written from scratch in Rust under one contract
(`docs/PROGRAM.md`), never a port or a translation. Two classes: *written* (from a
specification, `#![forbid(unsafe_code)]`) and *imported* (cryptography, the network daemons,
the compilers: compiled from a pinned upstream, patched, never rewritten). The toolchains are
imported: LLVM as OpenBSD ships it for the C, rust-lang/rust at a pinned stable tag built with
its own LLVM submodule (two LLVMs), so the `*-unknown-openbsd` targets get their `std` from a
stable release without nightly. Nothing GPL enters the tree. (The toolchain clause is
superseded by G-0015: one LLVM, upstream's, from the fork; nothing of OpenBSD in DevTools.)

## Consequences
`catastro.md` lists every program and its class; the forks `rust`, `llvm-project`, `cargo` and
`OpenBSD-src` are the mirrors the pins name; `policies/licences.md`.

## Sources
`Userland/CLAUDE.md`; `Userland/README.md`, Rules; `Userland/compiler/README.md` and the two `UPSTREAM.toml`.
