# Process

How EmiBSD is made. The repositories' own `.claude/rules/subagents.md` files hold the detail
for each; this page is what they share.

## Who does what

- **The maintainer** decides and approves. Every decision that shapes the system is theirs: a
  phase, a milestone's order, a name, a dependency, a licence, a toolchain bump, a budget of
  `unsafe`, anything that leaves OpenBSD's behaviour. Every commit is approved in conversation
  ("dale") and every push ordered ("súbelo"). Nothing is pushed otherwise.
- **The agents** (Claude Code, under each repository's rules) write the code, the tests, the
  documents and the commits. They propose, never decide: a question that is the maintainer's is
  handed back, not guessed. Each commit carries the session's `Co-Authored-By:` line.
- **The main session** drives the work and launches the roles one by one with the Agent tool,
  seeing every plan and every commit. There is no batch workflow (decision G-0004).

## The roles

Each repository defines its roles in `.claude/agents/` under one contract in
`.claude/rules/subagents.md`. The shape repeats: a read-only surveyor or reviewer, a planner
or designer that writes documents and never code, implementers in worktrees, a `mechanical`
role for the same change over many files, an integrator that merges. Design and planning run
on Fable as a subagent; the main session stays on Opus (decision G-0005). At most four agents
run at once.

Writing roles carry a guard hook that refuses `git push`, `--amend`, any force, any write under
a reference or upstream tree, and any install or download. A refusal is a stop to report, never
to route around.

## What a commit needs

- The repository's gate green: `just check` or `just ci` as the repository defines it.
- A message `<scope>: <imperative summary>`, a body that says what and why, the repository's
  trailers (`LZ:` and `Unsafe:` in Kernel, `Change:` in Userland), then `Co-Authored-By:`.
- Paths added by name; never `git add -A`, never amend, never a push.
- Every fixed bug leaves a test that failed before the fix, in the same commit.

## Milestones

Each repository numbers its own: M in LandingZone, N in Kernel, T proposed for DevTools, R for
OS releases (`docs/roadmap.md`). A milestone opens with a plan the maintainer approves, runs as
slices, and closes with the smokes, `diff-openbsd` and a tag; the Portal's Status page and the
devlog follow.

## Decisions

A decision that crosses repositories goes in `decisions/` (`decisions/README.md` has the
format); a repository-internal one stays in that repository's log. Repositories cite the
cross-repository ones by number.

## Reviews

One reviewer run per unit of work before its merge; findings go back to the same implementer;
a second review only when the first found something blocking or the work is security-sensitive.
The maintainer may also request external reviews (an independent model's report, a human) and
records what was adopted from them in the decision log.
