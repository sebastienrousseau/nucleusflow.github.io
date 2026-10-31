---
name: "NucleusFlow"
short_name: "nucleusflow"
title: "Frequently Asked Questions (FAQ) | NucleusFlow"
description: "Answers to common questions about NucleusFlow: Rust MSRV (1.89.0), embedding the crate vs using the CLI, Handlebars vs Tera/Go templates, HTML sanitization, and licensing."
keywords: "nucleusflow faq, rust static site generator questions, nucleusflow vs zola, nucleusflow vs hugo, handlebars rust template engine"
author: "Sebastien Rousseau"
date: "2026-10-31"
language: "en-GB"
layout: "faqs"
permalink: "https://nucleusflow.com/faqs/index.html"
logo: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
banner: "https://cloudcdn.pro/stocks/images/quantum-computer-room-1200.webp"
eyebrow: "Questions & Answers"
headline: "Frequently Asked Questions"
lead: "Technical guidance on NucleusFlow's three-stage Rust architecture, supported formats, microsecond benchmarks, and dual licensing."
---



<div class="faq-container">
<div class="faq-search-box">
<span class="faq-search-icon" aria-hidden="true"></span>
<input type="search" class="faq-search-input" id="faqSearchInput" placeholder="Filter questions by keyword (e.g. Handlebars, YAML, MSRV, Sanitization, CLI)..." aria-label="Search frequently asked questions" />
</div>

<div class="faq-filter-pills" role="tablist" aria-label="FAQ Categories">
<button class="faq-pill active" data-filter="all" type="button">All Questions <span class="pill-count">8</span></button>
<button class="faq-pill" data-filter="architecture" type="button">Architecture &amp; API <span class="pill-count">3</span></button>
<button class="faq-pill" data-filter="formats" type="button">Formats &amp; Templates <span class="pill-count">3</span></button>
<button class="faq-pill" data-filter="security" type="button">Security &amp; Licensing <span class="pill-count">2</span></button>
</div>

<div class="faq-action-bar">
<span class="faq-count-label" id="faqCountLabel">Showing 8 of 8 questions</span>
<button type="button" class="faq-toggle-all-btn" id="faqToggleAllBtn" aria-expanded="false">
<span class="btn-text">Expand all</span>
<span class="chevron" aria-hidden="true"></span>
</button>
</div>

<div class="faq-list" id="faqList">
<details class="faq-card" data-category="architecture">
<summary class="faq-summary">
<span class="faq-question-wrap">
<span class="faq-category-badge">Architecture</span>
<span class="faq-question">Can I use NucleusFlow as a Rust library inside my own application, or is it only a CLI?</span>
</span>
<span class="faq-icon" aria-hidden="true"></span>
</summary>
<div class="faq-answer">
<p>Both. Unlike monolithic static site binaries such as Hugo or Zola, NucleusFlow is published on Crates.io as both a library crate (<code>nucleusflow</code>) and a CLI binary. You can import <code>ContentProcessor</code>, <code>HandlebarsRenderer</code>, <code>HtmlGenerator</code>, or the top-level <code>NucleusFlow</code> orchestrator directly into your Rust services.</p>
</div>
</details>

<details class="faq-card" data-category="architecture">
<summary class="faq-summary">
<span class="faq-question-wrap">
<span class="faq-category-badge">Toolchain</span>
<span class="faq-question">What is the Minimum Supported Rust Version (MSRV)?</span>
</span>
<span class="faq-icon" aria-hidden="true"></span>
</summary>
<div class="faq-answer">
<p>NucleusFlow requires <strong>Rust 1.89.0</strong> or newer (2021 Edition). This floor is verified in CI and enforced at build time by <code>Cargo.toml</code> and <code>build.rs</code>.</p>
</div>
</details>

<details class="faq-card" data-category="architecture">
<summary class="faq-summary">
<span class="faq-question-wrap">
<span class="faq-category-badge">Concurrency</span>
<span class="faq-question">Is NucleusFlow thread-safe for concurrent rendering?</span>
</span>
<span class="faq-icon" aria-hidden="true"></span>
</summary>
<div class="faq-answer">
<p>Yes. Every core stage implements <code>Send + Sync</code> and the blanket <code>Shareable</code> trait (<code>.into_shared()</code>), wrapping instances in <code>Arc&lt;parking_lot::RwLock&lt;Self&gt;&gt;</code> so compiled Handlebars templates and assets can be shared safely across worker threads.</p>
</div>
</details>

