<!-- SPDX-License-Identifier: Apache-2.0 OR MIT -->

<p align="center">
  <img src="https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg" alt="NucleusFlow logo" width="128" />
</p>

<h1 align="center">NucleusFlow</h1>

<p align="center">
  Fast, flexible, and secure static site generator and content processing pipeline in Rust. Parse CommonMark + GFM with YAML frontmatter, render cached Handlebars templates, and minify HTML5 output in ~28 microseconds per page with zero unsafe code.
</p>

<p align="center">
  <a href="https://github.com/sebastienrousseau/nucleusflow.github.io/actions"><img src="https://img.shields.io/github/actions/workflow/status/sebastienrousseau/nucleusflow.github.io/ci.yml?style=for-the-badge&logo=github" alt="Build" height="28" /></a>
  <a href="https://github.com/sebastienrousseau/nucleusflow.github.io/releases"><img src="https://img.shields.io/github/v/release/sebastienrousseau/nucleusflow.github.io?style=for-the-badge&color=fc8d62&logo=git" alt="Release" height="28" /></a>
  <a href="https://nucleusflow.com"><img src="https://img.shields.io/badge/docs-nucleusflow.com-blue?style=for-the-badge&labelColor=555555&logo=googlechrome" alt="Docs" height="28" /></a>
  <a href="https://scorecard.dev/viewer/?uri=github.com/sebastienrousseau/nucleusflow.github.io"><img src="https://img.shields.io/ossf-scorecard/github.com/sebastienrousseau/nucleusflow.github.io?style=for-the-badge&label=OpenSSF%20Scorecard&logo=openssf" alt="OpenSSF Scorecard" height="28" /></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-Apache--2.0%20OR%20MIT-blue.svg?style=for-the-badge" alt="License: Apache-2.0 OR MIT" height="28" /></a>
  <a href="https://static-site-generator.com/"><img src="https://img.shields.io/badge/SSG-0.0.69-93450a.svg?style=for-the-badge&logo=rust" alt="Built with Rust SSG" height="28" /></a>
</p>

---

## Contents

**Getting started**

