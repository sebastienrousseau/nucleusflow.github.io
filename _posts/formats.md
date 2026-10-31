---
author: "Sebastien Rousseau"
title: "Supported Formats: CommonMark, GFM, YAML 1.2, Handlebars & HTML5"
description: "Complete specification of formats supported by NucleusFlow: CommonMark, GitHub Flavored Markdown (GFM), YAML 1.2 frontmatter, Handlebars (.hbs) templates, TOML configuration, and minified HTML5."
keywords: "nucleusflow formats, rust commonmark gfm, yaml 1.2 frontmatter, handlebars templates rust, html5 minification, nucleusflow.toml schema"
date: "2026-10-31T00:00:00+00:00"
updated: "2026-10-31T00:00:00+00:00"
apple_status_bar_style: "black-translucent"
apple-touch-icon: "https://cloudcdn.pro/nucleusflow/v1/icons/192x192.png"
banner: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
canonical: "https://nucleusflow.com/formats/index.html"
Category: "Documentation"
item_url: "https://nucleusflow.com/formats/index.html"
lang: "en-GB"
eyebrow: "Format Specifications"
headline: "Supported Content, Template & Config Formats"
lead: "Input, template, configuration, and output formats across the three-stage pipeline."
layout: "page"
locale: "en_GB"
logo: "https://cloudcdn.pro/nucleusflow/v1/logos/nucleusflow.svg"
manifest: "/manifest.json"
name: "NucleusFlow"
permalink: "https://nucleusflow.com/formats/index.html"
robots: "index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1"
rss: "/rss.xml"
short_name: "nucleusflow"
subtitle: "Input, template, configuration, and output formats across the three-stage pipeline."
theme_color: "#11355c"
twitter_card: "summary_large_image"
url: "https://nucleusflow.com/formats/index.html"
viewport: "width=device-width, initial-scale=1, shrink-to-fit=no"
---

## Format Support Matrix

NucleusFlow processes structured text and templates through strict, memory-safe Rust parsers with deterministic resource budgets:

| Format Category | Specification / Syntax | Underlying Crate | Default Size Budget | Stage |
| :--- | :--- | :--- | :--- | :--- |
| **Markdown Content** | CommonMark + GitHub Flavored Markdown (GFM) | `pulldown-cmark` 0.13.5 | `10 MB` (`MAX_CONTENT_SIZE`) | Stage 1 (`MarkdownProcessor`) |
| **Document Frontmatter** | YAML 1.2 (`---` fenced header block) | `noyalib` 0.0.58 | `64 KB` (`max_metadata_size`) | Stage 1 (`MarkdownProcessor`) |
| **Layout Templates** | Handlebars (`.hbs`) with partials &amp; helpers | `handlebars` 6.2 | `1 MB` (`max_template_size`) | Stage 2 (`HandlebarsRenderer`) |
| **Pipeline Configuration** | TOML (`nucleusflow.toml`) &amp; Env Overrides | `toml` 0.9.5 + `serde` 1.0 | `1 MB` (`MAX_CONFIG_SIZE`) | Core (`ConfigBuilder`) |
| **Generated Markup** | HTML5 + Schema.org JSON-LD + `<meta>` tags | `minify-html` 0.18.1 | `100 MB` (`max_output_size`) | Stage 3 (`HtmlGenerator`) |

---

## 1. CommonMark &amp; GitHub Flavored Markdown (GFM)

`MarkdownProcessor` uses `pulldown-cmark` 0.13.5 with GFM extensions enabled by default:

* **GFM Tables (`ENABLE_TABLES`)**: Pipe-delimited tables with column alignment (`:---`, `:---:`, `---:`).
* **Footnotes (`ENABLE_FOOTNOTES`)**: Inline footnote references (`[^1]`) and bottom definition blocks.
* **Strikethrough (`ENABLE_STRIKETHROUGH`)**: Double-tilde (`~~deleted~~`) inline formatting.
* **Automated Table of Contents (`with_toc()`)**: Extracts heading hierarchies (`h1` through `h6`), generates URL-safe slugs, and prepends a semantic `<nav class="toc"><ul>...</ul></nav>` block.
* **HTML Allowlist Sanitization (`with_sanitization()`)**: Inspects inline HTML tags against `ALLOWED_HTML_TAGS` (`p`, `div`, `span`, `h1`..`h6`, `ul`, `ol`, `li`, `a`, `img`, `code`, `pre`, `blockquote`, `em`, `strong`, `del`, `table`, `thead`, `tbody`, `tr`, `th`, `td`, `sup`, `sub`, `br`, `hr`) and rejects suspicious patterns (`<script`, `javascript:`, `data:`, `vbscript:`, `onclick`, `onerror`, `onload`, `eval(`).

