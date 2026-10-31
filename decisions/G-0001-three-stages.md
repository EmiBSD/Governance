# G-0001: Three stages: the port, the native system, the userland

Date: 2026-10-07 · Status: accepted · Binds: LandingZone, Kernel, Userland

## Context
OpenBSD's kernel could be carried into Rust as one rewrite, or as a faithful port that a
redesign then grows from. A rewrite has no specification to check against; a port that is also
redesigned loses its fidelity.

## Decision
Three stages: OpenBSD, then the faithful file-by-file port (LandingZone, continuous), then the
native system redesigned from it (Kernel). The userland of EmiBSD's own came later as a third
phase (G-0009).

## Consequences
The port keeps OpenBSD's structure and names and never redesigns; the redesign never ports.
Each stage is its own repository with its own rules.

## Sources
`Kernel/docs/PHASE2.md`, "Decisions", 2026-10-07.
