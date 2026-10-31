# Repositories and the flows between them

The organisation [github.com/EmiBSD](https://github.com/EmiBSD) holds two kinds of
repository: **forks**, read-only mirrors of the upstreams the project builds from, and
**repositories**, where the work happens. Working draft of 2026-10-31; DevTools and OS are
proposed, not yet decided (the open decisions are at the end).

## Target layout

```mermaid
flowchart LR
    subgraph forks["Forks: read-only mirrors, no commits of ours"]
        direction TB
        obsd["OpenBSD-src<br/>(openbsd/src)"]
        rust["rust<br/>(rust-lang/rust)"]
        llvmrs["llvm-project<br/>(rust-lang/llvm-project)<br/>the LLVM 21 rustc builds"]
        cargo["cargo<br/>(rust-lang/cargo)"]
    end

    subgraph repos["Repositories"]
        direction LR
        dev["DevTools<br/>clang, lld, libc++, compiler-rt<br/>rustc, cargo, std"]
        lz["LandingZone<br/>the 1:1 port of the kernel"]
        ul["Userland<br/>programs and libc of our own"]
        k["Kernel<br/>the redesign in four crates"]
        os["OS<br/>pins + sets + installer + release"]
    end

    obsd -- "gnu/llvm 18 with OpenBSD's patches" --> dev
    rust --> dev
    llvmrs -- "submodule src/llvm-project" --> dev
    cargo -- "submodule src/tools/cargo" --> dev

    obsd -- "the kernel's specification" --> lz
    obsd -- "behaviour reference<br/>+ imports (ssh, LibreSSL, ...)" --> ul
    lz -- "inherited tree (bsd/), lz-sync" --> k

    dev -- "cross toolchain" --> ul
    dev -- "clang today, rustc when self-hosted" --> k
    ul -. "sysroot: libc, crt, headers<br/>(OpenBSD's libc until then)" .-> dev

    k --> os
    ul --> os
    dev -- "the comp set" --> os
```

Governance and Portal are not in the picture: Governance binds every repository and Portal
publishes their state; neither feeds a build.

### Every arrow

| From | To | What flows | Why |
|---|---|---|---|
| OpenBSD-src | LandingZone | the C under `sys/`, read-only, at the pin | the specification of the port |
| OpenBSD-src | Userland | manuals and code as the behaviour reference; the imported programs' sources | written programs come from a specification written against OpenBSD; imported ones are compiled from it |
| OpenBSD-src | DevTools | `gnu/llvm`, `gnu/usr.bin/clang`, `gnu/lib/libcxx*`, `libclang_rt`, `libcompiler_rt` | the system's clang, lld and libc++ are OpenBSD's LLVM 18 with its patches (RETGUARD, `-fret-clean`, the fixed X18, BTI and PAC defaults) |
| rust, llvm-project, cargo | DevTools | rust-lang/rust at the pinned stable tag with its two submodules | rustc 1.98.1 needs LLVM 21 and builds the one in its submodule; OpenBSD's 18 stays for the C. Two LLVMs, decided 2026-10-23 |
| LandingZone | Kernel | the tree at `lz-origin`, then every commit triaged as `lz-sync:` | Kernel redesigns what LandingZone ported; LandingZone never learns Kernel exists |
| DevTools | Userland | the cross toolchain for `aarch64-unknown-openbsd` | Userland's programs are built with it, on the host first |
| DevTools | Kernel | clang today (`just comp`, for the inherited userland the smokes boot); rustc once EmiBSD hosts its own build | the kernel itself needs only a stock stable toolchain, its target is `aarch64-unknown-none-softfloat` |
| Userland | DevTools (dotted) | the sysroot: libc, crt, headers | a binary the toolchain produces needs a libc; `std`'s rlibs are compiled without one, so the cycle breaks there. Until Userland has `lib/libc`, the sysroot is the libc of the OpenBSD sets Kernel already boots |
| Kernel, Userland, DevTools | OS | a kernel image, the installed tree, the comp set | OS assembles and releases; it builds nothing of its own |

## Today

```mermaid
flowchart LR
    subgraph forks["Forks (already created)"]
        direction TB
        obsd["OpenBSD-src"]
        rust["rust"]
        llvmrs["llvm-project (rust-lang)"]
        cargo["cargo"]
    end

    subgraph repos["Repositories"]
        direction LR
        lz["LandingZone"]
        k["Kernel<br/>just comp builds OpenBSD's clang"]
        ul["Userland<br/>compiler/llvm, compiler/rust: pins written, nothing built<br/>distrib/: sets and installer, skeletons"]
    end

    obsd --> lz
    obsd --> ul
    obsd -- "gnu/llvm" --> k
    rust -- "UPSTREAM.toml" --> ul
    llvmrs -- "UPSTREAM.toml" --> ul
    cargo -- "UPSTREAM.toml" --> ul
    lz --> k
```

The pins are consistent: `Userland/compiler/rust/UPSTREAM.toml` names the `llvm-project` and
`cargo` commits that rust's tag 1.98.1 carries as submodules, and both exist in the mirrors.

- **DevTools** is `Userland/compiler/` and Kernel's `just comp` moved to their own repository.
- **OS** is the images and ISO of Kernel's xtask, `Userland/distrib/` and `regress/harness`
  gathered in one place.

## Proposed order

```mermaid
flowchart TB
    T0["T0 · the DevTools repository<br/>moves Userland/compiler/, copies upstream.py,<br/>a recipe that pushes only the pinned tags to the mirrors"]
    T1["T1 · OpenBSD's LLVM 18 built from DevTools<br/>moves Kernel's just comp<br/>proof: Kernel's smoke-install with that clang"]
    T2["T2 · rustc 1.98.1 with x.py, macOS host,<br/>target aarch64-unknown-openbsd, OpenBSD's sysroot<br/>proof: a static Rust hello under Kernel in QEMU,<br/>then Userland's ls and cp"]
    T3["T3 · GitHub releases: one tarball per host + sha256<br/>version 1.98.1+emibsd.1; Userland runs just toolchain<br/>cve.py watches llvm and rust"]
    T4["T4 · self-hosting: rustc built on EmiBSD<br/>stage0 = T3; after N9"]
    R0["OS · created when Userland has a bootable base set<br/>manifest of pins (Kernel, Userland, DevTools),<br/>sets, installer, smokes of the whole system,<br/>diff-openbsd, the YEAR.MONTH tag, the Portal's status.json"]

    T0 --> T1 --> T2 --> T3 --> T4
    T3 -.-> R0
```

T2 is the day EmiBSD runs code of its own on a kernel of its own; it deserves a devlog entry.

## Rules the layout implies

- Forks are mirrors: no commit of ours lands on them, only the upstream tags the pins name.
  Patches live in the consuming repository's `patches/`, each with its reason.
- DevTools knows nothing of Kernel's or Userland's internals; it takes a sysroot as an input
  and publishes artifacts with checksums. Nothing it ships is written code: imported class
  only, plus build glue.
- OS builds nothing; it pins, assembles, boots and tags.

## Open decisions (the maintainer's)

1. Name: DevTools or Toolchain. If lldb is in, DevTools; if only compilers and `std`, Toolchain.
2. Scope of DevTools: clang, lld, libc++, compiler-rt, rustc, cargo, std. lldb?
3. `compiler/` leaves Userland entirely; `distrib/sets` lists the comp set from DevTools' artifacts.
4. Kernel's `just comp` moves to DevTools now.
5. Hosts: macOS arm64 first; Linux x86_64 only if Userland's CI needs it; arm64 OpenBSD at T4.
6. OS now or at the first release; its name: OS, Release or System.
