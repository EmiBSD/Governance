# G-0011: A Governance repository holds what crosses repositories

Date: 2026-10-31 · Status: accepted · Binds: every repository

## Context
The decision of 2026-10-07 put the governance files inside each repository. Since then the
cross-repository rules lived in a loose `CLAUDE.md` at the workspace root, outside any git, and
the decisions were scattered as "the user's decision of <date>" comments in four repositories.
The question was whether to version them, and where: the workspace root as a repository (it
holds four clones), a sibling folder, or the organisation's `.github` repository.

## Decision
A sibling repository, `EmiBSD/Governance`, cloned as `Governance/` beside the others. The
workspace root stays a plain folder and reads the repository's `CLAUDE.md` and `.claude/`
through symbolic links. It holds the invariants, the process, the architecture of the
repositories, the roadmap across them, this decision log and the policies. `.github` keeps the
organisation profile and the community health files; Website publishes.

## Consequences
What is decided here binds the other repositories; each cites a decision as `G-NNNN`. The
first ten entries of the log are recovered from the repositories' rule files. The repositories'
own logs stay for what is internal to them. Supersedes the 2026-10-07 wording "governance
files live inside each repository" for cross-repository matters only.

## Sources
This session, 2026-10-31.
