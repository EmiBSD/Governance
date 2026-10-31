# G-0016: EmiBSD's own target triple, `aarch64-unknown-emibsd`; the toolchains carry it as patches of ours

Date: 2026-10-31 · Status: accepted · Binds: DevTools, Userland, Kernel, OS

## Context
With G-0015 DevTools builds upstream LLVM and Rust and nothing of OpenBSD enters it. The
target they emitted for was still named `aarch64-unknown-openbsd`: the triple upstream LLVM
and Rust already carry for the ABI EmiBSD keeps byte-identical to OpenBSD's
(`docs/invariants.md`: the system-call ABI, sysctl names, ioctls, device numbers). Three
options were put to the maintainer: keep that triple as the name of the ABI; a triple of
EmiBSD's own, which is new operating-system support written into LLVM and Rust and carried
by us; or the first now and the second only when EmiBSD's ABI diverges. The agent
recommended the third.

## Decision
The maintainer, 2026-10-31: the second. "No quiero nada de OpenBSD en DevTools; son tools
sanas." The toolchains emit for **`aarch64-unknown-emibsd`** (`x86_64-unknown-emibsd` when
amd64 returns, G-0007). The support for that target is written by us as the patch series of
`DevTools/llvm/patches/` and `DevTools/rust/patches/` over the pinned upstream tags, each
patch with its reason, regenerated at every pin bump, offered upstream when it is stable.
The forks stay mirrors.

## Consequences
- **DevTools stops being imported-only.** The target support is written code, ISC, in
  `patches/`: the one place G-0009 allows it. Its size is counted in each release's
  DELIVERY.md and the series is the first thing a pin bump must re-apply.
- **LLVM** (T1): the OS `EmiBSD` in `llvm/lib/TargetParser/Triple.cpp`; a clang toolchain
  driver `clang/lib/Driver/ToolChains/EmiBSD.cpp` (the crt files, PIE by default, lld,
  compiler-rt, libc++, the `ld.so` path, `-z` defaults: what the ABI needs, written from the
  ABI, not copied from OpenBSD's driver); the target info in
  `clang/lib/Basic/Targets/OSTargets.h` defining `__EmiBSD__`; the runtimes' CMake and
  sources (compiler-rt, libunwind, libc++abi, libc++) taught the new OS; a CMake platform
  file for `CMAKE_SYSTEM_NAME=EmiBSD`, shipped by DevTools.
- **Rust** (T2): a target spec `aarch64-unknown-emibsd` in `rustc_target` with its base;
  `std`'s `target_os = "emibsd"` paths; the `libc` crate's bindings for the new OS, which
  rust vendors, as a patch of DevTools too. Tier 3 by upstream's rules, as the openbsd one
  was; `std` still built from the stable release's source, no nightly.
- **The inherited C of Userland** (G-0013) tests `__OpenBSD__`. What the compiler defines
  for it is Userland's matter, decided there: `__EmiBSD__` alone and the C adapted where it
  asks, or both macros while the inherited tree lasts. Not decided here.
- **Kernel** reads `aarch64-unknown-emibsd` wherever it read the old triple (its smokes, the
  comp disk); the ABI the kernel presents does not change: the triple is a new name for the
  ABI EmiBSD already has.
- **OS** packages the comp set under the new triple.
- Upstreaming: an OS target needs a maintainer upstream; until EmiBSD is one, the series
  is carried. The watcher reports pin bumps as before; the series' re-application is the
  cost of each.

## Sources
The maintainer, 2026-10-31, in DevTools' T1 session, after the fetch of `llvm-project` at
`llvmorg-22.1.6`; G-0015; `docs/invariants.md` (the ABI).
