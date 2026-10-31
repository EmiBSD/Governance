# G-0008: The repositories are named for what they hold, under the EmiBSD organisation

Date: 2026-10-22 · Status: accepted · Binds: every repository, Website

## Context
The repositories lived under a personal account as `EmiBSD` and `EmiBSD.LZ`; the names said
whose they were, not what they held.

## Decision
The organisation `EmiBSD` holds them. The port is `LandingZone` (the stage keeps the name
EmiBSD.LZ), the native system is `Kernel`, the userland `Userland`, the site `Portal`, renamed `Website` on 2026-10-31 (in English "portal" suggests a gateway or a login page, not a static site; `Website` says what it holds). The forks
keep their upstream names. GitHub redirects the old URLs; `lz/PINNED.md` and every link follow.

## Consequences
The site's clone is `Website/` in the workspace; the old URL redirects. A new repository is named for its content: hence DevTools and OS as proposals (G-0012), and
Governance (G-0011).

## Sources
`Kernel/docs/PHASE2.md`, "Decisions", 2026-10-22.