- [Install](#install) - build prerequisites and tooling
- [Requirements](#requirements) - toolchain floor, platforms
- [Quick Start](#quick-start) - compile and preview the static site

**The NucleusFlow ecosystem**

- [The NucleusFlow ecosystem](#the-nucleusflow-ecosystem) - core Rust library, standalone CLI, and documentation portal

**Library reference**

- [Capabilities at a glance](#capabilities-at-a-glance) - static site architecture and web standards
- [Ecosystem comparison](#ecosystem-comparison) - design and accessibility comparison
- [Benchmarks](#benchmarks) - compilation speed and WCAG audit scores
- [Features](#features) - site capabilities and security features
- [Configuration](#configuration) - SSG manifest and layout configuration
- [Examples](#examples) - runnable development workflows

**Operational**

- [When not to use NucleusFlow](#when-not-to-use-nucleusflow) - limitations
- [Development](#development) - make targets, validation, and quality gates
- [Security](#security) - Subresource Integrity (SRI) and CSP enforcement
- [Documentation](#documentation) - all reference docs
- [Stability guarantees](#stability-guarantees) - versioning discipline and WCAG AAA compliance
- [License](#license)

---

## Install

### As a Static Site Generator project

Ensure `ssg` (Static Site Generator) and Python are installed:

```bash
# Install the Rust static site generator
cargo install ssg --locked --version 0.0.69

# Clone the documentation repository
git clone https://github.com/sebastienrousseau/nucleusflow.github.io.git
cd nucleusflow.github.io
```

Optional audit tooling includes `html-validate`, `pa11y-ci`, and `@lhci/cli` for CI conformance testing:

```bash
npm install -g html-validate pa11y-ci @lhci/cli
```

---

## Requirements

- **Rust Toolchain**: Rust 1.89.0+ (for compiling `nucleusflow` and `ssg`).
- **Node.js**: Node 24+ (for HTML validation and accessibility CI suites).
- **Python**: Python 3.12+ (for contrast ratios and regression testing).
- **Target Platforms**: Linux, macOS, Windows (cross-platform static deployment).

---

## Quick Start

```bash
# Build the complete site using Rust SSG and post-build optimization
make build

# Run WCAG 2.2 Level AAA and regression test suites
make audit

# Serve locally for preview
python3 -m http.server 8000 --directory docs
```

Navigate to `http://localhost:8000` to inspect the generated documentation portal.

---

## The NucleusFlow ecosystem

The `nucleusflow` suite provides end-to-end tooling for Rust content processing, static site generation, and web documentation:

| Component | Purpose | Use case |
| :--- | :--- | :--- |
| [nucleusflow](https://github.com/sebastienrousseau/nucleusflow) | Core Rust library crate (`v0.0.11`) | Embeddable CommonMark + GFM, YAML 1.2 frontmatter, Handlebars templating, and HTML5 minification pipeline |
| [nucleusflow CLI](https://crates.io/crates/nucleusflow) | Standalone command-line binary | Scaffolding (`new`), site builds (`build --minify`), and local live-reload server (`serve --watch`) |
| [nucleusflow.github.io](https://github.com/sebastienrousseau/nucleusflow.github.io) | Static documentation portal | Sovereign, zero-telemetry documentation, benchmarks, and architectural guides |

---

## Capabilities at a glance

| Area | Capability | Status |
| :--- | :--- | :--- |
| Static Generation | Rust SSG v0.0.69 + Visage layout compilation | Active |
| Accessibility | 100% WCAG 2.2 Level AAA compliance (>= 7.0:1 contrast) | Verified |
| Content Security | Strict Content-Security-Policy (CSP) with zero inline scripts | Enforced |
| Integrity | SHA-384 Subresource Integrity (SRI) on all scripts and styles | Active |
| Search Engine | Client-side zero-telemetry indexing via `search-index.json` | Active |
| Feeds & Syndication | Atom, RSS 2.0, JSON Feed, Sitemap, and CycloneDX SBOM | Automated |

---

## Ecosystem comparison

| Project | Zero Telemetry | WCAG 2.2 AAA | Subresource Integrity | Fast Native SSG |
| :--- | :---: | :---: | :---: | :---: |
| **NucleusFlow Web** | **Yes** | **Yes (7.09:1 worst)** | **Yes (SHA-384)** | **Yes (Rust SSG)** |
| Docusaurus | No (Default telemetry) | Partial (AA) | No | No (Node.js) |
| GitBook / Cloud Docs | No (Third-party hosted) | Partial (AA) | No | No (Proprietary SaaS) |

---

## Benchmarks

Audit and regression suites are executed continuously on every commit:

| Scenario | Result | Environment |
| :--- | ---: | :--- |
| Full Site Compilation (20 Pages) | < 2.0s | Rust `ssg` 0.0.69, 32 plugins |
| NucleusFlow Core Single-Page Pipeline | ~28.0 µs | Criterion 0.8, Apple Silicon M4, Rust 1.89.0 |
| Worst Light Mode Contrast Ratio | 7.09:1 | WCAG 2.2 AAA (Threshold >= 7.0:1) |
| Worst Dark Mode Contrast Ratio | 8.10:1 | WCAG 2.2 AAA (Threshold >= 7.0:1) |
| Lighthouse Performance / A11y | 100 / 100 | Clean ECMAScript, zero heavy runtimes |

---

## Features

- **Rust Static Site Generator (SSG)**: Markdown content parsing and template compilation with sub-second page generation.
- **Visage Design System**: Responsive glass-morphism sticky header, squircle buttons, and smooth contrast-mode toggling on Skeletonic CSS.
- **Subresource Integrity (SRI)**: SHA-384 cryptographic digests enforced across all fingerprinted assets.
- **Zero Third-Party Tracking**: Completely self-hosted static assets and brand icons on `cloudcdn.pro`.
- **Offline PWA Readiness**: Web App Manifest generation, Service Worker (`sw.js`), and offline fallback routes.
- **AI Integration Manifests**: Machine-readable `llms.txt`, `llms-full.txt`, and Agent API documentation.

---

## Configuration

Site configuration is governed by `config/ssg.json` and `ssg.toml`:

```json
{
  "site_name": "NucleusFlow",
  "site_title": "NucleusFlow | Fast, Secure Rust Static Site Generator & Content Pipeline",
  "base_url": "https://nucleusflow.com"
}
```

Template layouts reside in `_layouts/` with Markdown articles under `_posts/`. Compiled web assets are mirrored to `docs/` for GitHub Pages distribution.

---

## Examples

### Local Compilation & Contrast Verification

```bash
# Compile Markdown content and layout templates into public/ and docs/
make build

# Run mathematical color contrast verification
make contrast

# Run Markdown frontmatter schema validator
make validate
```

---

## When not to use NucleusFlow

- **Dynamic Server-Side Applications**: This repository produces purely static, pre-rendered HTML/CSS/JS. It does not include a dynamic backend database or runtime server.
- **Browser-Executed Rust Compilation**: The documentation portal explains and distributes NucleusFlow; actual Markdown processing and site compilation run locally via the `nucleusflow` Rust library or CLI binary.

---

## Development

```bash
# Run complete verification suite
make verify

# Run HTML5 and WCAG structural validation
npx html-validate "docs/**/*.html"

# Run color contrast compliance check
python3 audit/contrast.py
```

All pull requests must pass HTML validation, contrast analysis, and structural self-hosting guardrails before merging.

---

## Security

Every deployment enforces strict cryptographic and architectural controls:

- **Strict Content Security Policy**: Eliminates unsafe script evaluation and restricts asset loading.
- **Air-Gapped Operation**: Zero outbound analytics, telemetry, or external tracking beacons.
- **Supply Chain Assurance**: CycloneDX SBOM generated on every build (`docs/sbom.cdx.json`).

Report vulnerabilities according to [`SECURITY.md`](SECURITY.md).

---

## Documentation

- [Getting Started Guide](https://nucleusflow.com/getting-started/index.html)
- [Supported Formats Matrix](https://nucleusflow.com/formats/index.html)
- [Pipeline Architecture](https://nucleusflow.com/architecture/index.html)
- [Performance Benchmarks](https://nucleusflow.com/benchmarks/index.html)
- [Memory-Safe Security](https://nucleusflow.com/security/index.html)
- [API & CLI Reference](https://nucleusflow.com/api/index.html)
- [Release Changelog](https://nucleusflow.com/changelog/index.html)
- [Contributing Guidelines](https://nucleusflow.com/contributing/index.html)
- [WCAG AAA Accessibility Statement](https://nucleusflow.com/accessibility/index.html)

---

## Stability guarantees

Versions follow strict Semantic Versioning (`0.0.1` -> `0.0.2` -> ...), advancing incrementally per repository conventions. The layout, visual contrast, and WCAG AAA compliance are regression-tested on every release.

---

## License

Dual-licensed under [Apache-2.0](LICENSE-APACHE) OR [MIT](LICENSE-MIT), at your option. See [LICENSE](LICENSE).
