# 00 – Overview

> **Cosmos** is a minimal, statically-linked, vendor-agnostic package management system designed for resilience, portability, and clarity.

> Originally born from [Comet](https://github.com/wombatlinux/comet), Cosmos takes the core ideas of ultra-simple packaging and expands them into a clean, modular architecture that can scale from embedded systems to full operating systems.

---

## 🚀 Goals

- **Fully static binaries** — no dynamic linking, no runtime dependencies beyond libc
- **Bootstrappable from nothing** — only needs libc and a filesystem to operate
- **Human-readable package definitions** — `star.toml`, `constellation.toml`, etc.
- **Multiple install sources** — install packages from:
  - HTTP (enabled by default)
  - HTTPS (optional feature)
  - Cached tarballs
  - Local mirrors
  - Mounted drives or USB sticks
- **No interpreters** — Cosmos does not depend on Bash, Python, Perl, etc.
- **No TLS or crypto stacks required by default** — optional transport features available via `cosmos-transport`
- **Deterministic install flows** — installs and uninstalls are traceable and safe
- **Nebula and Constellation support** — virtual meta-packages and install presets
- **Self-contained system state** — all installed packages tracked in `universe.toml`
- **Cross-arch and cross-host capable** — runs equally well on tiny embedded systems and full desktops

---

## 🧰 Core Concepts

- **Star**: a single package (with metadata, source, dependencies, and install logic)
- **Nebula**: a virtual dependency-only Star with no installable files
- **Constellation**: a list of Stars used as an install preset
- **Galaxy**: a folder containing one or more Star packages and a `meta.toml`
- **Universe**: local system state (installed Stars, versions, and file paths)

---

## 🌎 Real-World Use Cases

- Recovery: fix broken systems where the package manager failed (dependency chain breakage, runtime corruption, missing Python).
- Airgapped Deployments: install and update packages offline without relying on HTTPS, GPG signatures, or heavy runtimes.
- Embedded Systems: run a package manager in minimal or musl-only environments with predictable, portable builds.
- Scratch Systems: bootstrap clean devices from USB or file servers without assuming a full distro or shell.

---

## 🔍 Design Principles

- **No shell required**: Nova scripting allows safe, portable install logic
- **No TLS required by default**: HTTP and file-based installs work out of the box; HTTPS is available as an optional feature
- **No interpreter required**: Rust binary only; works even if `/usr` is broken
- **No central server required**: everything can run from local or offline storage

---

Cosmos is not just a package manager—it is a foundation for building, deploying, and repairing entire operating systems in minimalist or constrained environments.
