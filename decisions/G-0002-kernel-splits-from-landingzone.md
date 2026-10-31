# G-0002: Kernel splits from LandingZone; LandingZone never learns of it

Date: 2026-10-09 · Status: accepted · Binds: LandingZone, Kernel

## Context
The port had reached M14 (installable). The redesign needed a tree to start from and a way to
keep receiving the port's work without the port bending towards it.

## Decision
Kernel starts from LandingZone's tree at `lz-origin` = `f5985f1d055a`, keeps
`reference/openbsd-src` at the same pin, and triages every later LandingZone commit as an
`lz-sync:` commit. LandingZone is never written to from Kernel and never names it.

## Consequences
The reading is one-way, Kernel reads LandingZone. `lineage.toml` records where every file and
function came from. The port's milestones (M) and the redesign's (N) are independent.

## Sources
`Kernel/docs/PHASE2.md`, "Decisions", 2026-10-07 and 2026-10-09; `Kernel/CLAUDE.md`, Non-negotiables.
