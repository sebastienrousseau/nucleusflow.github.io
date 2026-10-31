---
author: "Sebastien Rousseau"
title: "Performance Benchmarks: Sub-30 Microsecond Rust Content Pipeline"
description: "Verified Criterion 0.8 micro-benchmarks for NucleusFlow on Apple Silicon (M4, Rust 1.89.0): ~4.2 µs Markdown + YAML parse, ~12.5 µs Handlebars render, ~8.1 µs HTML minification, and ~28.0 µs end-to-end."
keywords: "nucleusflow benchmarks, rust static site generator speed, criterion benchmarks rust, pulldown-cmark benchmark, minify-html benchmark, hugo vs zola vs nucleusflow"
date: "2026-10-31T00:00:00+00:00"
updated: "2026-10-31T00:00:00+00:00"
apple_status_bar_style: "black-translucent"
apple-touch-icon: "https://cloudcdn.pro/nucleusflow/v1/icons/192x192.png"
banner: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
canonical: "https://nucleusflow.com/benchmarks/index.html"
Category: "Documentation"
item_url: "https://nucleusflow.com/benchmarks/index.html"
lang: "en-GB"
eyebrow: "Performance Engineering"
headline: "Sub-30 Microsecond Benchmarks"
lead: "Reproducible Criterion 0.8 latency measurements and ecosystem comparison."
layout: "page"
locale: "en_GB"
logo: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
manifest: "/manifest.json"
name: "NucleusFlow"
permalink: "https://nucleusflow.com/benchmarks/index.html"
robots: "index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1"
rss: "/rss.xml"
short_name: "nucleusflow"
subtitle: "Reproducible Criterion 0.8 latency measurements and ecosystem comparison."
theme_color: "#11355c"
twitter_card: "summary_large_image"
url: "https://nucleusflow.com/benchmarks/index.html"
viewport: "width=device-width, initial-scale=1, shrink-to-fit=no"
---

## 1. Benchmark Methodology &amp; Hardware Baseline

All performance numbers reported for NucleusFlow are measured using [Criterion.rs](https://github.com/bheisler/criterion.rs) (`criterion = "0.8"`) in `/benches/processing_benchmark.rs` and can be reproduced locally on any machine.

* **Hardware**: Apple Silicon (M4, macOS)
* **Toolchain**: `rustc 1.89.0` (2021 Edition), `--release` profile (`opt-level = 3`, `lto = true`, `codegen-units = 1`)
* **Harness**: `cargo bench --bench processing_benchmark`

---

## 2. Stage-by-Stage Micro-Benchmarks

| Benchmark Suite | Operation | Median Time | Throughput Equivalent |
| :--- | :--- | :--- | :--- |
| `markdown_processing` | Parse CommonMark + GFM + extract YAML 1.2 frontmatter | **`~4.2 µs`** | ~238,000 docs / sec (single core) |
| `template_rendering` | Evaluate compiled Handlebars layout with nested partials | **`~12.5 µs`** | ~80,000 renders / sec (single core) |
| `html_minification` | Validate HTML5 tags &amp; collapse whitespace (`minify-html`) | **`~8.1 µs`** | ~123,000 pages / sec (single core) |
| `single_page_pipeline` | End-to-end read, parse, render, minify, and write | **`~28.0 µs`** | ~35,700 pages / sec (single core) |

At `~28.0 µs` per page on a single thread, a 1,000-page documentation portal processes through the entire Markdown, Handlebars, and HTML5 minification pipeline in **~28 milliseconds**.

---

## 3. Ecosystem Comparison

How NucleusFlow compares against established static site generators and content engines:

| Feature / Dimension | **NucleusFlow (`v0.0.11`)** | **Zola** | **Hugo** | **Eleventy (11ty)** |
| :--- | :--- | :--- | :--- | :--- |
| **Implementation Language** | Rust (`1.89.0+`) | Rust | Go | JavaScript (Node.js) |
| **Memory Safety Guarantee** | `#![forbid(unsafe_code)]` | Standard Rust | Go GC | V8 GC |
| **Usage Model** | **Library Crate + CLI Binary** | CLI Binary only | CLI Binary only | Node CLI / NPM package |
| **Template Engine** | Handlebars (`.hbs`) | Tera (`.html`) | Go `html/template` | Liquid / Nunjucks / Hbs |
| **Frontmatter Parser** | YAML 1.2 (`noyalib`) | TOML (`+++`) / YAML | YAML / TOML / JSON | YAML / JSON / JS |
| **Built-in HTML Sanitizer** | Yes (`ALLOWED_HTML_TAGS` + XSS filter) | No | Partial | Via external plugins |
| **Built-in HTML5 Minifier** | Yes (`minify-html` in `~8.1 µs`) | Yes | Yes | Via external plugins |
| **Bounded DoS Budgets** | Explicit content, template &amp; cache caps | Unbounded | Unbounded | Unbounded |

---

## 4. Reproducing Benchmarks Locally

Clone the core library repository and run the Criterion benchmark suite:

```bash
git clone https://github.com/sebastienrousseau/nucleusflow.git
cd nucleusflow
cargo bench --bench processing_benchmark
```

HTML latency distribution plots and regression comparisons are written automatically to `target/criterion/report/index.html`.
