---
author: "Sebastien Rousseau"
title: "API & CLI Reference: NucleusFlow Rust Crate & Command-Line Interface"
description: "Complete API reference for the nucleusflow Rust crate (v0.0.11) and CLI binary. Explore NucleusFlow, ContentProcessor, HandlebarsRenderer, HtmlGenerator, ConfigBuilder, core traits, and error types."
keywords: "nucleusflow api reference, nucleusflow docs.rs, ContentProcessor, HandlebarsRenderer, HtmlGenerator, ConfigBuilder, ProcessingError, nucleusflow cli flags"
date: "2026-10-31T00:00:00+00:00"
updated: "2026-10-31T00:00:00+00:00"
apple_status_bar_style: "black-translucent"
apple-touch-icon: "https://cloudcdn.pro/nucleusflow/v1/icons/192x192.png"
banner: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
canonical: "https://nucleusflow.com/api/index.html"
Category: "Documentation"
item_url: "https://nucleusflow.com/api/index.html"
lang: "en-GB"
eyebrow: "Developer Reference"
headline: "API & CLI Reference"
lead: "Complete Rust library types, traits, configuration builders, and CLI subcommands."
layout: "page"
locale: "en_GB"
logo: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
manifest: "/manifest.json"
name: "NucleusFlow"
permalink: "https://nucleusflow.com/api/index.html"
robots: "index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1"
rss: "/rss.xml"
short_name: "nucleusflow"
subtitle: "Complete Rust library types, traits, configuration builders, and CLI subcommands."
theme_color: "#11355c"
twitter_card: "summary_large_image"
url: "https://nucleusflow.com/api/index.html"
viewport: "width=device-width, initial-scale=1, shrink-to-fit=no"
---

## 1. Crate Overview &amp; Re-Exports (`nucleusflow`)

The root `nucleusflow` module re-exports the primary pipeline types so applications can import them directly from `nucleusflow::*`:

| Symbol | Module Path | Description |
| :--- | :--- | :--- |
| `NucleusFlow` | `nucleusflow::NucleusFlow` | Top-level pipeline orchestrator connecting content processing, template rendering, and HTML generation |
| `NucleusFlowConfig` | `nucleusflow::NucleusFlowConfig` | Directory configuration (`content_dir`, `output_dir`, `template_dir`) for `NucleusFlow` |
| `ContentProcessor` | `nucleusflow::processors::markdown::MarkdownProcessor` | Stage 1 CommonMark + GFM + YAML 1.2 frontmatter processor |
| `HandlebarsRenderer` | `nucleusflow::template::HtmlTemplateRenderer` | Stage 2 thread-safe cached Handlebars template renderer |
| `HtmlGenerator` | `nucleusflow::generators::html::HtmlOutputGenerator` | Stage 3 HTML5 validator, `<meta>` injector, and minifier |
| `FileContentProcessor` | `nucleusflow::process::FileContentProcessor` | File-oriented content processor with path sanitization and extension checks |
| `TemplateRenderer` | `nucleusflow::template::TemplateRenderer` | Trait for rendering named templates with JSON context values |
| `ConfigBuilder` | `nucleusflow::core::config::ConfigBuilder` | Builder for multi-profile TOML and environment configuration |
| `ProcessingError` | `nucleusflow::core::error::ProcessingError` | Unified error enum for all pipeline stages |

