# ADR 0001: Adopt Static Site Generator (SSG) with Skeletonic CSS

- Status: accepted
- Date: 2026-10-31

## Context

Nucleusflow.com requires high-speed page loads, privacy-preserving static
hosting, zero runtime tracking, and WCAG 2.2 Level AAA accessibility compliance
for Rust engineers, documentation authors, and security auditors.

## Decision

Build the website using native Rust Static Site Generator (`ssg` 0.0.69)
compilation over Markdown `_posts/` and `_layouts/` templates. Adopt Skeletonic
CSS and Apple HIG Visage navigation patterns with self-hosted assets, strict
Content Security Policy (CSP), and Subresource Integrity (SRI) hashes.

## Consequences

Site compilation produces deterministic HTML, sitemaps, and feeds. Color contrast
and regression tests are automated in CI. Output files are generated from templates
and never edited manually.
