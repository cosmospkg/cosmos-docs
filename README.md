# 📚 Cosmos Documentation

> This is the full documentation repo for the Cosmos package manager.

Built using [MkDocs](https://www.mkdocs.org/) with the [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) theme. It contains all user-facing guides, architectural documentation, contributor guides, and internal reference materials for Cosmos and its tooling.

---

## 🧭 Structure

```
docs/
├── 00-Overview.md
├── 01-Architecture.md
├── 02-Core-Concepts.md
├── 03-File-Formats.md
├── 04-Nebula-vs-Constellation.md
├── 05-Glossary.md
├── 06-Caching-and-Syncing.md
├── 07-Flows.md
├── 08-CLI.md
├── 09-Tooling.md
├── 10-Nova.md
├── 11-FFI.md
├── 12-Phase-3.md
├── 13-Design-Rationale.md
├── 14-Security.md
├── 15-Examples.md
├── 16-Galaxies.md
├── 17-Versioning.md
├── 18-Cosmos-Contribution.md
├── 19-Contribution-Guide-for-Maintainers.md
├── 20-Crate-Policy.md
├── 21-Bootstrapping.md
├── faq.md
└── index.md
```

Each document focuses on a different part of Cosmos — from package flows and file formats to embedded scripting and contributor etiquette.

---

## 🚀 Live Site

Docs are published at:  
🔗 **[https://docs.cosmos-pkg.org](https://docs.cosmos-pkg.org)**

Changes to `main` will be automatically deployed to the static site when hosted.

---

## 🛠️ Local Dev

1. Install MkDocs and dependencies:

```bash
pip install mkdocs mkdocs-material
```

2. Start the local dev server:

```bash
mkdocs serve
```

3. Build static output:

```bash
mkdocs build
```

Site will be generated in the `site/` directory.

---

## 📄 Style Guide

- Use Markdown headings for structure
- Keep documents under ~200 lines if possible
- Use fenced code blocks with language annotations (`bash`, `toml`, `lua`, etc.)
- Keep dry humor subtle and docs technically accurate

---

## 🙏 Contributions

PRs welcome — especially for:
- Typos or clarity improvements
- Expanding examples and usage guides
- Fixing inconsistencies across command references

Please follow the tone and format used across existing files.

---

> These docs are readable, linkable, and load without JavaScript. Just like Cosmos intended.

