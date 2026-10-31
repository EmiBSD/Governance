# G-0012: DevTools and OS as repositories

Date: 2026-10-31 · Status: proposed · Binds: Userland, Kernel, DevTools, OS

## Context
The toolchains are defined as imported programs under `Userland/compiler/` with nothing built,
and the only working toolchain build is Kernel's `just comp`. Release assembly is split between
Kernel's xtask (images, ramdisk, ISO) and Userland's `distrib/` skeletons. The maintainer's
sketch of 2026-10-31 puts the toolchain in its own repository fed by the forks, and an OS
repository assembling Kernel and Userland. `docs/architecture.md` is the worked version.

## Proposal
1. **DevTools** now: `Userland/compiler/` and Kernel's `just comp` move there; it releases the
   toolchains as artifacts with checksums; milestones T0 to T4.
2. **OS** at the first bootable base set, not before: a manifest of pins, the sets, the
   installer, the smokes of the whole system, the `YEAR.MONTH` tag.
3. Forks stay read-only mirrors; patches live in the consuming repository.

## Open (the maintainer's)
The name (DevTools or Toolchain), the scope (lldb?), hosts, and when OS is created. Listed at
the end of `docs/architecture.md`. This file becomes `accepted` with the answers recorded.

## Sources
The maintainer's diagram and this session, 2026-10-31.
