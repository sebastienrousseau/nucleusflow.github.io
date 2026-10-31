---
author: "Sebastien Rousseau"
title: "Release Changelog: NucleusFlow Library & Documentation History"
description: "Complete release history for the NucleusFlow Rust crate (v0.0.1 through v0.0.11) and the nucleusflow.com documentation portal."
keywords: "nucleusflow changelog, nucleusflow releases, v0.0.11 release notes, rust crate history"
date: "2026-10-31T00:00:00+00:00"
updated: "2026-10-31T00:00:00+00:00"
apple_status_bar_style: "black-translucent"
apple-touch-icon: "https://cloudcdn.pro/nucleusflow/v1/icons/192x192.png"
banner: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
canonical: "https://nucleusflow.com/changelog/index.html"
Category: "Information"
item_url: "https://nucleusflow.com/changelog/index.html"
lang: "en-GB"
eyebrow: "Release History"
headline: "Release Changelog"
lead: "Version history across the nucleusflow Rust crate and documentation portal."
layout: "page"
locale: "en_GB"
logo: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
manifest: "/manifest.json"
name: "NucleusFlow"
permalink: "https://nucleusflow.com/changelog/index.html"
robots: "index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1"
rss: "/rss.xml"
short_name: "nucleusflow"
subtitle: "Version history across the nucleusflow Rust crate and documentation portal."
theme_color: "#11355c"
twitter_card: "summary_large_image"
url: "https://nucleusflow.com/changelog/index.html"
viewport: "width=device-width, initial-scale=1, shrink-to-fit=no"
---

## Core Library &amp; CLI (`nucleusflow` Crate)

### `v0.0.11` (2026-10-30)

* **Full Pipeline Orchestration**: Connected `NucleusFlow::process()` end-to-end across `ContentProcessor`, `HandlebarsRenderer`, and `HtmlGenerator` with CLI `new`, `build`, and `serve` execution.
* **Dependency &amp; MSRV Modernization**: Raised MSRV to **Rust 1.89.0**, migrated YAML frontmatter parsing to `noyalib 0.0.58`, and upgraded `clap` (`4.5`), `handlebars` (`6.2`), `pulldown-cmark` (`0.13.5`), `minify-html` (`0.18.1`), `toml` (`0.9.5`), `thiserror` (`2.0`), and `criterion` (`0.8`).
* **Architectural &amp; Packaging Suite**: Added `docs/ARCHITECTURE.md`, `docs/BENCHMARKS.md`, `docs/COMPARISON.md`, `docs/MIGRATION-HUGO.md`, `docs/MIGRATION-ZOLA.md`, `docs/ADR` (`0001`..`0003`), and distribution manifests (`pkg/brew`, `pkg/aur`, `pkg/debian`, `pkg/rpm`, `pkg/nix`).

### `v0.0.9` - `v0.0.10`

* **Quality Gate &amp; CI Hardening**: Enforced clippy pedantic lints, SPDX license headers, OpenSSF Scorecard workflows, and deterministic Criterion benchmarks.

### `v0.0.1` - `v0.0.8`

* **Foundation Releases**: Introduced `MarkdownProcessor` (CommonMark + GFM + TOC + HTML sanitization), `HtmlTemplateRenderer` (`parking_lot::RwLock` Handlebars cache + `UppercaseHelper`), `HtmlOutputGenerator` (HTML5 tag balance + `minify-html`), `FileContentProcessor` (`sanitize_path`), and `ConfigBuilder`.

---

## Documentation Website (`nucleusflow.github.io`)

### `v0.0.1` (2026-10-31)

* **Initial Public Launch (`nucleusflow.com`)**: Published 20 WCAG 2.2 Level AAA compliant documentation routes compiled with `ssg 0.0.69` and Skeletonic CSS (`Visage` theme).
* **Cryptographic &amp; AI Readiness**: Added SHA-384 Subresource Integrity (SRI) digests, strict Content-Security-Policy (CSP), CycloneDX 1.5 SBOM (`sbom.cdx.json`), RFC 9116 `security.txt`, and LLM context bundles (`llms.txt` and `llms-full.txt`).
