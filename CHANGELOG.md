<!-- SPDX-FileCopyrightText: 2024-2026 Sebastien Rousseau -->
<!-- SPDX-License-Identifier: Apache-2.0 OR MIT -->

# Changelog

All notable changes to `nucleusflow.github.io` (`nucleusflow.com`) are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.0.1] - 2026-10-31

### Added

- **Initial Documentation Portal (`v0.0.1`)**: Launched `nucleusflow.com`, the official static documentation website for the `nucleusflow` (`v0.0.11`) Rust static site generator and content processing pipeline.
- **20 Comprehensive Documentation Routes**: Published guides covering Getting Started (`getting-started`), Supported Formats (`formats`), API & CLI Reference (`api`), Pipeline Architecture (`architecture`), Performance Benchmarks (`benchmarks`), Use Cases (`use-cases`), Memory-Safe Security (`security`), FAQs (`faqs`), About & Governance (`about`), Release Changelog (`changelog`), Contact (`contact`), Contributing (`contributing`), WCAG AAA Accessibility (`accessibility`), Privacy (`privacy`), Terms (`terms`), Acknowledgements (`thanks`), Made with SSG (`made-with-ssg`), 404 (`404`), and Offline PWA (`offline`).
- **Visage Theme & WCAG 2.2 Level AAA Compliance**: Engineered on Skeletonic CSS with verified `>= 7.0:1` contrast ratios across light and dark modes (`7.09:1` worst light pair, `8.10:1` worst dark pair), skip links, semantic landmarks, and zero-flash theme persistence.
- **Rust SSG 0.0.69 Compilation & Supply Chain Hardening**: Configured deterministic builds emitting SHA-384 Subresource Integrity (SRI) digests, strict Content-Security-Policy (CSP) headers, CycloneDX 1.5 SBOM (`docs/sbom.cdx.json`), RFC 9116 `security.txt`, and AI context bundles (`llms.txt` and `llms-full.txt`).
