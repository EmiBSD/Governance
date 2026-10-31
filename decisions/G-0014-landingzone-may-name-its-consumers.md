# G-0014: LandingZone may name what grows from it, in its README only

Date: 2026-10-31 · Status: proposed · Binds: LandingZone, Kernel

## Context
G-0002 said LandingZone never learns that Kernel exists. The maintainer recalls why: at one
point LandingZone's agents began writing about Kernel, and the rule stopped the port from
bending towards the redesign. With every repository now carrying a "Where it sits" diagram
in its README, LandingZone would be the one hole in the picture, and the maintainer judges
the original danger past: the port's rules, agents and code never mention Kernel, and the
sync is Kernel's job alone.

## Decision
LandingZone's README may say what grows from it, Kernel and the rest of the pipeline, with a
link to Governance, in its "Where it sits" section. Nothing else changes: no file under
`sys/`, no rule in `.claude/`, no agent instruction, no commit message of LandingZone names
Kernel or adapts to it; Kernel reads LandingZone and never writes to it; provenance (`LZ:`
trailers, `lineage.toml`) stays Kernel's. An agent of LandingZone that finds itself reasoning
about Kernel stops and reports, as before.

## Consequences
G-0002 is superseded in this one point and stands in every other. `docs/invariants.md` 7 and
`CLAUDE.md` say "Kernel reads LandingZone, never the reverse; LandingZone's README may name
its consumers, nothing else in it may". The README section is the only place the name appears.

## Sources
The maintainer, 2026-10-31 ("quedó vieja esa regla, era porque en un momento LandingZone
empezó a escribir sobre Kernel"); G-0002.
