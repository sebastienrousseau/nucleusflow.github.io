---
author: "Sebastien Rousseau"
title: "Privacy Policy: Zero Telemetry & Zero Cookies | NucleusFlow"
description: "Privacy policy for nucleusflow.com and the NucleusFlow Rust crate: zero cookies, zero analytics beacons, zero telemetry, and strict Content Security Policy enforcement."
keywords: "nucleusflow privacy policy, zero telemetry ssg, no cookies documentation, gdpr compliant static site"
date: "2026-10-31T00:00:00+00:00"
updated: "2026-10-31T00:00:00+00:00"
apple_status_bar_style: "black-translucent"
apple-touch-icon: "https://cloudcdn.pro/nucleusflow/v1/icons/192x192.png"
banner: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
canonical: "https://nucleusflow.com/privacy/index.html"
Category: "Legal"
item_url: "https://nucleusflow.com/privacy/index.html"
lang: "en-GB"
eyebrow: "Zero Telemetry"
headline: "Privacy Policy"
lead: "Zero tracking, zero analytics cookies, and 100% offline-capable software."
layout: "page"
locale: "en_GB"
logo: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
manifest: "/manifest.json"
name: "NucleusFlow"
permalink: "https://nucleusflow.com/privacy/index.html"
robots: "index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1"
rss: "/rss.xml"
short_name: "nucleusflow"
subtitle: "Zero tracking, zero analytics cookies, and 100% offline-capable software."
theme_color: "#11355c"
twitter_card: "summary_large_image"
url: "https://nucleusflow.com/privacy/index.html"
viewport: "width=device-width, initial-scale=1, shrink-to-fit=no"
---

## 1. Zero Telemetry in `nucleusflow` Library &amp; CLI

The `nucleusflow` Rust library crate and command-line binary operate 100% locally on your files. They contain **zero telemetry**, **zero crash-reporting beacons**, and **zero outbound network requests** during project scaffolding (`new`) or static site compilation (`build`).

---

## 2. Zero Tracking on `nucleusflow.com`

This documentation website (`https://nucleusflow.com`) is a purely static site compiled with `ssg` and hosted on GitHub Pages:

* **No Cookies**: We do not set tracking, advertising, or session cookies.
* **No Third-Party Analytics**: We do not embed Google Analytics, Meta pixels, or third-party behavioral scripts.
* **Local Theme Preference Only**: Your selected colour theme (`light`, `dark`, or `system`) is stored exclusively in your browser's local `localStorage` under the key `visage-theme-mode` and is never transmitted over the network.
* **Client-Side Search**: Site search queries run entirely in-browser against the static `/search-index.json` file.
* **Contact Form**: If you voluntarily submit a message via the [Contact](/contact/index.html) page, the fields you enter (`name`, `email`, `message`) are processed via Formspree solely to deliver your inquiry to the maintainer.
