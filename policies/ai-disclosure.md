# Disclosure of the AI process

EmiBSD's code, tests, documents and commit messages are written by Claude Code agents under
the written rules of each repository. The maintainer, Emilio Navarrete Lineros, takes every
decision, approves every commit in conversation and orders every push; nothing lands
otherwise. This is stated on the site, in every README and in every post about the project.

## In the repositories
- Every commit carries the session's `Co-Authored-By:` line naming the model.
- The rules the agents work under are public: `CLAUDE.md`, `.claude/rules/` and
  `.claude/agents/` in each repository, this repository for what crosses them.
- The agents' memories (`.claude/agent-memory/`) are committed with the trees.

## In public
- The verification comes first: the smokes, `diff-openbsd` against a real OpenBSD, the
  `unsafe` report. Readers judge an AI-written project by its checks, not its size.
- A post on a forum is written by the maintainer, never generated, states the process in its
  first lines, and asks a concrete technical question (the Rust users forum treats pasted LLM
  text as spam).
- Funding is sought only where the process described here is acceptable to the funder; where a
  policy excludes it, the project does not apply and does not argue.

## What the process does not claim
That the code is correct because a model wrote it. The claim is narrower: where
`#![forbid(unsafe_code)]` holds, the compiler rules out memory errors, and where it does not,
every site is listed, counted and proven.
