# G-0003: Authorship: the author's ISC block first, every original notice kept whole

Date: 2026-10-09 · Status: accepted · Binds: every repository

## Context
Derived files carry OpenBSD's copyright notices; the project's own work needed an author and a
licence that stay with the file through later migrations and derivative works.

## Decision
Every source file starts its licence zone with the author's ISC block, "Copyright (c) 2026
Emilio Navarrete Lineros <enavarre@outlook.com>", first, before the original notices, which are
kept whole. New code is ISC. Nobody else's name replaces it.

## Consequences
LandingZone and Kernel check the `<LICENSES>` zone of every `.rs`; Userland copies the header
of `lib/crates/cli/src/lib.rs` into every file. `policies/licences.md`.

## Sources
`Kernel/docs/PHASE2.md`, "Decisions", 2026-10-09; `LandingZone/CLAUDE.md`, Non-negotiables.
