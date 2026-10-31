# G-0010: The reference is read to write the specification and to settle gaps, never copied

Date: 2026-10-25 · Status: accepted · Binds: Userland

## Context
A written program must behave as OpenBSD's does, byte for byte to a pipe, while owing it
nothing in fact or appearance. The rule of 2026-10-24 let only the surveyor and the designer
read the reference; implementers hit gaps the specification had not settled.

## Decision
The reference manual and code are read to write a program's DEFINITION (what each parameter
does, how the output is formatted, the exit codes) and, while implementing, to settle what the
DEFINITION leaves open and to check a behaviour. The behaviour is taken; the structure and the
text never: nothing is copied or translated, a gap settled this way becomes a DEFINITION line,
and nothing in the repository names the reference system for a written program.

## Consequences
The reviewer checks for copied structure; the reference is read with `git show`, never from
inside the tree; the guard hook refuses writes and `cd` into it.

## Sources
`Userland/CLAUDE.md`; `Userland/.claude/rules/rust.md`, "From the specification"; `subagents.md`, Setup in a worktree (replacing the rule of 2026-10-24).
