---
author: "Sebastien Rousseau"
title: "Pipeline Architecture: Three-Stage Trait Design & Thread Safety"
description: "Deep dive into NucleusFlow's three-stage Rust architecture: ContentProcessor, HandlebarsRenderer with parking_lot::RwLock caching, HtmlGenerator, and Architectural Decision Records (ADRs)."
keywords: "nucleusflow architecture, rust static site generator design, trait-based content pipeline, parking_lot RwLock template cache, rust ADR, memory-safe ssg"
date: "2026-10-31T00:00:00+00:00"
updated: "2026-10-31T00:00:00+00:00"
apple_status_bar_style: "black-translucent"
apple-touch-icon: "https://cloudcdn.pro/nucleusflow/v1/icons/192x192.png"
banner: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
canonical: "https://nucleusflow.com/architecture/index.html"
Category: "Documentation"
item_url: "https://nucleusflow.com/architecture/index.html"
lang: "en-GB"
eyebrow: "System Design"
headline: "Three-Stage Pipeline Architecture"
lead: "How NucleusFlow processes Markdown, templates, and HTML5 with deterministic concurrency."
layout: "page"
locale: "en_GB"
logo: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
manifest: "/manifest.json"
name: "NucleusFlow"
permalink: "https://nucleusflow.com/architecture/index.html"
robots: "index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1"
rss: "/rss.xml"
short_name: "nucleusflow"
subtitle: "How NucleusFlow processes Markdown, templates, and HTML5 with deterministic concurrency."
theme_color: "#11355c"
twitter_card: "summary_large_image"
url: "https://nucleusflow.com/architecture/index.html"
viewport: "width=device-width, initial-scale=1, shrink-to-fit=no"
---

## 1. End-to-End Execution Flow

NucleusFlow decouples static site generation into three composable stages coordinated by `NucleusFlow`:

```text
+-------------------------------------------------------------------------+
|                        Configuration & CLI Layer                        |
|   ConfigBuilder (nucleusflow.toml + NUCLEUSFLOW_* env) / clap 4.5 CLI   |
+------------------------------------+------------------------------------+
                                     |
                                     v
+-------------------------------------------------------------------------+
|                    Stage 1: ContentProcessor (Processor)                |
|  1. Enforce 10 MB input cap & path traversal sanitization               |
|  2. Extract YAML 1.2 frontmatter (noyalib 0.0.58)                       |
|  3. Parse CommonMark + GFM tables/footnotes (pulldown-cmark 0.13.5)     |
|  4. Synthesize <nav class="toc"> & Schema.org JSON-LD metadata          |
|  5. Enforce ALLOWED_HTML_TAGS & block XSS attribute/URI patterns        |
+------------------------------------+------------------------------------+
                                     |
                                     v
+-------------------------------------------------------------------------+
|                 Stage 2: HandlebarsRenderer (TemplateRenderer)          |
|  1. Validate balanced braces & strict-mode context variables            |
|  2. Read compiled .hbs templates from Arc<parking_lot::RwLock<...>>     |
|  3. Evaluate partials and custom TemplateHelper implementations         |
+------------------------------------+------------------------------------+
                                     |
                                     v
+-------------------------------------------------------------------------+
|                     Stage 3: HtmlGenerator (Generator)                  |
|  1. Verify HTML5 structural tag balance (void & optional-close aware)   |
|  2. Inject HTML-escaped <meta> tags into <head>                         |
|  3. Minify HTML/CSS/JS (minify-html 0.18.1) or pretty-print             |
|  4. Copy cached static assets & write output files (0o644 permissions)  |
+-------------------------------------------------------------------------+
```

---

## 2. Crate Module Layout

The `nucleusflow` workspace is organized into focused modules with strict complexity ceilings (`<= 10` cyclomatic complexity, `<= 15` cognitive complexity per function):

| Source File | Responsibility |
| :--- | :--- |
| `src/lib.rs` | Top-level `NucleusFlow` orchestrator, `NucleusFlowConfig`, crate-level `#![forbid(unsafe_code)]`, and public re-exports |
| `src/main.rs` | Binary entrypoint initializing structured logging (`dtt` + `log`) and dispatching CLI subcommands |
| `src/cli.rs` | Declarative `clap` 4.5 command-line parser (`new`, `build`, `serve`) and argument validation |
| `src/core/config.rs` | `Config`, `ConfigBuilder`, `Profile`, `ContentConfig`, `TemplateConfig`, `OutputConfig`, and safe environment variable merging |
| `src/core/traits.rs` | Core abstraction traits (`Processor`, `Transform`, `Generator`, `Validator`, `ProcessingContext`, `Shareable`) |
| `src/core/error.rs` | Structured `ProcessingError` enum (`thiserror` 2.0) covering all validation, parsing, template, and I/O failures |
| `src/process.rs` | `FileContentProcessor` with path sanitization (`sanitize_path`), file extension allowlists, and `ProcessError` |
| `src/processors/markdown.rs` | `MarkdownProcessor` implementing CommonMark + GFM parsing, YAML frontmatter extraction, TOC generation, and HTML sanitization |
| `src/template.rs` | `HtmlTemplateRenderer` wrapping `handlebars::Handlebars` in `Arc<parking_lot::RwLock<...>>`, partial management, and `UppercaseHelper` |
| `src/generators/html.rs` | `HtmlOutputGenerator` implementing HTML5 tag balance verification, `<meta>` injection, `minify-html` integration, and asset caching |

---

## 3. Concurrency &amp; `parking_lot::RwLock` Caching

Static site builds are heavily read-biased: templates and configuration are compiled once at startup and read concurrently across hundreds or thousands of content pages.

* **Zero Poisoning Overhead**: NucleusFlow uses `parking_lot::RwLock` (`0.13`) rather than `std::sync::RwLock`, avoiding lock poisoning overhead and providing compact, fast uncontended read locks.
* **Blanket `Shareable` Trait**: Every component implementing `Send + Sync + 'static` automatically gains `.into_shared()`, wrapping the instance in `Arc<RwLock<Self>>` for multi-threaded pipeline execution.
* **Async Runtime Compatibility**: `ConfigBuilder` supports asynchronous reload tasks on `tokio` (`1.47`) while keeping the core parsing and rendering path synchronous and allocation-lean.

---

## 4. Architectural Decision Records (ADRs)

All load-bearing architectural invariants in the `nucleusflow` repository are documented under `docs/adr/`:

| ADR | Title | Status | Core Invariant |
| :--- | :--- | :--- | :--- |
| **ADR-0001** | Zero Unsafe Rust Policy | Accepted | Enforces `#![forbid(unsafe_code)]` at the crate roots (`src/lib.rs` and `src/main.rs`) and audits dependencies via `cargo-deny`. |
| **ADR-0002** | Deterministic Rendering Pipeline | Accepted | Separates content parsing, template evaluation, and HTML generation behind deterministic traits synchronized with `parking_lot::RwLock`. |
| **ADR-0003** | Path Traversal Rejection | Accepted | Rejects `..`, null bytes, and leading slashes in `sanitize_path` before any filesystem read or write, verified by `proptest` fuzzing. |
