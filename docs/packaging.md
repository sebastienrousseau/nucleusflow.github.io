# Notes for distribution maintainers

Nucleusflow.com is a static documentation website, not a system library
or standalone executable binary. Standard package formats (deb, rpm, Homebrew,
AUR, Nix) for the core `nucleusflow` CLI live in the upstream
`sebastienrousseau/nucleusflow` repository under `pkg/`.

To reproduce the deployable static site locally:

1. Install the Rust toolchain and Static Site Generator: `cargo install ssg --locked --version 0.0.69`
2. Install Python 3.10 or later.
3. Run `make build`.

The compiled site is placed in `public/` and synchronized to `docs/`.

Project-authored code and templates are dual-licensed under Apache-2.0 OR MIT.
Verify release tags against `KEYS.asc` and authenticate archives using `SHA256SUMS`.
