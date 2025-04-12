# 03 – File Formats

This document defines the key file formats used throughout the Cosmos ecosystem. All formats are plaintext and designed to be human-readable, primarily using **TOML**. These formats cover packages, system state, groups, and repository metadata.

---

## 📄 star.toml
Defines a single **Star** package.

```toml
name = "zlib"
version = "1.2.13"
description = "Compression library"
type = "normal"  # or "nebula" / "meta" for meta packages
source = "http://mirror.example.org/zlib-1.2.13.tar.gz"
install_script = "install.lua"
license = "MIT"

[authors]
afroraydude = "afro@wombat.linux"

[dependencies]
musl = ">=1.2.0"
```

### Required Fields:
- `name`: unique name of the star
- `version`: SemVer-compatible version string
- `type`: `normal` (default) or `nebula`

### Optional Fields:
- `description`: human-readable description
- `source`: URL or path to a tarball
- `install_script`: shell script or nova-compatible command string
- `license`: SPDX-style license string or plain name (e.g. "MIT")
- `[authors]`: map of contributor names to emails
- `[dependencies]`: map of star names to version requirements

---

## 🔍 universe.toml
Tracks the system's currently installed stars and their associated files.

```toml
[system]
arch = "x86_64"
version = "1.0"

[installed.zlib]
version = "1.2.13"
files = [
    "/usr/lib/libz.so",
    "/usr/include/zlib.h"
]

[installed.busybox]
version = "1.36.0"
files = [
    "/bin/busybox"
]
```

### Sections:
- `[system]`: system metadata
- `[installed.<name>]`: installed star metadata

---

## 🌌 constellation.toml
Defines a **Constellation**: a named list of stars, used as an install preset.

```toml
name = "desktop"
description = "Base desktop setup"
members = [
    "xorg",
    "firefox",
    "wayland",
    "alacritty"
]
```

### Fields:
- `name`: name of the constellation
- `description`: optional text
- `members`: list of star names

Constellations are not packages and are not tracked in the Universe.

---

## 🌍 galaxy/meta.toml
Metadata about a **Galaxy** (i.e. a repository of stars).

```toml
name = "core"
description = "Core system packages"
version = "2025.04.01"

[stars]
zlib = "1.2.13"
busybox = "1.36.0"
```

### Fields:
- `name`: the name of the galaxy
- `description`: brief overview
- `version`: a human-readable version tag for the repository snapshot
- `stars`: list of star names in the galaxy (required for HTTP/S3 hosting)

---

## 🔗 galaxy layout (filesystem)
Galaxies are just static folders, easily hosted over HTTP, Git, or even a mounted drive.

```
core-galaxy/
├── meta.toml
├── packages/
│   ├── zlib-1.2.13.tar.gz
│   └── busybox-1.36.0.tar.gz
├── stars/
│   ├── zlib.toml
│   └── busybox.toml
```

- `meta.toml` → info about the galaxy, including star list
- `stars/` → individual star definitions (1 per package)
- `packages/` → optional tarballs referenced by `source` fields in star files

This layout is portable, versionable, and supports offline or air-gapped installs.

---

## 📁 config.toml
The main Cosmos config file defines the install root, cache path, and known Galaxy sources.

```toml
[galaxies]
core = "http://mirror.example.org/core"
extra = "file:///mnt/usb/galaxies/extra"
dev = "./galaxies/devtools"

install_dir = "/"
cache_dir = "/var/lib/cosmos"
```

### Fields:
- `[galaxies]`: map of galaxy name to URL or path
- `install_dir`: where packages are installed
- `cache_dir`: where synced files are stored

---

All Cosmos formats prioritize:
- Human readability
- Git friendliness
- Simplicity and forward compatibility
- Minimal external tooling

They are designed so that an entire package ecosystem can be stored, edited, and mirrored using only a text editor and tar.

