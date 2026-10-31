# G-0015: One LLVM, upstream's, from the fork; nothing of OpenBSD in DevTools

Date: 2026-10-31 · Status: accepted · Binds: DevTools, Userland, Kernel

## Decision
The maintainer, 2026-10-31, stated three times and recorded here as said: **DevTools takes
nothing from OpenBSD.** Not LLVM's code, not OpenBSD's patches to it, not OpenBSD's build
glue. DevTools' LLVM is upstream LLVM at the tag `llvmorg-22.1.6`, from the mirror
`EmiBSD/llvm-project`, built with LLVM's own CMake for the host and for the target
`aarch64-unknown-openbsd`, which upstream LLVM supports. One LLVM: it compiles the C and the
C++ of the system and it is the backend rustc 1.98.1 builds against (`llvm-config`; rustc
needs 21 or newer). `OpenBSD-src` is not an input of DevTools in any form.

## Context
The design of 2026-10-23 took "LLVM as OpenBSD ships it" from `gnu/llvm` inside the OpenBSD
tree, with OpenBSD's patches and BSD-make glue, and kept a second LLVM for rustc because
OpenBSD's was believed to be 18.1.8. At the pin it is 22.1.6. The agent then proposed, in
sequence, the fork's code with OpenBSD's patches and glue, then without the glue; each kept a
piece of OpenBSD in DevTools against the maintainer's instruction. This record replaces those
drafts.

## Consequences
- `rust/UPSTREAM.toml`'s `src/llvm-project` submodule pin is dropped: rustc builds against
  DevTools' LLVM. `llvm/UPSTREAM.toml` pins the mirror at the upstream tag with
  `origin = llvm/llvm-project`, no `path`, no patch series.
- OpenBSD's compiler changes (RETGUARD, `-fret-clean`, the reserved X18, BTI and PAC
  defaults) are not in this compiler. Whatever the base system's C needs of them is
  Userland's matter, where that C is built: flags adapted in its build, or, if the maintainer
  ever wants such hardening, EmiBSD's own commits on EmiBSD's fork, decided then. Not now,
  not from OpenBSD's tree.
- Kernel's `just comp` and its `bsdmake.rs` path for LLVM are not moved to DevTools; T1 is a
  CMake build of the fork.
- The two-LLVMs clause of G-0009 is superseded. The watcher compares `llvmorg-22.1.6` with
  upstream's releases.

## Sources
The maintainer, 2026-10-31; `EmiBSD/llvm-project` tag `llvmorg-22.1.6` = `fc4aad7b`;
`EmiBSD/rust` at 1.98.1, `src/bootstrap/src/core/build_steps/llvm.rs` (`need >=21`).
