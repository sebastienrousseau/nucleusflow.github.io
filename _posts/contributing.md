---
author: "Sebastien Rousseau"
title: "Contributing to NucleusFlow: Workflow, Quality Gates & Commit Standards"
description: "Contributor guide for the NucleusFlow Rust library and documentation portal: branch naming, complexity ceilings, SSH-signed commits, and verification commands."
keywords: "contribute to nucleusflow, rust open source contribution, nucleusflow development guide, make verify"
date: "2026-10-31T00:00:00+00:00"
updated: "2026-10-31T00:00:00+00:00"
apple_status_bar_style: "black-translucent"
apple-touch-icon: "https://cloudcdn.pro/nucleusflow/v1/icons/192x192.png"
banner: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
canonical: "https://nucleusflow.com/contributing/index.html"
Category: "Legal"
item_url: "https://nucleusflow.com/contributing/index.html"
lang: "en-GB"
eyebrow: "Open Source"
headline: "Contributing to NucleusFlow"
lead: "Engineering standards, verification gates, and pull request discipline."
layout: "page"
locale: "en_GB"
logo: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
manifest: "/manifest.json"
name: "NucleusFlow"
permalink: "https://nucleusflow.com/contributing/index.html"
robots: "index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1"
rss: "/rss.xml"
short_name: "nucleusflow"
subtitle: "Engineering standards, verification gates, and pull request discipline."
theme_color: "#11355c"
twitter_card: "summary_large_image"
url: "https://nucleusflow.com/contributing/index.html"
viewport: "width=device-width, initial-scale=1, shrink-to-fit=no"
---

## 1. Development Workflow

We welcome contributions to both the [core Rust repository](https://github.com/sebastienrousseau/nucleusflow) and the [documentation portal](https://github.com/sebastienrousseau/nucleusflow.github.io).

1. **Fork &amp; Branch**: Create a feature branch targeting the next patch iteration (`feat/vX.Y.Z`).
2. **Code Quality &amp; Complexity Ceilings**:
   * Zero `unsafe` blocks (`#![forbid(unsafe_code)]`).
   * Cyclomatic complexity `<= 10` per function; cognitive complexity `<= 15` per function.
   * Maximum `60` lines of code per function and `500` lines per file.
3. **SPDX Headers**: Every source file must carry `SPDX-FileCopyrightText` and `SPDX-License-Identifier: Apache-2.0 OR MIT` headers.

---

## 2. Running Local Quality Gates

### Core Rust Library (`nucleusflow`)

```bash
cargo fmt --all -- --check
cargo clippy --all-targets --all-features -- -D warnings
cargo test --all-targets --all-features
```

### Documentation Website (`nucleusflow.github.io`)

```bash
# Compile with ssg 0.0.69 and run all lint, test, and WCAG AAA audit suites
make verify
```

---

## 3. Commit &amp; Pull Request Conventions

* **Conventional Commits**: `<type>: <subject>` (maximum 50 characters, imperative mood; body wrapped at 72 characters).
* **Signed Commits**: Sign commits with SSH/GPG (`git commit -S -s`).
* **Dual Licensing**: All contributions are provided under the repository's dual `Apache-2.0 OR MIT` license.
