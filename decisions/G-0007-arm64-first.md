# G-0007: arm64 first; amd64 frozen until N10

Date: 2026-10-18 · Status: accepted · Binds: Kernel, Userland, DevTools, OS

## Context
Keeping two architectures green through the redesign doubled every slice's cost while the
design questions were the same on both.

## Decision
Only arm64 is built, booted and measured until N10 homologates amd64 (Kernel's decision 44). No
amd64 code is written or fixed before then; every `hal` boundary type is still designed with
the amd64 C in view.

## Consequences
`EMIBSD_ARCHS` defaults to arm64; the userland and the toolchain target `aarch64` first; the
first releases are arm64 only.

## Sources
`Kernel/CLAUDE.md`, Non-negotiables; `Kernel/docs/PHASE2.md`, "Decisions", 2026-10-18.
