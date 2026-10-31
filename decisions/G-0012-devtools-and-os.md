# G-0012: DevTools and OS as repositories

Date: 2026-10-31 · Status: proposed · Binds: Userland, Kernel, DevTools, OS

## Context
The toolchains are defined as imported programs under `Userland/compiler/` with nothing built,
and the only working toolchain build is Kernel's `just comp`. Release assembly is split between
Kernel's xtask (images, ramdisk, ISO) and Userland's `distrib/` skeletons. The maintainer's
sketch of 2026-10-31 puts the toolchain in its own repository fed by the forks, and an OS
repository assembling Kernel and Userland.

## Proposal
1. **DevTools** now: `Userland/compiler/` and Kernel's `just comp` move there; it releases the
   toolchains as artifacts with checksums; milestones T0 to T4.
2. **OS** at the first bootable base set, not before: a manifest of pins, the sets, the
   installer, the smokes of the whole system, the `YEAR.MONTH` tag.
3. Forks stay read-only mirrors; patches live in the consuming repository.

## Decided so far
- 2026-10-31: the name is **DevTools**; the repository `EmiBSD/DevTools` was created by the
  maintainer and T0 started the same day (the pins copied from `Userland/compiler/`, the
  tools, the mirror check). `Userland/compiler/` is removed when Userland's `main` restarts, not before.
- 2026-10-31: **OS now**, not at the first release: the repository `EmiBSD/OS` was created by
  the maintainer the same day, thin, with the manifest of pins (which already states a
  reproducible system: Kernel 2c1d8144 on OpenBSD's userland), the release checklist and the
  milestones R0..R2; it builds nothing of its own.

## Open (the maintainer's)
The scope of DevTools (lldb in or out) and its hosts beyond macOS arm64. This file becomes
`accepted` with the answers recorded; DevTools keeps the detail in its own log.

## Sources
The maintainer's diagram and this session, 2026-10-31.
