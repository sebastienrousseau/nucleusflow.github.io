---
name: "NucleusFlow"
short_name: "nucleusflow"
title: "NucleusFlow: Fast, Secure Rust Static Site Generator & Content Pipeline"
description: "Fast, flexible, and secure static site generator and content processing pipeline in Rust. Parse CommonMark + GFM, render cached Handlebars templates, and minify HTML5 in ~28 microseconds per page with zero unsafe code."
keywords: "nucleusflow, rust static site generator, content processing pipeline, rust markdown parser, handlebars template engine, pulldown-cmark, noyalib, html minification, static site builder, rust cms pipeline"
author: "Sebastien Rousseau"
date: "2026-10-31"
language: "en-GB"
layout: "index"
permalink: "https://nucleusflow.com/"
logo: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
banner: "https://cloudcdn.pro/stocks/images/corporate-finance-1200.webp"
banner_alt: "NucleusFlow - Fast, Secure Rust Static Site Generator & Content Pipeline"
---



<section class="visage-intro" aria-labelledby="hero-heading">
<div class="wrap">
<p class="lilac-label">Rust Content Processing · Open Source</p>
<h1 id="hero-heading">Compile static pages in <em>microseconds.</em><br>Zero unsafe code. Zero telemetry.</h1>
<p>An open-source, high-throughput static site generator and content processing pipeline engineered in pure memory-safe Rust. Converts CommonMark + GFM with YAML 1.2 frontmatter and cached Handlebars templates into validated, minified HTML5 in ~28.0 µs per page as both an embeddable crate and standalone CLI.</p>
<div class="actions">
<a class="primary" href="/getting-started/index.html">Install CLI &amp; Crate</a>
<a href="/formats/index.html">Explore Supported Formats</a>
</div>
<ul class="hero-assurances">
<li>#![forbid(unsafe_code)] Rust Core</li>
<li>~28.0 µs Single-Page Pipeline</li>
<li>Library Crate + Standalone CLI</li>
</ul>
</div>
</section>

<section class="trust-strip" aria-label="Platform guarantees">
<div class="wrap">
<ul>
<li>
<b>~28.0 µs / Page</b>
<span>End-to-end read, Markdown parse, Handlebars render, HTML5 minification, and write.</span>
</li>
<li>
<b>~4.2 µs Markdown + YAML</b>
<span>CommonMark + GFM tables/footnotes (`pulldown-cmark`) and YAML 1.2 frontmatter (`noyalib`).</span>
</li>
<li>
<b>~12.5 µs Template Render</b>
<span>Thread-safe `parking_lot::RwLock` compiled Handlebars template cache with partials and helpers.</span>
</li>
<li>
<b>Zero Unsafe Code</b>
<span>Crate-wide `#![forbid(unsafe_code)]`, HTML allowlist sanitization, and path traversal rejection.</span>
</li>
</ul>
</div>
</section>

<section class="analysis-stage" id="workflows" aria-labelledby="workflows-heading">
<div class="wrap image-shell">
<picture>
<source media="(max-width: 40rem)" srcset="https://cloudcdn.pro/stocks/images/getty-images-LaU3HadwEeE-unsplash-320.webp 320w, https://cloudcdn.pro/stocks/images/getty-images-LaU3HadwEeE-unsplash-640.webp 640w, https://cloudcdn.pro/stocks/images/getty-images-LaU3HadwEeE-unsplash-1200.webp 1200w" sizes="100vw">
<img src="https://cloudcdn.pro/stocks/images/getty-images-LaU3HadwEeE-unsplash-1200.webp" srcset="https://cloudcdn.pro/stocks/images/getty-images-LaU3HadwEeE-unsplash-640.webp 640w, https://cloudcdn.pro/stocks/images/getty-images-LaU3HadwEeE-unsplash-1200.webp 1200w, https://cloudcdn.pro/stocks/images/getty-images-LaU3HadwEeE-unsplash-1920.webp 1920w" sizes="(max-width: 40rem) 100vw, 80rem" width="1200" height="675" alt="High-throughput Rust content pipeline and static site compilation dashboard" loading="lazy" decoding="async">
</picture>
<div class="consultation-card">
<p class="lilac-label">Composable Rust Architecture</p>
<h2 id="workflows-heading">Embeddable Pipeline for Modern Engineering</h2>
<p>Stop maintaining separate Markdown parsers for backend services and monolithic binaries for documentation portals. Share the exact same trait-driven Rust pipeline across your CLI builds, CI/CD jobs, and web microservices.</p>
<a class="primary" href="/use-cases/index.html">View Production Workflows <span aria-hidden="true">↗</span></a>
</div>
</div>
</section>

