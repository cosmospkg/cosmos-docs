# 08 – Command Line Interface (CLI)

This document outlines the structure and design of the `cosmos` command-line interface. The CLI is designed to be minimal, explicit, and script-friendly, with no hidden behavior or runtime state.

---

## ⚙️ Philosophy

- Every command does one thing well
- No auto-updates or background syncs
- No dynamic linking, no TLS required by default
- Install flows are deterministic and offline-capable

---

## � Command Overview

### `cosmos install <star> [--offline] [--root <path>]`
Installs a Star and its dependencies.

- Uses cached star and tarball if available
- Fetches missing files unless `--offline` is set
- Honors `--root` to install into a different root filesystem
- Skips extraction/scripts for Nebulae

---

### `cosmos install --constellation <path> [--offline] [--root <path>]`
Installs all Stars listed in a `constellation.toml`.

- Respects ordering
- Supports version pinning
- Same install rules as `install <star>`

---

### `cosmos uninstall <star> [--root <path>]`
Uninstalls a Star by removing its tracked files.

- Pulls file list from `universe.toml`
- Skips missing files with a warning
- Removes entry from universe

---

### `cosmos status [--root <path>]`
Prints the list of installed Stars from `universe.toml`.

- Includes version numbers
- Future: add filtering/sorting

---

### `cosmos sync [--stars] [--full]`
Downloads Galaxy metadata, Stars, and optional packages.

- Default: fetches `meta.toml` only
- `--stars`: also fetches all `star.toml` files
- `--full`: also fetches `.tar.gz` packages
- Skips local galaxies automatically
- HTTPS and other protocols require enabling transport features

---

### `cosmos show <star>`
Prints the metadata for a given Star.

- Looks up in memory, cache, or fetches if needed
- Prints name, version, description, license, dependencies

---

### `cosmos search <term>`
Searches across all loaded Galaxies.

- Matches against Star name and description
- Returns matching Star names

---

## � Flags

- `--offline`: Disables any network calls or on-demand fetches
- `--constellation <file>`: Install using a constellation file
- `--root <path>`: Override install root (default is `/`)
- `--stars`, `--full`: Optional flags for `sync`

---

## � Examples

```bash
cosmos install zlib
cosmos install --constellation ./presets/desktop.toml
cosmos uninstall zlib
cosmos status
cosmos sync --stars
cosmos show zlib
cosmos search dev
```

---

## 🧘 Future Commands

- `verify <star>`: Validate files against tarball hash
- `repair <star>`: Reinstall from cache without touching deps
- `freeze`: Write a lockfile for reproducible reinstalls

---

Cosmos CLI is intentionally boring: no shell magic, no YAML