---
author: "Sebastien Rousseau"
title: "Security & Memory Safety: Zero Unsafe Rust & XSS Defense"
description: "NucleusFlow security architecture: #![forbid(unsafe_code)], property-tested path traversal rejection, HTML allowlist sanitization, bounded DoS budgets, and supply-chain verification."
keywords: "nucleusflow security, rust forbid unsafe_code, markdown xss sanitization, path traversal protection rust, cargo-deny audit, static site security"
date: "2026-10-31T00:00:00+00:00"
updated: "2026-10-31T00:00:00+00:00"
apple_status_bar_style: "black-translucent"
apple-touch-icon: "https://cloudcdn.pro/nucleusflow/v1/icons/192x192.png"
banner: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
canonical: "https://nucleusflow.com/security/index.html"
Category: "Documentation"
item_url: "https://nucleusflow.com/security/index.html"
lang: "en-GB"
eyebrow: "Defense in Depth"
headline: "Security & Memory-Safe Rust"
lead: "Crate-wide #![forbid(unsafe_code)], path traversal prevention, and HTML sanitization."
layout: "page"
locale: "en_GB"
logo: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
manifest: "/manifest.json"
name: "NucleusFlow"
permalink: "https://nucleusflow.com/security/index.html"
robots: "index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1"
rss: "/rss.xml"
short_name: "nucleusflow"
subtitle: "Defense-in-depth memory safety, input sanitization, and supply-chain provenance."
theme_color: "#11355c"
twitter_card: "summary_large_image"
url: "https://nucleusflow.com/security/index.html"
viewport: "width=device-width, initial-scale=1, shrink-to-fit=no"
---

## 1. Crate-Wide `#![forbid(unsafe_code)]` (ADR-0001)

Both `src/lib.rs` and `src/main.rs` declare `#![forbid(unsafe_code)]` at the crate root. Any attempt to introduce an `unsafe` block fails compilation immediately. Combined with `cargo-deny` and `cargo-audit` in CI, NucleusFlow eliminates buffer overflows, use-after-free bugs, and undefined memory behavior across its content processing pipeline.

---

## 2. Path Traversal Prevention (ADR-0003)

Static site generators read content files, templates, and static assets from disk and mirror directory structures into output trees. NucleusFlow enforces strict path validation at every I/O boundary:

* **`FileContentProcessor` (`sanitize_path`)**: Rejects any path containing `..` parent traversal segments, null bytes (`\0`), or leading `/` root escapes before opening a file, returning `ProcessError::InvalidInput`.
* **`HtmlGenerator::copy_assets_to`**: Verifies that every cached relative asset path contains no `..` components before creating subdirectories or writing output files.
* **Property-Based Fuzzing (`proptest 1.6`)**: Path sanitization invariants are continuously fuzz-tested against arbitrary string inputs in the test suite.

---

## 3. HTML Allowlist Sanitization &amp; XSS Prevention

When `ContentProcessor::new().with_sanitization()` is active, `MarkdownProcessor` enforces a two-layer filter on rendered HTML:

1. **Tag Allowlist (`ALLOWED_HTML_TAGS`)**: Only permits `p`, `div`, `span`, `h1`, `h2`, `h3`, `h4`, `h5`, `h6`, `ul`, `ol`, `li`, `a`, `img`, `code`, `pre`, `blockquote`, `em`, `strong`, `del`, `table`, `thead`, `tbody`, `tr`, `th`, `td`, `sup`, `sub`, `br`, and `hr`. Any unapproved tag (such as `<script>`, `<iframe>`, `<object>`, `<embed>`, `<form>`, or `<style>`) triggers `ProcessingError::HtmlSanitizationError`.
2. **Suspicious Pattern Blocker**: Scans for `javascript:`, `data:`, `vbscript:`, `onclick`, `onerror`, `onload`, and `eval(`, blocking dangerous URI schemes and inline event handlers.
3. **Escaped `<meta>` Injection**: `HtmlGenerator` escapes `&`, `<`, `>`, `"`, and `'` in all `<meta>` tag names and values before inserting them into `<head>`.

---

## 4. Deterministic Denial-of-Service (DoS) Budgets

Every stage enforces explicit upper bounds so untrusted or malformed files cannot exhaust process memory:

| Resource Budget | Default Limit | Enforced By |
| :--- | :--- | :--- |
| **Markdown Content Size** | `10 MB` (`10,485,760` bytes) | `MarkdownProcessor::MAX_CONTENT_SIZE` &amp; `ContentConfig` |
| **Frontmatter Metadata Size** | `64 KB` (`65,536` bytes) | `ContentConfig::max_metadata_size` |
| **Template File Size** | `1 MB` (`1,048,576` bytes) | `TemplateConfig::max_template_size` |
| **Template Cache Memory** | `100 MB` (`104,857,600` bytes) | `TemplateConfig::max_cache_size` |
| **Configuration File Size** | `1 MB` (`1,048,576` bytes) | `ConfigBuilder::MAX_CONFIG_SIZE` |
| **Generated Output Size** | `100 MB` (`104,857,600` bytes) | `OutputConfig::max_output_size` |
| **Environment Variable Filter** | Blocks `PATH`, `HOME`, `USER`, `SHELL` | `ConfigBuilder::is_safe_env_var` |

---

## 5. Vulnerability Disclosure Policy (RFC 9116)

Report suspected vulnerabilities privately via [GitHub Security Advisories](https://github.com/sebastienrousseau/nucleusflow/security/advisories/new) or by email to `sebastian.rousseau@gmail.com`. Machine-readable disclosure metadata is published at [`/.well-known/security.txt`](/.well-known/security.txt). Initial acknowledgement is provided within **48 hours**.
