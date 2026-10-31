# Licences

## Our code
ISC, with the author's block first in every file (decision G-0003). Each repository carries
`LICENSE` with the same text.

## Derived code
A file derived from OpenBSD keeps every original copyright and licence notice whole, after the
author's block. C is never copied verbatim; the derivation is a port or a redesign of
behaviour, and `lineage.toml` (Kernel) or `ports.toml` (LandingZone) records it.

## Written userland programs
Owe nothing to anyone: written from a specification, nothing copied or translated
(G-0010). They carry only the author's block.

## Imported programs and dependencies
| Licence | Status |
|---|---|
| ISC, MIT, BSD-2, BSD-3, Apache-2.0 (with or without the LLVM exception), Zlib, Unicode-3.0 | acceptable |
| MPL-2.0 | with a written reason |
| Artistic (Perl) | open: the maintainer's decision, see Userland's README "Open" |
| GPL, LGPL, AGPL | never, not even a build tool that links |

An imported program's licence family is recorded in its `UPSTREAM.toml`; the texts stay in
the source. `cargo deny` with this allowlist once it is installed.

## Trademarks
EmiBSD is not affiliated with or endorsed by the OpenBSD project and says so wherever it
presents itself. OpenBSD's name appears only where code under its copyright is compiled or
where its behaviour is named as the specification.