<section class="visage-features" id="features" aria-labelledby="features-heading">
<div class="wrap">
<header class="section-heading">
<p class="lilac-label">Engine Architecture</p>
<h2 id="features-heading">Engineered for deterministic safety.</h2>
<p>Three-stage trait pipeline combining memory-safe Rust execution, bounded DoS budgets, and dual Apache-2.0 or MIT licensing.</p>
</header>
<div class="three-cells">
<article>
<span>01</span>
<h3>ContentProcessor (Stage 1)</h3>
<p>Extracts YAML 1.2 frontmatter via noyalib, parses CommonMark + GFM tables, footnotes, and strikethrough via pulldown-cmark, generates semantic TOC navigation, and enforces an explicit HTML tag allowlist.</p>
</article>
<article>
<span>02</span>
<h3>HandlebarsRenderer (Stage 2)</h3>
<p>Compiles and caches .hbs templates in a thread-safe parking_lot::RwLock registry. Supports nested partials, custom TemplateHelper implementations, balanced-brace checks, and strict variable validation.</p>
</article>
<article>
<span>03</span>
<h3>HtmlGenerator (Stage 3)</h3>
<p>Verifies HTML5 structural tag balance across void and optional-close elements, injects escaped meta tags into head, minifies HTML/CSS/JS in ~8.1 µs via minify-html, and manages static asset caching.</p>
</article>
</div>
</div>
</section>

<section class="visage-process" id="process" aria-labelledby="process-heading">
<div class="wrap">
<p class="lilac-label">The Processing Pipeline</p>
<h2 id="process-heading">From Markdown source to minified HTML5.</h2>
<div class="process-grid">
<article>
<b>1</b>
<h3>Extract</h3>
<p>Validate 10 MB content budgets, sanitize file paths, and parse YAML 1.2 frontmatter into Schema.org JSON-LD metadata.</p>
<small>noyalib 0.0.58 frontmatter engine</small>
</article>
<article>
<b>2</b>
<h3>Parse</h3>
<p>Transform CommonMark + GFM tables, footnotes, and strikethrough into sanitized HTML with automated Table of Contents.</p>
<small>~4.2 µs pulldown-cmark 0.13.5</small>
</article>
<article>
<b>3</b>
<h3>Render</h3>
<p>Evaluate compiled .hbs Handlebars layouts, partials, and custom helpers from the concurrent RwLock cache.</p>
<small>~12.5 µs handlebars 6.2 render</small>
</article>
<article>
<b>4</b>
<h3>Minify</h3>
<p>Verify HTML5 tag balance, inject escaped meta tags, and collapse HTML, inline CSS, and JS whitespace.</p>
<small>~8.1 µs minify-html 0.18.1</small>
</article>
</div>
</div>
</section>

<section class="analysis-stage" id="architecture-preview" aria-labelledby="arch-heading">
<div class="wrap image-shell">
<picture>
<source media="(max-width: 40rem)" srcset="https://cloudcdn.pro/stocks/images/vadim-sherbakov-xS_RzdD5CFE-320.webp 320w, https://cloudcdn.pro/stocks/images/vadim-sherbakov-xS_RzdD5CFE-640.webp 640w, https://cloudcdn.pro/stocks/images/vadim-sherbakov-xS_RzdD5CFE-1200.webp 1200w" sizes="100vw">
<img src="https://cloudcdn.pro/stocks/images/vadim-sherbakov-xS_RzdD5CFE-1200.webp" srcset="https://cloudcdn.pro/stocks/images/vadim-sherbakov-xS_RzdD5CFE-640.webp 640w, https://cloudcdn.pro/stocks/images/vadim-sherbakov-xS_RzdD5CFE-1200.webp 1200w, https://cloudcdn.pro/stocks/images/vadim-sherbakov-xS_RzdD5CFE-1920.webp 1920w" sizes="(max-width: 40rem) 100vw, 80rem" width="1200" height="675" alt="London architectural skyline at night representing high-assurance Rust infrastructure" loading="lazy" decoding="async">
</picture>
<div class="consultation-card">
<p class="lilac-label">High-Performance Pipeline</p>
<h2 id="arch-heading">Deterministic Execution at Enterprise Scale</h2>
<p>Engineered for documentation portals and backend services requiring bounded memory budgets, lock-free read concurrency, and zero unsafe blocks.</p>
<a class="primary" href="/architecture/index.html">View Architecture Blueprint <span aria-hidden="true">↗</span></a>
</div>
</div>
</section>

