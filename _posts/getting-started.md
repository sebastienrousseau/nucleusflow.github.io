---
author: "Sebastien Rousseau"
title: "Getting Started with NucleusFlow: Rust CLI & Library Quickstart"
description: "Install NucleusFlow via Cargo, Homebrew, Nix, or Docker. Scaffold a new static site or embed the three-stage Rust content processing pipeline in under five minutes."
keywords: "install nucleusflow, cargo install nucleusflow, rust static site generator tutorial, nucleusflow cli, nucleusflow rust example, nucleusflow.toml"
date: "2026-10-31T00:00:00+00:00"
updated: "2026-10-31T00:00:00+00:00"
apple_status_bar_style: "black-translucent"
apple-touch-icon: "https://cloudcdn.pro/nucleusflow/v1/icons/192x192.png"
banner: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
canonical: "https://nucleusflow.com/getting-started/index.html"
Category: "Documentation"
item_url: "https://nucleusflow.com/getting-started/index.html"
lang: "en-GB"
eyebrow: "Quickstart Guide"
headline: "Getting Started with NucleusFlow"
lead: "Install the standalone CLI or embed the Rust crate in under five minutes."
layout: "page"
locale: "en_GB"
logo: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
manifest: "/manifest.json"
name: "NucleusFlow"
permalink: "https://nucleusflow.com/getting-started/index.html"
robots: "index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1"
rss: "/rss.xml"
short_name: "nucleusflow"
subtitle: "Install the standalone CLI or embed the Rust crate in minutes."
theme_color: "#11355c"
twitter_card: "summary_large_image"
url: "https://nucleusflow.com/getting-started/index.html"
viewport: "width=device-width, initial-scale=1, shrink-to-fit=no"
---

## 1. Prerequisites &amp; Toolchain Floor

NucleusFlow requires **Rust 1.89.0** or later (2021 Edition). The Minimum Supported Rust Version (MSRV) is enforced at compile time via `Cargo.toml` (`rust-version = "1.89.0"`) and `build.rs`.

---

## 2. Installation

### As a Rust Library Dependency

Add `nucleusflow` to your project's `Cargo.toml`:

```toml
[dependencies]
nucleusflow = "0.0.11"
```

Or run `cargo add`:

```bash
cargo add nucleusflow
```

### As a Standalone Command-Line Binary

Install the `nucleusflow` CLI directly from [Crates.io](https://crates.io/crates/nucleusflow):

```bash
cargo install nucleusflow --locked
```

### System Package Managers &amp; Containers

NucleusFlow ships native packaging definitions across major operating systems in the upstream repository (`pkg/`):

| Channel | Command / Manifest | Notes |
| :--- | :--- | :--- |
| **Cargo** | `cargo install nucleusflow --locked` | Builds from source with Rust 1.89.0+ |
| **Homebrew** | `brew install pkg/brew/nucleusflow.rb` | macOS &amp; Linux formula |
| **Nix Flakes** | `nix run github:sebastienrousseau/nucleusflow` | Reproducible flake (`pkg/nix/flake.nix`) |
| **Arch Linux (AUR)** | `makepkg -si` in `pkg/aur/` | Builds from signed release tarball |
| **Debian / Ubuntu** | `dpkg-buildpackage -us -uc -b` | Includes binary, manpage `nucleusflow.1`, completions |
| **Fedora / RHEL** | `rpmbuild -ba pkg/rpm/nucleusflow.spec` | RPM spec with manpage and shell completions |
| **GNU Make** | `make install PREFIX=/usr/local` | Installs binary, manpage, and bash/zsh/fish/pwsh completions |

---

## 3. CLI Quick Start

### Step 1: Scaffold a New Site

Create a new project directory using one of the built-in starter templates (`blog`, `docs`, or `portfolio`):

```bash
nucleusflow new my-docs --template docs
cd my-docs
```

This creates a standard directory layout with `content/`, `templates/`, `public/`, and `nucleusflow.toml`.

### Step 2: Build the Static Site

Compile all Markdown files in `content/` through your Handlebars layouts in `templates/` and write minified HTML5 to `public/`:

```bash
nucleusflow build \
  --content-dir content \
  --output-dir public \
  --template-dir templates \
  --minify \
  --config nucleusflow.toml
```

### Step 3: Preview Locally with Live Watch

Start the local development server on port `3000` with automatic file watching:

```bash
nucleusflow serve --port 3000 --watch --dir public
```

---

## 4. Rust Library Quick Start

### End-to-End Pipeline (`NucleusFlow`)

Combine `ContentProcessor`, `HandlebarsRenderer`, and `HtmlGenerator` behind the top-level `NucleusFlow` orchestrator:

```rust
use nucleusflow::{
    ContentProcessor, HandlebarsRenderer, HtmlGenerator,
    NucleusFlow, NucleusFlowConfig,
};

fn main() -> Result<(), Box<dyn std::error::Error>> {
    // 1. Configure content, output, and template directories
    let config = NucleusFlowConfig::new("content", "public", "templates");

    // 2. Configure Stage 1 (Markdown + YAML frontmatter + TOC + sanitization)
    let content_processor = ContentProcessor::new()
        .with_toc()
        .with_sanitization();

    // 3. Configure Stage 2 (Handlebars templates + strict variable checking)
    let template_renderer = HandlebarsRenderer::new()
        .with_strict_mode();

    // 4. Configure Stage 3 (HTML5 tag balance check + minification)
    let output_generator = HtmlGenerator::new()
        .with_minification()
        .with_asset_dir("static");

    // 5. Execute the pipeline
    let mut flow = NucleusFlow::new(
        config,
        content_processor,
        template_renderer,
        output_generator,
    );
    flow.process()?;
    Ok(())
}
```

### Standalone Markdown &amp; Frontmatter Processing

You can also invoke individual pipeline stages directly without running a full directory build:

```rust
use nucleusflow::core::traits::Processor;
use nucleusflow::processors::markdown::MarkdownProcessor;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let processor = MarkdownProcessor::new()
        .with_toc()
        .with_sanitization();

    let markdown = "---\ntitle: Hello NucleusFlow\n---\n\n# Welcome\n\nFast **Rust** content pipeline.";
    let html = processor.process(markdown)?;
    assert!(html.contains("<h1>Welcome</h1>"));
    Ok(())
}
```

---

## 5. Next Steps

* **[Supported Formats](/formats/index.html)**: Explore CommonMark + GFM extensions, YAML 1.2 frontmatter, Handlebars helpers, and TOML configuration schemas.
* **[API &amp; CLI Reference](/api/index.html)**: Inspect every public struct, trait, error variant, and CLI flag.
* **[Pipeline Architecture](/architecture/index.html)**: Understand how the three-stage trait pipeline and `parking_lot::RwLock` template cache work under the hood.
