# Invariants

The rules every EmiBSD repository obeys. A repository's own rules may be stricter, never
looser. Each entry says what, why, and where it is enforced; the compressed list is in
`CLAUDE.md`.

## 1. OpenBSD's behaviour is the specification

When the Rust differs from OpenBSD, the Rust is wrong. For the kernel the C under
`reference/openbsd-src` at the pin is the specification and is read-only; for the userland
OpenBSD's manuals and code are read to write a program's specification, and the program is
written from that specification (decision G-0010).

**Why:** the project's claim is a system that behaves as OpenBSD does, with the memory errors
ruled out by the compiler. A behaviour that drifts cannot be verified against anything.

**Enforced by:** `diff-openbsd` against a real OpenBSD at every milestone close; the smokes;
Userland's golden tests, byte for byte to a pipe.

## 2. What userland sees never changes

The system-call ABI, the `#[repr(C)]` structures it shares, sysctl names, ioctls, errno and
device numbers stay byte-identical to OpenBSD's. OpenBSD's own binaries run unmodified on
EmiBSD's kernel; the `abi` crate is translation only.

**Why:** it is the one contract that lets a real userland prove the kernel, and the base system
EmiBSD runs until its own userland exists.

**Enforced by:** the smokes booting OpenBSD's userland; Kernel's `.claude/rules/lz-sync.md`;
additions arrive only through `lz-sync:` commits.

## 3. Nothing copied, every notice kept, new code ISC

C is never copied verbatim. A file derived from OpenBSD keeps the original copyright and
licence notices whole, preceded by the author's ISC block (decision G-0003). A written
userland program comes from its specification and owes nothing in fact or in appearance.
New code is ISC. Nothing GPL enters the tree, not even a build tool that links
(`policies/licences.md`).

**Why:** the derivative works are honest about their origin; the written ones are free of it;
the licence stays one that OpenBSD's own community accepts.

**Enforced by:** the `<LICENSES>` zone checks in LandingZone and Kernel; Userland's reviewer;
the licence allowlist.

## 4. `unsafe` is counted, capped and confined

Every repository counts its `unsafe` with tooling (`cargo xtask unsafe-report` in Kernel) and
caps it with budgets only the maintainer raises. It lives in one named place per repository:
`hal` in Kernel; `lib/crates/sys`, `lib/libc` and `libexec/rtld` in Userland. Every site carries
a `// SAFETY:` proof. Progress is measured first as `unsafe` removed (decision G-0006).

**Why:** the thesis is that the compiler, not a reviewer, rules out memory errors; that is
only true where `#![forbid(unsafe_code)]` holds, so the exceptions must be few, listed and
proven.

**Enforced by:** `#![forbid(unsafe_code)]` at the crate roots; `unsafe-report --check` in CI;
the Status page publishes the figures.

## 5. Stable Rust, pinned; nothing added without the maintainer

Stable toolchain only, pinned in each `rust-toolchain.toml`; a bump is its own commit and the
maintainer's decision. No nightly features, ever. A new crate dependency, a download, an
install, a tool outside the repository: the maintainer's OK first (`policies/dependencies.md`).

**Why:** reproducibility, and a dependency tree the maintainer has read.

**Enforced by:** the agent guard hooks (`cargo install`, `rustup`, `curl` refused); the
dependency allowlists; `Cargo.lock` committed.

## 6. Green `just ci` is done; milestones close with the smokes and `diff-openbsd`

A change is done when the repository's `just ci` is green. A milestone is met when the
scripted QEMU boots pass and the comparison with a real OpenBSD shows no difference the
milestone left open.

**Why:** "it looks right" is not evidence; the project is judged by its checks.

## 7. The repositories read in one direction

OpenBSD feeds LandingZone, Userland and DevTools. LandingZone feeds Kernel; Kernel reads it
and never writes to it, and nothing in LandingZone's code, rules or agents names Kernel or
adapts to it. Its README alone may say what grows from it (G-0002, relaxed in that one point
on 2026-10-31). DevTools feeds LandingZone, Kernel, Userland and OS and knows nothing of their internals: it
takes a sysroot as an input. Kernel and Userland feed OS (`docs/architecture.md`).

**Why:** the port stays a faithful port, checkable against the C alone; the toolchain stays
reusable; a cycle would make the first self-hosted build impossible to explain.

## 8. Not affiliated with OpenBSD

EmiBSD is not affiliated with or endorsed by the OpenBSD project, and says so wherever it
presents itself: every README, the site, every post.

## 9. English inside, Spanish to the maintainer

Everything inside a repository (code, comments, docs, commits) is in English. The maintainer
is answered in Spanish.