<section class="terminal-stage" aria-labelledby="quickstart-heading">
<div class="wrap terminal-grid">
<div>
<p class="lilac-label">Developer Quickstart</p>
<h2 id="quickstart-heading">Install via Cargo, Homebrew, or Nix in Seconds</h2>
<p>Run as a standalone CLI binary to scaffold, build, and live-serve static sites, or add the crate to your Cargo.toml to embed the pipeline in Rust.</p>
<div class="actions">
<a class="primary" href="/getting-started/index.html">Full Documentation →</a>
<a href="/api/index.html">API &amp; CLI Reference</a>
</div>
</div>
<div class="terminal-box">
<div class="terminal-bar">
<span class="terminal-dot dot-red"></span>
<span class="terminal-dot dot-yellow"></span>
<span class="terminal-dot dot-green"></span>
<span class="terminal-title">bash - nucleusflow</span>
</div>
<div class="terminal-body">
<div class="t-line"><span class="t-comment">&#35; 1. Install standalone CLI and Rust crate (v0.0.11)</span></div>
<div class="t-line"><span class="t-prompt">$</span>cargo install nucleusflow --locked</div>
<div class="t-line"><span class="t-prompt">$</span>nucleusflow new docs-portal --template docs</div>
<div class="t-line"><span class="t-comment">&#35; 2. Compile CommonMark + Handlebars with HTML5 minification</span></div>
<div class="t-line"><span class="t-prompt">$</span>nucleusflow build --content-dir content --output-dir public --minify</div>
<div class="t-line"><span class="t-comment">&#35; 3. Deterministic pipeline output (~28.0 µs / page)</span></div>
<div class="t-line">{ <span class="t-key">&quot;status&quot;</span>: <span class="t-val">&quot;COMPLETED&quot;</span>, <span class="t-key">&quot;latency_us&quot;</span>: <span class="t-val">28.0</span>, <span class="t-key">&quot;unsafe_blocks&quot;</span>: <span class="t-val">0</span> }</div>
</div>
</div>
</div>
</section>

<section class="privacy-panel" aria-labelledby="privacy-heading">
<div class="wrap privacy-grid">
<p class="lilac-label">Defense-in-Depth Security</p>
<div>
<h2 id="privacy-heading">Memory-safe Rust with strict input boundaries.</h2>
<p>NucleusFlow enforces #![forbid(unsafe_code)] across every module (ADR-0001), rejects path traversal via property-tested sanitize_path (ADR-0003), sanitizes HTML against an explicit tag allowlist, and caps input budgets with zero outbound network calls.</p>
</div>
<a href="/security/index.html">Read Security Architecture <span aria-hidden="true">↓</span></a>
</div>
</section>

<section class="visage-faq" id="faq" aria-labelledby="faq-heading">
<div class="wrap faq-layout">
<header>
<p class="lilac-label">Clear Answers</p>
<h2 id="faq-heading">Frequently Asked Questions</h2>
</header>
<div>
<details>
<summary>Can I use NucleusFlow as a Rust library as well as a CLI binary?</summary>
<p>Yes. Unlike monolithic static site binaries such as Hugo or Zola, NucleusFlow is published on Crates.io as both a library crate (nucleusflow) and a standalone CLI binary. You can embed ContentProcessor, HandlebarsRenderer, HtmlGenerator, or the NucleusFlow orchestrator directly inside your Rust services.</p>
</details>
<details>
<summary>Which Markdown, frontmatter, and template formats are supported?</summary>
<p>NucleusFlow parses CommonMark plus GitHub Flavored Markdown (tables, footnotes, strikethrough) via pulldown-cmark 0.13.5, extracts YAML 1.2 frontmatter via noyalib 0.0.58, compiles Handlebars (.hbs) templates via handlebars 6.2, and minifies HTML5 via minify-html 0.18.1.</p>
</details>
<details>
<summary>What is the Minimum Supported Rust Version (MSRV)?</summary>
<p>NucleusFlow requires Rust 1.89.0 or newer (2021 Edition). This floor is enforced at compile time via Cargo.toml and build.rs and verified in CI.</p>
</details>
<details>
<summary>Is NucleusFlow open source and available for commercial use?</summary>
<p>Yes. NucleusFlow is dual-licensed under the Apache-2.0 and MIT open-source licenses, allowing commercial and open-source embedding without copyleft restrictions.</p>
</details>
</div>
</div>
</section>

<section class="visage-closing" aria-labelledby="visage-close">
<div class="wrap">
<div>
<p class="lilac-label">Developer Quickstart</p>
<h2 id="visage-close">Content pipelines are core infrastructure.<br><em>Yours should be memory-safe and instant.</em></h2>
</div>
<div>
<p>Get started in minutes with the Rust library crate or CLI binary across Linux, macOS, and Windows.</p>
<a class="primary" href="/getting-started/index.html">Install NucleusFlow</a>
</div>
</div>
</section>