---

## 2. YAML 1.2 Frontmatter (`noyalib`)

Every Markdown document may begin with a `---` delimited YAML 1.2 frontmatter block parsed by `noyalib` 0.0.58:

```yaml
---
title: "Deterministic Rust Pipelines"
author: "Sebastien Rousseau"
date: "2026-10-31"
description: "Guide to trait-based static site generation."
---
```

* **Validation**: Rejects empty frontmatter blocks and malformed YAML mappings with a typed `ProcessingError::InvalidFrontmatter`.
* **Automatic Schema.org JSON-LD Synthesis**: Valid frontmatter mappings are serialized into a `<script type="application/ld+json">` block with `@context: "https://schema.org"` and `@type: "Article"` when rendered through `MarkdownProcessor`.

---

## 3. Handlebars Templates (`.hbs`)

`HandlebarsRenderer` (`HtmlTemplateRenderer`) compiles `.hbs` templates using `handlebars` 6.2 and caches compiled templates inside an `Arc<parking_lot::RwLock<Handlebars>>`:

* **Context Variables**: Standard page fields (`title`, `description`, `content`, `author`, `date`) and custom metadata key-value pairs.
* **Reusable Partials (`with_partial`)**: Register shared layout components such as headers, footers, and navigation menus.
* **Custom Helpers (`with_helper` / `TemplateHelper` trait)**: Implement the `TemplateHelper` trait (`fn execute(&self, args: &[Value]) -> Result<Value, ProcessingError>`) or use the built-in `UppercaseHelper`.
* **Strict Mode (`with_strict_mode`)**: Verifies that every `&#123;&#123;variable&#125;&#125;` referenced in the template exists in the rendering context before execution, returning `ProcessingError::TemplateDataError` if any required key is missing.
* **Balanced Brace Verification**: Pre-validates template syntax for matching `&#123;&#123;` and `&#125;&#125;` delimiters before invoking the compiler.

---

## 4. HTML5 Output &amp; Minification

`HtmlGenerator` (`HtmlOutputGenerator`) transforms rendered template output into production-ready HTML5 files:

* **HTML5 Structural Validation**: Validates tag nesting and closing order while properly recognizing HTML5 void elements (`area`, `base`, `br`, `col`, `embed`, `hr`, `img`, `input`, `link`, `meta`, `param`, `source`, `track`, `wbr`) and optional-closing elements (`p`, `dt`, `dd`, `li`, `option`, `thead`, `th`, `tbody`, `tr`, `td`, `tfoot`, `colgroup`).
* **Escaped `<meta>` Injection (`with_meta_tags`)**: Escapes `&`, `<`, `>`, `"`, and `'` in metadata keys and values and injects `<meta name="..." content="...">` into `<head>`.
* **Sub-10 µs Minification (`with_minification`)**: Uses `minify-html` 0.18.1 to minify HTML, inline CSS (`minify_css: true`), and inline JavaScript (`minify_js: true`) while preserving closing tags for strict validator compatibility.
* **Pretty-Printing (`without_minification`)**: Indents nested HTML elements cleanly for local debugging and snapshot inspection.

---

## 5. Multi-Environment Configuration (`nucleusflow.toml`)

`ConfigBuilder` loads TOML configuration files, validates numerical bounds, and optionally applies environment variable overrides:

```toml
profile = "production"

[content]
max_content_size = 10485760
max_metadata_size = 65536
allowed_extensions = ["md", "markdown"]
validate_markdown = true
allow_html = false

[templates]
max_template_size = 1048576
cache_ttl = 3600
max_cache_size = 104857600
allowed_extensions = ["hbs", "handlebars"]

[output]
max_output_size = 104857600
file_permissions = 0o644
create_Backup = false
max_concurrent_ops = 10

[custom]
site_url = "https://nucleusflow.com"
```
