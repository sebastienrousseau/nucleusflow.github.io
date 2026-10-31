# Migration Guide

This guide covers migrating existing static sites and content pipelines to **NucleusFlow** from Hugo, Zola, and Node.js generators, as well as maintaining the `nucleusflow.github.io` documentation portal.

---

## 1. Migrating from Hugo to NucleusFlow

Hugo uses Go `html/template` syntax and `hugo.toml`, whereas NucleusFlow uses compiled Handlebars (`.hbs`) templates and `nucleusflow.toml`. Both engines support CommonMark Markdown with `---` YAML frontmatter.

| Concept | Hugo | NucleusFlow |
| :--- | :--- | :--- |
| **Configuration File** | `hugo.toml` / `config.yaml` | `nucleusflow.toml` |
| **Content Directory** | `content/` | `content/` (`--content-dir`) |
| **Template Directory** | `layouts/` | `templates/` (`--template-dir`) |
| **Output Directory** | `public/` | `public/` (`--output-dir`) |
| **Template Engine** | Go `html/template` | Handlebars (`.hbs`) |
| **Embeddable Rust API** | No (CLI only) | Yes (`nucleusflow` library crate) |

### Template Syntax Translation

| Construct | Hugo (Go Templates) | NucleusFlow (Handlebars `.hbs`) |
| :--- | :--- | :--- |
| **Page Title** | `{{ .Title }}` | `{{title}}` |
| **Rendered Body** | `{{ .Content }}` | `{{{content}}}` |
| **Partial Inclusion** | `{{ partial "header.html" . }}` | `{{> header}}` |
| **Conditional Block** | `{{ if .Params.toc }}...{{ end }}` | `{{#if toc}}...{{/if}}` |
| **Loop Iteration** | `{{ range .Pages }}...{{ end }}` | `{{#each pages}}...{{/each}}` |

---

## 2. Migrating from Zola to NucleusFlow

Zola is a monolithic Rust CLI binary using Tera templates and TOML (`+++`) frontmatter. NucleusFlow provides both a standalone CLI (`nucleusflow`) and a composable Rust library crate using YAML (`---`) frontmatter and Handlebars (`.hbs`) templates.

| Concept | Zola | NucleusFlow |
| :--- | :--- | :--- |
| **Frontmatter Delimiter** | `+++` (TOML) or `---` (YAML) | `---` (YAML 1.2 via `noyalib`) |
| **Template Engine** | Tera (`.html`) | Handlebars (`.hbs`) |
| **Unescaped HTML Body** | `{{ page.content | safe }}` | `{{{content}}}` |
| **HTMLSanitization** | External / manual | Built-in allowlist (`ALLOWED_HTML_TAGS`) & XSS blocker |
| **HTML5 Minification** | Optional flag | Built-in `minify-html` (~8.1 µs per page) |

### CLI Command Equivalents

```bash
# Scaffold a new project
nucleusflow new my-site --template docs

# Build and minify static HTML output
nucleusflow build \
  --content-dir content \
  --output-dir public \
  --template-dir templates \
  --minify \
  --config nucleusflow.toml

# Serve locally with live file watching
nucleusflow serve --port 3000 --watch --dir public
```

---

## 3. Documentation Portal Standards (`nucleusflow.github.io`)

* **Canonical Theme (`Visage`)**: All layouts in `_layouts/` inherit from `_layouts/base.html`, `_layouts/header.html`, and `_layouts/footer.html` using `{{#extends "base"}}` and `{{!content}}`.
* **Header Search Slot**: Top navigation embeds `<span data-ssg-search></span>` inside `#navMenu`, which `ssg 0.0.69` populates at build time with `#ssg-search-btn`.
* **Verification Gate**: Run `make verify` to execute SPDX header checks, frontmatter validation, WCAG 2.2 AAA contrast verification, property tests, and `ssg audit`.
