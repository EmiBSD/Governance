# G-0004: A session drives the work and launches the roles one by one; no batch workflow

Date: 2026-10-10 · Status: accepted · Binds: every repository

## Context
The first `/redesign` run used a batch workflow. It hid the plan, could not be steered, and
stopped in its Plan phase.

## Decision
The main session drives the work and launches each role with the Agent tool, one at a time,
seeing every plan and every commit; at most four agents run at once. A whole milestone in the
background gets a coordinator agent, never a Workflow.

## Consequences
Every repository's `.claude/rules/subagents.md` says so; the Workflow tool is not used.

## Sources
`Kernel/docs/PHASE2.md`, "Decisions", 2026-10-10; `Userland/.claude/rules/subagents.md`, Roles.
