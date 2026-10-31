---
author: "Sebastien Rousseau"
title: "Use Cases: Documentation Portals, Embedded Pipelines & CI/CD Publishing"
description: "Production patterns for NucleusFlow: technical documentation portals, embedded Rust CMS pipelines, automated CI/CD release notes, and sanitized user-content rendering."
keywords: "nucleusflow use cases, rust documentation generator, embedded markdown pipeline rust, ci cd static site build, sanitized markdown rendering"
date: "2026-10-31T00:00:00+00:00"
updated: "2026-10-31T00:00:00+00:00"
apple_status_bar_style: "black-translucent"
apple-touch-icon: "https://cloudcdn.pro/nucleusflow/v1/icons/192x192.png"
banner: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
canonical: "https://nucleusflow.com/use-cases/index.html"
Category: "Documentation"
item_url: "https://nucleusflow.com/use-cases/index.html"
lang: "en-GB"
eyebrow: "Production Workflows"
headline: "Real-World Use Cases & Integration Patterns"
lead: "How engineering teams deploy NucleusFlow across web portals and backend services."
layout: "page"
locale: "en_GB"
logo: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
manifest: "/manifest.json"
name: "NucleusFlow"
permalink: "https://nucleusflow.com/use-cases/index.html"
robots: "index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1"
rss: "/rss.xml"
short_name: "nucleusflow"
subtitle: "How engineering teams deploy NucleusFlow across web portals and backend services."
theme_color: "#11355c"
twitter_card: "summary_large_image"
url: "https://nucleusflow.com/use-cases/index.html"
viewport: "width=device-width, initial-scale=1, shrink-to-fit=no"
---

## 1. Technical Documentation Portals &amp; Developer Hubs

Engineering organizations use the `nucleusflow` CLI (`--template docs`) to compile multi-version product manuals, ADR catalogs, and API guides with zero runtime JavaScript dependencies.

* **Automated Table of Contents (`with_toc`)**: Every documentation page automatically receives a semantic `<nav class="toc">` anchor tree derived from `h1`..`h6` headings.
* **Schema.org JSON-LD Structured Data**: YAML 1.2 frontmatter fields are automatically emitted as `<script type="application/ld+json">` metadata for search engine and LLM indexing.
* **Sub-30 µs Page Compilation**: Rebuild hundreds of documentation pages in milliseconds during local `nucleusflow serve --watch` editing sessions.

---

## 2. Embedded Content Pipelines in Rust Web Services

Because NucleusFlow is architected as a library crate (`nucleusflow = "0.0.11"`) first and a CLI second, Rust web servers (Axum, Actix-Web, Tower) can embed `ContentProcessor`, `HandlebarsRenderer`, and `HtmlGenerator` directly in-process:

```rust
use std::sync::Arc;
use parking_lot::RwLock;
use nucleusflow::{ContentProcessor, HandlebarsRenderer, HtmlGenerator};
use nucleusflow::core::traits::Shareable;

pub struct ContentEngine {
    pub processor: Arc<RwLock<ContentProcessor>>,
    pub renderer: Arc<RwLock<HandlebarsRenderer>>,
    pub generator: Arc<RwLock<HtmlGenerator>>,
}

impl ContentEngine {
    pub fn new() -> Self {
        Self {
            processor: ContentProcessor::new().with_toc().with_sanitization().into_shared(),
            renderer: HandlebarsRenderer::new().with_strict_mode().into_shared(),
            generator: HtmlGenerator::new().with_minification().into_shared(),
        }
    }
}
```

---

## 3. Sanitized User-Submitted Markdown Rendering

Platforms that accept Markdown from external contributors, webhooks, or CMS editors require strict protection against Cross-Site Scripting (XSS) and resource exhaustion:

* **Explicit Tag Allowlist**: `ContentProcessor::new().with_sanitization()` restricts inline HTML to `ALLOWED_HTML_TAGS` (`p`, `div`, `span`, `h1`..`h6`, `ul`, `ol`, `li`, `a`, `img`, `code`, `pre`, `blockquote`, `em`, `strong`, `del`, `table`, `thead`, `tbody`, `tr`, `th`, `td`, `sup`, `sub`, `br`, `hr`).
* **Active Pattern Blocking**: Rejects `javascript:`, `data:`, `vbscript:`, inline event handlers (`onclick`, `onerror`, `onload`), and `eval(` expressions with `ProcessingError::HtmlSanitizationError`.
* **10 MB Input Ceiling**: Prevents denial-of-service memory spikes before invoking the parser.

---

## 4. High-Speed CI/CD Static Publishing &amp; Changelogs

Run `nucleusflow build --minify` inside ephemeral CI runners or distroless containers to compile release notes, security advisories, and compliance reports into static HTML5 artifacts with `0o644` permissions and verified tag balance.