Full generated Rustdoc is available on [docs.rs/nucleusflow](https://docs.rs/nucleusflow).

---

## 2. Stage 1: Content Processing (`nucleusflow::processors::markdown`)

### `MarkdownProcessor` (re-exported as `ContentProcessor`)

```rust
use nucleusflow::ContentProcessor;
use nucleusflow::core::traits::Processor;

let processor = ContentProcessor::new()
    .with_toc()
    .with_sanitization()
    .with_custom_Metadata("site_name".to_string(), "NucleusFlow".to_string());

let html = processor.process("# Heading\n\nParagraph text.")?;
```

| Method | Signature | Behavior |
| :--- | :--- | :--- |
| `new()` | `pub fn new() -> Self` | Creates a processor with GFM tables, footnotes, and strikethrough enabled |
| `with_toc()` | `pub fn with_toc(self) -> Self` | Enables automatic `<nav class="toc">` generation from document headings |
| `with_sanitization()` | `pub fn with_sanitization(self) -> Self` | Enables HTML tag allowlist (`ALLOWED_HTML_TAGS`) and XSS pattern blocking |
| `with_options(opts)` | `pub fn with_options(self, options: Options) -> Self` | Customizes `pulldown_cmark::Options` bitflags |
| `with_custom_Metadata(k, v)` | `pub fn with_custom_Metadata(self, key: String, value: String) -> Self` | Attaches custom metadata key-value pairs |
| `process(&self, input)` | `fn process(&self, input: &str) -> Result<String, ProcessingError>` | Validates size (`<= 10 MB`), extracts YAML frontmatter, renders Markdown, and sanitizes output |

---

## 3. Stage 2: Template Rendering (`nucleusflow::template`)

### `HtmlTemplateRenderer` (re-exported as `HandlebarsRenderer`)

```rust
use nucleusflow:: {HandlebarsRenderer, TemplateRenderer};
use nucleusflow::template::UppercaseHelper;
use serde_json::json;

let renderer = HandlebarsRenderer::new()
    .with_template_dir("templates")
    .with_strict_mode()
    .with_helper("upper".to_string(), Box::new(UppercaseHelper));

renderer.load_templates()?;
let rendered = renderer.render("page", &json!({ "title": "API Guide" }))?;
```

| Method | Signature | Behavior |
| :--- | :--- | :--- |
| `new()` | `pub fn new() -> Self` | Initializes an `Arc<RwLock<Handlebars>>` registry with `UppercaseHelper` registered |
| `with_template_dir(dir)` | `pub fn with_template_dir<P: Into<PathBuf>>(self, dir: P) -> Self` | Sets the directory used by `load_templates()` to scan `.hbs` files |
| `with_partial(name, content)` | `pub fn with_partial(self, name: String, content: String) -> Result<Self, ProcessingError>` | Validates and registers a named Handlebars partial |
| `with_helper(name, helper)` | `pub fn with_helper(self, name: String, helper: Box<dyn TemplateHelper>) -> Self` | Registers a custom `TemplateHelper` implementation |
| `with_strict_mode()` | `pub fn with_strict_mode(self) -> Self` | Requires all referenced template variables to exist in the context JSON |
| `load_templates(&self)` | `pub fn load_templates(&self) -> Result<(), ProcessingError>` | Recursively loads all `.hbs` files from `template_dir` into the cache |
| `register_template(&mut self, name, tpl)` | `fn register_template(&mut self, name: &str, template: &str) -> Result<(), ProcessingError>` | Validates brace balance and compiles a template into the registry |
| `render(&self, name, data)` | `fn render(&self, template_name: &str, data: &Value) -> Result<String, ProcessingError>` | Renders a cached template with a `serde_json::Value` context |

---

## 4. Stage 3: Output Generation (`nucleusflow::generators::html`)

### `HtmlOutputGenerator` (re-exported as `HtmlGenerator`)

```rust
use std::collections::HashMap;
use nucleusflow::HtmlGenerator;
use nucleusflow::core::traits::Generator;

let mut meta = HashMap::new();
meta.insert("author".to_string(), "Sebastien Rousseau".to_string());

let generator = HtmlGenerator::new()
    .with_minification()
    .with_meta_tags(meta)
    .with_asset_dir("static");

let output = generator.generate("<!DOCTYPE html><html><head></head><body><h1>Hi</h1></body></html>")?;
```

| Method | Signature | Behavior |
| :--- | :--- | :--- |
| `new()` | `pub fn new() -> Self` | Creates an HTML5 generator with minification enabled by default |
| `with_minification()` | `pub fn with_minification(self) -> Self` | Enables `minify-html` (HTML, CSS, and JS minification) |
| `without_minification()` | `pub fn without_minification(self) -> Self` | Disables minification and enables indented pretty-printing |
| `with_asset_dir(dir)` | `pub fn with_asset_dir<P: Into<PathBuf>>(self, dir: P) -> Self` | Sets the static asset directory and loads assets into the in-memory cache |
| `with_meta_tags(tags)` | `pub fn with_meta_tags(self, tags: HashMap<String, String>) -> Self` | Injects HTML-escaped `<meta>` tags into `<head>` |
| `copy_assets_to(dir)` | `pub fn copy_assets_to<P: AsRef<Path>>(&self, output_dir: P) -> Result<(), ProcessingError>` | Writes cached static assets to `output_dir`, rejecting `..` path traversal |
| `generate(&self, content)` | `fn generate(&self, content: &str) -> Result<String, ProcessingError>` | Validates HTML5 tag balance, injects `<meta>` tags, and minifies or formats |

---

## 5. Configuration &amp; Core Traits (`nucleusflow::core`)

### `ConfigBuilder` &amp; `Profile`

```rust
use std::time::Duration;
use nucleusflow::core::config::{ConfigBuilder, Profile};

let config = ConfigBuilder::new(Profile::Production)
    .with_file("nucleusflow.toml")?
    .with_env_prefix("NUCLEUSFLOW")?
    .with_auto_reload(true)
    .with_reload_interval(Duration::from_secs(30))
    .with_max_file_size(512 * 1024)
    .build()?;
```

### Core Traits (`nucleusflow::core::traits`)

* **`Processor`**: `fn process(&self, input: &str) -> Result<String, ProcessingError>` and `fn validate(&self, input: &str) -> Result<(), ProcessingError>`.
* **`Transform`**: `fn transform(&self, input: &str) -> Result<String, ProcessingError>` and `fn can_transform(&self, input_type: &str) -> bool`.
* **`Generator`**: `fn generate(&self, content: &str) -> Result<String, ProcessingError>` and `fn supported_formats(&self) -> Vec<&str>`.
* **`Validator`**: `fn validate(&self, content: &str, rules: &[ValidationRule]) -> Result<(), ProcessingError>`.
* **`ProcessingContext`**: `get_metadata(&self, key: &str)`, `set_metadata(&mut self, key: String, value: String)`, `get_options(&self) -> &ProcessingOptions`.
* **`Shareable`**: Blanket trait providing `.into_shared()` (`Arc<RwLock<Self>>`) for any `Send + Sync + 'static` component.

### `ProcessingError` Variants (`nucleusflow::core::error`)

| Variant | Trigger Condition |
| :--- | :--- |
| `MarkdownError(String)` | Empty input, content exceeding `10 MB`, or Markdown parse failure |
| `InvalidFrontmatter(String)` | Empty frontmatter or invalid YAML 1.2 syntax |
| `HtmlSanitizationError(String)` | Disallowed HTML tag or suspicious XSS attribute/URI pattern |
| `TemplateSyntaxError(String)` | Unbalanced `&#123;&#123;` / `&#125;&#125;` braces or invalid Handlebars syntax |
| `TemplateRenderError(String)` | Missing template name or Handlebars evaluation error |
| `TemplateDataError(String)` | Missing required variable in strict mode or non-object context |
| `InvalidHtmlError(String)` | Mismatched closing HTML tag or unclosed non-void element |
| `ConfigValidationError(String)` | Configuration file exceeding `1 MB` or out-of-range budget parameter |
| `EnvVarError(String)` | Disallowed environment variable name (`PATH`, `HOME`, `USER`, `SHELL`) |
| `AssetCopyError(String)` | Path traversal attempt (`..`) or missing asset source directory |

---

## 6. CLI Reference (`nucleusflow`)

```text
nucleusflow [SUBCOMMAND] [OPTIONS]
```

| Subcommand | Flags &amp; Arguments | Defaults | Description |
| :--- | :--- | :--- | :--- |
| `new <name>` | `-t, --template <template>` | `blog` (`blog`, `docs`, `portfolio`) | Scaffolds a new NucleusFlow project directory |
| `build` | `-c, --content-dir <DIR>`<br>`-o, --output-dir <DIR>`<br>`-t, --template-dir <DIR>`<br>`-m, --minify`<br>`--config <FILE>` | `content`<br>`public`<br>`templates`<br>`false`<br>`nucleusflow.toml` | Compiles Markdown content and Handlebars templates into static HTML |
| `serve` | `-p, --port <PORT>`<br>`-w, --watch`<br>`-d, --dir <DIR>` | `3000`<br>`false`<br>`public` | Serves the generated static directory locally with optional live watching |
