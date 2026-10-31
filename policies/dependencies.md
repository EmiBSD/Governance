# Dependencies, toolchains and tools

- **Stable Rust only**, pinned in each repository's `rust-toolchain.toml`. A bump is its own
  commit and the maintainer's decision. No nightly features, not even for a tool.
- **A new crate** needs the maintainer's OK before it is proposed in a commit: does `std`
  cover it, what `cargo tree` says it drags in, who maintains it and when it last released.
  Each repository keeps its allowlist (Kernel's `docs/ARCHITECTURE.md` "Dependencies",
  Userland's `docs/PROGRAM.md`). Userland's workspace has no external crate today, and that
  is the state to keep.
- **Nothing installed, nothing downloaded** by an agent without the maintainer's OK. The guard
  hooks refuse `cargo install`, `rustup`, `brew`, `pip`, `npm`, `curl` and `wget`; the one
  sanctioned download is a repository's own fetch recipe (`tools/upstream.py fetch`), run
  from the main session.
- **Upstream pins** are commits or tags in the organisation's mirrors, recorded in
  `UPSTREAM.toml` with the origin, the licence family and the CPE for the CVE watch. The
  mirrors are read-only; our patches live in `patches/` with their reason.
- **Lockfiles are committed**; `cargo update` is its own commit with the diff reviewed.
- **Toolchain artifacts** (DevTools, once it exists) are consumed by pin and checksum, never
  by "latest".
