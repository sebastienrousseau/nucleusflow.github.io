---
author: "Sebastien Rousseau"
title: "Made with SSG & Skeletonic CSS | NucleusFlow Documentation"
description: "How nucleusflow.com is compiled using the Rust Static Site Generator (ssg 0.0.69) and styled with the Skeletonic CSS Visage design system."
keywords: "made with ssg, rust static site generator, skeletonic css, visage theme, wcag 2.2 aaa"
date: "2026-10-31T00:00:00+00:00"
updated: "2026-10-31T00:00:00+00:00"
apple_status_bar_style: "black-translucent"
apple-touch-icon: "https://cloudcdn.pro/nucleusflow/v1/icons/192x192.png"
banner: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
canonical: "https://nucleusflow.com/made-with-ssg/index.html"
Category: "Legal"
item_url: "https://nucleusflow.com/made-with-ssg/index.html"
lang: "en-GB"
eyebrow: "Documentation Stack"
headline: "Made with SSG & Skeletonic CSS"
lead: "How this documentation portal is compiled, fingerprinted, and audited."
layout: "page"
locale: "en_GB"
logo: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
manifest: "/manifest.json"
name: "NucleusFlow"
permalink: "https://nucleusflow.com/made-with-ssg/index.html"
robots: "index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1"
rss: "/rss.xml"
short_name: "nucleusflow"
subtitle: "How this documentation portal is compiled, fingerprinted, and audited."
theme_color: "#11355c"
twitter_card: "summary_large_image"
url: "https://nucleusflow.com/made-with-ssg/index.html"
viewport: "width=device-width, initial-scale=1, shrink-to-fit=no"
---

## Built with Rust `ssg` (`v0.0.69`) &amp; Skeletonic CSS

The **`nucleusflow.com`** documentation portal is compiled using the [Static Site Generator (`ssg`)](https://static-site-generator.com/) (`v0.0.69`) and styled with [Skeletonic CSS](https://skeletonic.com/) (`Visage` design system).

### Build &amp; Verification Pipeline

1. **Content &amp; Templates**: Markdown articles in `_posts/*.md` are rendered into `_layouts/*.html` templates.
2. **Asset Fingerprinting &amp; SRI**: `ssg` hashes stylesheets and scripts (`styles.<hash>.css`, `main.<hash>.js`) and injects `sha384-` Subresource Integrity (SRI) attributes.
3. **Syndication &amp; Discovery**: Automatically emits `sitemap.xml`, `rss.xml`, `atom.xml`, `feed.json`, `search-index.json`, `llms.txt`, `llms-full.txt`, and CycloneDX SBOM (`sbom.cdx.json`).
4. **10-Pillar Quality Gate**: Every commit is verified against WCAG 2.2 Level AAA contrast ratios, HTML5 structural validation, Hypothesis property tests, and SPDX license compliance.
