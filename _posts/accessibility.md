---
author: "Sebastien Rousseau"
title: "Accessibility Statement (WCAG 2.2 Level AAA) | NucleusFlow"
description: "WCAG 2.2 Level AAA accessibility compliance statement for nucleusflow.com: verified 7.0:1+ colour contrast ratios, keyboard navigation, semantic landmarks, and reduced motion."
keywords: "nucleusflow accessibility, wcag 2.2 aaa compliance, accessible documentation, contrast ratio verification"
date: "2026-10-31T00:00:00+00:00"
updated: "2026-10-31T00:00:00+00:00"
apple_status_bar_style: "black-translucent"
apple-touch-icon: "https://cloudcdn.pro/nucleusflow/v1/icons/192x192.png"
banner: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
canonical: "https://nucleusflow.com/accessibility/index.html"
Category: "Legal"
item_url: "https://nucleusflow.com/accessibility/index.html"
lang: "en-GB"
eyebrow: "WCAG 2.2 Level AAA"
headline: "Accessibility Statement"
lead: "Engineered and continuously audited to meet WCAG 2.2 Level AAA contrast and structure."
layout: "page"
locale: "en_GB"
logo: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
manifest: "/manifest.json"
name: "NucleusFlow"
permalink: "https://nucleusflow.com/accessibility/index.html"
robots: "index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1"
rss: "/rss.xml"
short_name: "nucleusflow"
subtitle: "Engineered and continuously audited to meet WCAG 2.2 Level AAA."
theme_color: "#11355c"
twitter_card: "summary_large_image"
url: "https://nucleusflow.com/accessibility/index.html"
viewport: "width=device-width, initial-scale=1, shrink-to-fit=no"
---

## 1. Conformance Status

**NucleusFlow (`nucleusflow.com`)** targets and enforces **Web Content Accessibility Guidelines (WCAG) 2.2 Level AAA** across all 20 documentation routes. Every build runs automated contrast verification (`python3 audit/contrast.py`), HTML5/WCAG structural validation (`npx html-validate "docs/**/*.html"`), and `ssg audit`.

---

## 2. Verified Colour Contrast (`>= 7.0:1` AAA)

All foreground and background colour pairs in both Light and Dark themes exceed the WCAG 1.4.6 Contrast (Enhanced) Level AAA requirement of `7.0:1`:

| Theme &amp; Pair | Foreground | Background | Contrast Ratio | WCAG 2.2 AAA (`>= 7.0:1`) |
| :--- | :--- | :--- | :--- | :--- |
| **Light Body Text** | `#14181f` | `#ffffff` | `17.79:1` | PASS |
| **Light Muted / Background** | `#41474f` | `#ffffff` | `9.42:1` | PASS |
| **Light Muted / Surface** | `#41474f` | `#eef2f6` | `8.35:1` | PASS |
| **Light Brand / Background** | `#1c4e9e` | `#ffffff` | `7.98:1` | PASS |
| **Light Brand / Surface** | `#1c4e9e` | `#eef2f6` | `7.09:1` | PASS |
| **Light Primary Button** | `#ffffff` | `#173f86` | `10.02:1` | PASS |
| **Dark Body Text** | `#e8eef4` | `#0d1117` | `16.21:1` | PASS |
| **Dark Muted / Background** | `#aeb9c5` | `#0d1117` | `9.47:1` | PASS |
| **Dark Muted / Surface** | `#aeb9c5` | `#161c26` | `8.55:1` | PASS |
| **Dark Brand / Background** | `#82b4ff` | `#0d1117` | `8.94:1` | PASS |
| **Dark Brand / Surface** | `#82b4ff` | `#161c26` | `8.08:1` | PASS |

---

## 3. Assistive Technology &amp; Interaction Features

* **Keyboard Navigation &amp; Skip Link**: Every page opens with a `<a class="skip-link" href="#main">Skip to main content</a>` link, high-contrast `:focus-visible` outlines, and `Escape` key dismissal for mobile navigation and search modals.
* **Semantic Landmarks &amp; Heading Hierarchy**: Every route contains exactly one `<header>`, one `<main id="main">`, one `<h1>`, and one `<footer>`, with no skipped heading levels (`h1` -> `h2` -> `h3`).
* **Reduced Motion**: Respects `prefers-reduced-motion: reduce` by disabling animations and smooth scrolling.
* **Generated Content Accessibility**: The core `nucleusflow` library itself emits semantic `<nav class="toc">` landmarks when `with_toc()` is enabled.
