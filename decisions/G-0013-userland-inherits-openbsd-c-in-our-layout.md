# G-0013: Userland inherits OpenBSD's C in our layout; the Rust rewrite moves to `experimental`

Date: 2026-10-31 · Status: proposed · Binds: Userland, Kernel, DevTools, OS, Website

## Context
G-0009 made Userland a from-scratch Rust system under one contract. After a week the
maintainer judged the proof of concept not worth continuing as the base system: two programs
designed, the rest planned, and the system still running OpenBSD's userland from Kernel's own
build. What the maintainer wants to keep is the other half of that work: the layout. OpenBSD's
tree is the accretion of thirty years (`bin`, `sbin`, `usr.bin`, `usr.sbin`, `libexec`, `lib`,
programs beside each other with no grouping); Userland's layout groups every program by what
it does (`bin/files`, `sbin/storage/mounts`, `usr/sbin/network/...`), and `tools/layout.py`
already maps every one of the 518 program directories of OpenBSD at the pin to exactly one
place in it, with no gaps (checked 2026-10-31).

Kernel already cross-compiles OpenBSD's userland at the same pin (`3ce1f3f7`) with
`cargo xtask userland`: sysroot, csu, libc, the shared libraries, `ld.so`, 110 programs and
the ffs ramdisk, through `bsdmake.rs`, an evaluator of OpenBSD's Makefiles. The build exists;
it has no home of its own and no order.

## Proposal
1. **Branch.** The current `main` of Userland is pushed as `experimental` and left there with
   its history; `main` continues from the same commit (no orphan branch), removing the Rust
   programs and crates and keeping the layout, the catalogue, the import machinery
   (`UPSTREAM.toml`, `upstream.py`, `cve.py`), `distrib/`, `regress/` and the document discipline.
2. **Three classes** replace G-0009's two: *inherited* (OpenBSD's C at the pin of
   LandingZone and Kernel, from the `OpenBSD-src` mirror, every notice kept whole, patched
   under `patches/`, never rewritten in place), *imported* (third-party upstreams, as today),
   *written* (from scratch, in Rust, when a program is worth it; the long path, one program at
   a time, as Kernel replaces LandingZone's tree subsystem by subsystem).
3. **Source and pin.** The C comes from the `OpenBSD-src` mirror, never from LandingZone,
   which holds no userland (its `reference/` is a read-only clone of the same mirror). The
   pin is LandingZone's (`reference/PINNED.md`, `ports.toml`): it defines the ABI the kernel
   implements, Kernel follows it through `lz-sync`, and Userland reads it and refuses to
   build if it differs from Kernel's. The inherited class uses the existing `UPSTREAM.toml`
   form (`repo` the mirror, `commit` the pin, `path` the directories), as `compiler/llvm`
   already does. The ABI headers (`sys/*.h`, `machine/*.h` into `/usr/include`) come from
   OpenBSD's `sys/` at the same pin today, byte-identical to Kernel's by invariant 2; when
   Kernel exports them from its `abi` crate, that becomes an arrow from Kernel to Userland.
4. **The tree is generated, never moved by hand.** `layout.toml`, derived from `layout.py`'s
   table, maps `origin` (the OpenBSD directory) to `target` (ours) with the class; a
   materialiser fetches the pin from the mirror, places each directory, applies `patches/`,
   and writes the per-group `Makefile.inc` the relocated Makefiles need. A pin bump is a
   re-run plus the patches that no longer apply.
5. **The build driver** moves from Kernel to Userland: `bsdmake.rs` and `userland.rs` become
   Userland's build, told each program's original path so the Makefiles' relative references
   resolve. Kernel keeps its copy against `reference/` for its own smokes until N9, so the
   redesign does not couple to Userland's state; OS unifies both.
6. **`gnu/` is excluded** by default (`policies/licences.md`): DevTools takes LLVM; Perl and
   the rest are decided piece by piece. OpenBSD's `regress/` stays out of the tree and is
   brought per program when needed.
7. **Public wording** changes the same day: Userland's README and GitHub description, the
   the Website, this repository's `CLAUDE.md` and `docs/`. Userland is to the userland what
   LandingZone is to the kernel: OpenBSD's code in an order of our own, the base the rewrite
   grows from.

## Consequences
- G-0009 is superseded in part: "written from scratch" stops being the base system and
  becomes a class; the imported class and "nothing GPL" stand. `docs/PROGRAM.md`'s contract
  (records, `-o`, `--request`) applies to written programs only and lives in `experimental`
  until one is written again.
- DevTools' order changes: T1 (the clang of upstream LLVM, G-0015) is what Userland needs now; T2 (rustc cross)
  waits for the first written program.
- Kernel's invariant "OpenBSD's userland runs unmodified" stays literally true: the binaries
  are the same C, built from the same pin, in a different order on disk.
- The 110 programs Kernel builds today are the first milestone; the other 408 directories are
  where the time goes (host tools the Makefiles expect, flags, yacc and Perl at build time).

## Open (the maintainer's)
1. Is the inherited C committed to Userland, or fetched like `upstream/`? Committed is
   proposed: it is the product and the thing to read on GitHub, as LandingZone commits its Rust.
2. Does Kernel keep its own userland build until N9 (proposed), or consume Userland's ramdisk now?
3. Perl: inherit it under its Artistic licence for the five Perl programs, or leave those five out.
4. The milestone letter for Userland (U?) and its first milestone: the 110 programs Kernel
   builds, in the new layout, booting Kernel's smokes.

## Sources
The maintainer, 2026-10-31: the order is the thing to keep, OpenBSD's tree grew by accretion and
ours classifies every program by purpose; `Userland/tools/layout.py` (`LAYOUT`, `SOURCE_DIR`, `IMPORTED`);
`Kernel/tools/xtask/src/userland.rs` and `bsdmake.rs`; `Kernel/reference/PINNED.md`.
