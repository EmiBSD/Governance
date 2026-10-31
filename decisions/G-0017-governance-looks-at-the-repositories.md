# G-0017: Governance looks at the repositories; no repository looks at Governance

Date: 2026-10-31 · Status: proposed · Binds: every repository, this one first

## Context
G-0011 made this repository the home of what crosses repositories and had each repository
cite a decision as `G-NNNN`, with a link to Governance in its README. In practice an agent
working inside a repository followed the citation, landed here, and found the whole
solution: every other repository, the forks, the flows, the plans. It lost its footing in its
own task ("se mareaban enteros"). DevTools was the first to cut the relation on its own
(its D-0005: it carries every rule that binds it and cites no other repository's files); OS
and Userland were scrubbed of every Governance link and G-number the same day. The question
was whether Governance keeps binding by citation or by another means.

## Decision
The maintainer, 2026-10-31: the relation is one-way. Governance reads the repositories and
describes them; a repository never reads, links or cites Governance. A decision taken here
reaches a repository as text of that repository's own (its `CLAUDE.md`, its rules, its own
decision log, its README), self-contained, with no pointer back. Each repository
concentrates on its task and knows of the others only what its own task needs. Nothing is
injected into a repository without the maintainer's explicit OK, decision by decision.

## Consequences
- The `Binds:` line of a decision names the repositories; the decision itself stays here.
  What reaches them is a rewording in their own terms, proposed to the maintainer first.
- No repository's `CLAUDE.md`, rules, README, manifests or docs name Governance or a `G-NNNN`.
  A repository that wants its own log numbers it its own way (DevTools: `D-NNNN`).
- The README's "Where it sits" sentence "the whole picture is in Governance" goes from every
  repository; the diagram and the text of each describe only the repository's own neighbours.
- `docs/architecture.md`, `docs/roadmap.md` and the repository table of `CLAUDE.md` are
  Governance's view of the repositories and are kept current by reading them, not the
  reverse. When a repository and Governance disagree, the repository is right and Governance
  is updated.
- G-0011's rule "each cites a decision as `G-NNNN`" is reversed by this decision; the
  repository, the log and the rest of G-0011 stand.
- A session that works inside a repository starts from that repository's root and reads its
  files alone. Only a session started here reads everything.

## Sources
The maintainer, 2026-10-31: "governance tiene que ver a los repos e inyectar decisiones en
ellos ... los repos se concentren en sus tareas"; DevTools' D-0005; the scrub of OS and
Userland the same day.
