# G-0005: Fable plans and designs as a subagent; the main session stays on Opus

Date: 2026-10-16 · Status: accepted · Binds: every repository

## Context
Planning and design gain from the strongest model; running it as the main session is costly
and its context fills with the implementers' output.

## Decision
Fable is used for planning and design only, as a subagent (`planner` in Kernel, `designer` in
Userland); it writes documents, never code. The main session stays on Opus. Mechanical work
runs on Sonnet; when unsure whether work is mechanical or delicate, it is delicate.

## Consequences
Each role definition fixes its model and `effort` in its frontmatter, so a run never inherits
the main session's level.

## Sources
`Userland/.claude/rules/subagents.md`, Roles (the decisions of 2026-10-03, 2026-10-16, 2026-10-19).
