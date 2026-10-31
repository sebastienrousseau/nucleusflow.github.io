---
author: "Sebastien Rousseau"
title: "About NucleusFlow: Engineering Principles & Governance"
description: "Learn about the engineering philosophy behind NucleusFlow, maintained by Sebastien Rousseau: memory-safe Rust, deterministic performance, and WCAG 2.2 Level AAA accessibility."
keywords: "about nucleusflow, sebastien rousseau, rust open source, memory-safe static site generator, nucleusflow governance"
date: "2026-10-31T00:00:00+00:00"
updated: "2026-10-31T00:00:00+00:00"
apple_status_bar_style: "black-translucent"
apple-touch-icon: "https://cloudcdn.pro/nucleusflow/v1/icons/192x192.png"
banner: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
canonical: "https://nucleusflow.com/about/index.html"
Category: "Information"
item_url: "https://nucleusflow.com/about/index.html"
lang: "en-GB"
eyebrow: "Project & Governance"
headline: "About NucleusFlow"
lead: "Built in London with memory-safe Rust, deterministic engineering, and zero telemetry."
layout: "page"
locale: "en_GB"
logo: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
manifest: "/manifest.json"
name: "NucleusFlow"
permalink: "https://nucleusflow.com/about/index.html"
robots: "index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1"
rss: "/rss.xml"
short_name: "nucleusflow"
subtitle: "Built in London with memory-safe Rust, deterministic engineering, and zero telemetry."
theme_color: "#11355c"
twitter_card: "summary_large_image"
url: "https://nucleusflow.com/about/index.html"
viewport: "width=device-width, initial-scale=1, shrink-to-fit=no"
---

## 1. Mission &amp; Design Philosophy

**NucleusFlow** (`nucleusflow`) was created to bridge the gap between monolithic static site generator binaries and low-level Markdown parsers. Engineering teams frequently need the same reliable content processing pipeline in two places: on the command line for static documentation builds, and inside Rust backend services for dynamic rendering and validation.

NucleusFlow is guided by four core engineering invariants:

1. **Zero Unsafe Code (`#![forbid(unsafe_code)]`)**: Memory safety is non-negotiable when parsing untrusted Markdown, YAML frontmatter, and HTML templates.
2. **Composable Trait Boundaries**: Content processing (`Processor`), template rendering (`TemplateRenderer`), and output generation (`Generator`) are cleanly separated so any stage can be swapped, mocked, or embedded independently.
3. **Deterministic Resource Budgets**: Every stage enforces explicit byte caps (`10 MB` content, `64 KB` metadata, `1 MB` template, `100 MB` cache) to guarantee predictable latency (`~28.0 µs` per page).
4. **Zero Telemetry**: Neither the `nucleusflow` library/CLI nor the `nucleusflow.com` documentation portal makes outbound analytics, telemetry, or tracking requests.

---

## 2. Ecosystem Synergy

NucleusFlow builds upon and integrates with open-source Rust crates maintained by [Sebastien Rousseau](https://sebastienrousseau.com/):

* **[noyalib](https://crates.io/crates/noyalib)**: Pure-Rust, memory-safe YAML 1.2 parser powering NucleusFlow's frontmatter extraction.
* **[dtt](https://crates.io/crates/dtt)**: Date, time, and timezone utilities for structured build timestamps.
* **[ssg](https://static-site-generator.com/)**: The Static Site Generator compiling this documentation portal (`nucleusflow.com`).
* **[Skeletonic CSS](https://skeletonic.com/)**: Lightweight, WCAG 2.2 Level AAA CSS layout foundation.

---

## 3. Governance &amp; Maintainer

NucleusFlow is authored and maintained by **[Sebastien Rousseau](https://sebastienrousseau.com/)** in London, United Kingdom. All architectural changes, releases, and security advisories are managed transparently on GitHub:

* **Core Rust Library &amp; CLI**: [github.com/sebastienrousseau/nucleusflow](https://github.com/sebastienrousseau/nucleusflow)
* **Documentation Portal**: [github.com/sebastienrousseau/nucleusflow.github.io](https://github.com/sebastienrousseau/nucleusflow.github.io)
* **Crates.io Package**: [crates.io/crates/nucleusflow](https://crates.io/crates/nucleusflow)
* **API Documentation**: [docs.rs/nucleusflow](https://docs.rs/nucleusflow)
