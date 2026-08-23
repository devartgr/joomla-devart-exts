# DevArt Exts for Joomla

Modular Joomla 6 content tools for reusable snippets, syntax-highlighted code blocks, and trusted raw embeds.

![Joomla](https://img.shields.io/badge/Joomla-6.x-blue)
![PHP](https://img.shields.io/badge/PHP-8.3%2B-green)
![Release](https://img.shields.io/badge/Version-1.3.0-orange)
![License](https://img.shields.io/badge/License-GPLv3-red)

---

## Overview

DevArt Exts is a production package for Joomla 6 sites that need safe reusable HTML, code blocks, and trusted embeds without external frameworks.

The package ships one administrator component, six plugins, and one site module. Social Share and Open Graph are **not** included — use **DevArt Article Tools** for those features.

---

## Version 1.3.0

DevArt Exts **1.3.0** is the current **public** release.

This is the first public release since **1.1.8**. Intermediate 1.2.x builds were development-only and were not published.

### Highlights in 1.3.0

**Core content tools**
- **Snippets** — reusable safe HTML and iframe content via `{snippet id="X"}`
- **Code Snippets** — syntax-highlighted blocks with copy button via `{codesnippet id=X}`
- **Source Code** — trusted raw embeds (scripts, iframes, widgets) via `{source}...{/source}`
- **Source Module** — `mod_devartsource` for module positions (complementary to article shortcodes)

**Administrator experience**
- Article Tools–style dashboard and section hubs (Snippets, Code Snippets, Source Code, Source Module, Tools)
- Authors-style Back navigation on list views
- Per-submenu header images and DevArt red active submenu styling
- Source Code usage panel with editor insert note (CMS Content → DevArt Source)

**Tools**
- JSON backup export/import for Snippets and Code Snippets
- Read-only Detect Head Scripts scanner with filters, third-party detection, duplicate grouping, and scan presets

**Security and access**
- Snippet and code snippet access levels with cache keys that respect authorised view levels
- Optional allowed user groups for `{source}` (editor button hidden and blocks stripped on save when unauthorised)
- Automatic code-snippet cache invalidation on save, delete, and publish
- Safe retirement of standalone `pkg_devartsource` without destroying module instances

**Languages**
- Component and plugin language packs for en-GB, el-GR, fr-FR, de-DE, es-ES, it-IT, pt-PT, cs-CZ, nl-NL, pl-PL, ru-RU, uk-UA, ja-JP, tr-TR, and zh-CN

---

## Requirements

- Joomla **6.0+**
- PHP **8.3+**
- MariaDB / MySQL

---

## Package Contents

Package ID: `pkg_devart_exts`

| Extension | Type | Purpose |
|-----------|------|---------|
| `com_devart_exts` | Component | Administrator dashboard, snippets, code snippets, source info, tools |
| `plg_content_devartsnippets` | Content plugin | Renders `{snippet id="X"}` shortcodes |
| `plg_content_devartcodesnippets` | Content plugin | Renders `{codesnippet id=X}` shortcodes |
| `plg_content_devartsource` | Content plugin | Executes trusted `{source}...{/source}` embed blocks |
| `plg_editors_xtd_devartsnippetsbutton` | Editor button | Inserts snippet shortcodes |
| `plg_editors_xtd_devartcodesnippetsbutton` | Editor button | Inserts code snippet shortcodes |
| `plg_editors_xtd_devartsourcebutton` | Editor button | Inserts source wrapper blocks |
| `mod_devartsource` | Site module | Trusted embed output in module positions |

---

## Installation

Download the latest release:

[`pkg_devart_exts_v1.3.0.zip`](https://github.com/devartgr/joomla-devart-exts/releases/download/v1.3.0/pkg_devart_exts_v1.3.0.zip)

Install via **System → Install → Extensions**.

The package installer enables all shipped plugins automatically.

### Upgrade from 1.1.8 or earlier

Install the 1.3.0 package ZIP over your existing installation. No manual database fixes are required.

- Existing Social Share / Open Graph plugins from old Exts builds are removed on update if still present
- Standalone `pkg_devartsource` is retired safely; `mod_devartsource` instances are preserved
- Snippet, code snippet, and `{source}` shortcode syntax is unchanged

---

## Updates

Updates are **package-only**. The package manifest owns the Joomla update server:

`https://raw.githubusercontent.com/devartgr/joomla-devart-exts/main/update.xml`

All extensions update together as one package.

---

## Shortcodes

| Syntax | Use for |
|--------|---------|
| `{snippet id="X"}` | Safe HTML / iframe snippets |
| `{codesnippet id=X}` | Syntax-highlighted code blocks |
| `{source}...{/source}` | Trusted scripts, iframes, embeds |

Scripts and PHP are **not** allowed in Snippets or Code Snippets. Use Source Code for trusted scripts.

Insert `{source}` blocks from the editor toolbar: **CMS Content → DevArt Source**.

---

## Repository Layout

- `source/` — development source (single source of truth)
- `scripts/validate.php` — repository validation
- `scripts/build.php` — validation-first packaging with SHA-256
- `builds/` — generated development packages
- `releases/` — verified release packages
- `update.xml` / `changelog.xml` — Joomla update server files (repo root)
- `docs/` — architecture and current-state documentation
- `qa/checklist.md` — Joomla QA checklist

---

## Validate and Build

```shell
php scripts/validate.php
php scripts/build.php
```

Repository validation does not replace Joomla runtime QA.

---

## Documentation

- `docs/architecture.md` — product and repository architecture
- `docs/current-state.md` — current capabilities and limitations
- `qa/checklist.md` — installation and functional QA

---

## Integrity (1.3.0)

| | |
|---|---|
| **File** | `pkg_devart_exts_v1.3.0.zip` |
| **SHA-256** | `68e4a1c1f4feb38c69278388875a6288684254ef3a3dc5454d3fdfa12b383d5e` |

---

## License

GNU General Public License version 3 or later.