<details class="faq-card" data-category="formats">
<summary class="faq-summary">
<span class="faq-question-wrap">
<span class="faq-category-badge">Markdown</span>
<span class="faq-question">Which Markdown extensions and frontmatter formats are supported?</span>
</span>
<span class="faq-icon" aria-hidden="true"></span>
</summary>
<div class="faq-answer">
<p><code>MarkdownProcessor</code> parses CommonMark plus GitHub Flavored Markdown (GFM tables, footnotes, and strikethrough) via <code>pulldown-cmark</code> 0.13.5, and extracts <code>---</code> fenced YAML 1.2 frontmatter via <code>noyalib</code> 0.0.58.</p>
</div>
</details>

<details class="faq-card" data-category="formats">
<summary class="faq-summary">
<span class="faq-question-wrap">
<span class="faq-category-badge">Templating</span>
<span class="faq-question">Why does NucleusFlow use Handlebars (.hbs) templates?</span>
</span>
<span class="faq-icon" aria-hidden="true"></span>
</summary>
<div class="faq-answer">
<p>Handlebars enforces logic-less templates with clear separation between presentation layouts and Rust business logic, while supporting partials, custom <code>TemplateHelper</code> functions, and strict variable checking via the battle-tested <code>handlebars</code> 6.2 crate.</p>
</div>
</details>

<details class="faq-card" data-category="formats">
<summary class="faq-summary">
<span class="faq-question-wrap">
<span class="faq-category-badge">HTML5</span>
<span class="faq-question">Does NucleusFlow validate and minify HTML5 output automatically?</span>
</span>
<span class="faq-icon" aria-hidden="true"></span>
</summary>
<div class="faq-answer">
<p>Yes. <code>HtmlGenerator</code> verifies structural HTML5 tag balance (accounting for void elements like <code>img</code> and <code>meta</code> as well as optional-closing elements like <code>p</code> and <code>li</code>) and minifies HTML, inline CSS, and inline JS in <code>~8.1 µs</code> using <code>minify-html</code> 0.18.1.</p>
</div>
</details>

<details class="faq-card" data-category="security">
<summary class="faq-summary">
<span class="faq-question-wrap">
<span class="faq-category-badge">Safety</span>
<span class="faq-question">Does NucleusFlow contain any unsafe Rust code?</span>
</span>
<span class="faq-icon" aria-hidden="true"></span>
</summary>
<div class="faq-answer">
<p>No. Both the library crate and CLI binary enforce <code>#![forbid(unsafe_code)]</code> at the crate root per ADR-0001.</p>
</div>
</details>

<details class="faq-card" data-category="security">
<summary class="faq-summary">
<span class="faq-question-wrap">
<span class="faq-category-badge">Licensing</span>
<span class="faq-question">How is NucleusFlow licensed?</span>
</span>
<span class="faq-icon" aria-hidden="true"></span>
</summary>
<div class="faq-answer">
<p>NucleusFlow and its documentation are dual-licensed under your choice of the <strong>Apache License, Version 2.0</strong> (<code>LICENSE-APACHE</code>) or the <strong>MIT License</strong> (<code>LICENSE-MIT</code>).</p>
</div>
</details>
</div>

<div class="faq-no-results" id="faqNoResults" role="status" aria-live="polite">
<div class="faq-no-results-icon" aria-hidden="true"></div>
<h2>No matching questions found</h2>
<p>Try searching for broader terms such as &ldquo;Handlebars&rdquo;, &ldquo;YAML&rdquo;, &ldquo;MSRV&rdquo;, or &ldquo;CLI&rdquo;.</p>
<button type="button" class="faq-reset-btn" id="faqResetBtn">Reset search &amp; filters</button>
</div>

<div class="faq-help-card">
<div class="faq-help-icon" aria-hidden="true"></div>
<div class="faq-help-content">
<h2>Have a question not listed here?</h2>
<p>Explore the documentation guides, open an issue on GitHub, or contact the maintainer directly.</p>
<div class="faq-help-actions">
<a href="/getting-started/index.html" class="primary">Getting Started Guide</a>
<a href="https://github.com/sebastienrousseau/nucleusflow/issues" class="secondary" target="_blank" rel="noopener">GitHub Issues</a>
<a href="/contact/index.html" class="secondary">Contact Support</a>
</div>
</div>
</div>
</div>
