# 02 – Core Concepts

Cosmos is built around four core abstractions: **Stars**, **Constellations**, **Galaxies**, and the **Universe**. These concepts form the foundation of how packages are created, grouped, distributed, and tracked within a Cosmos-based system.

---

## ✨ Star

A **Star** is a single package. It can contain software, configuration, or simply reference other Stars. Stars are defined by a `star.toml` file and optionally include a tarball and install logic.

The `source` field supports both HTTP URLs and local file paths (e.g., `/mnt/usb/zlib.tar.gz`).  
Tarballs are extracted to a temporary directory before the install script runs.

### Key Fields:
```toml
name = "zlib"
version = "1.2.13"
description = "Compression library"
type = "normal"  # or "nebula"
source = "http://mirror.example.org/zlib-1.2.13.tar.gz"
install_script = "./install.lua"

[dependencies]
musl = ">=1.2.0"
```

### Star Types:
- `normal`: A real package with files to extract and install
- `nebula`: A virtual package that only contains dependencies (no source, no script)

Nebulae can define system roles, such as:
```toml
name = "dev-base"
type = "nebula"

[dependencies]
busybox = "^1.36"
zlib = ">=1.2"
gcc = "^13"
```

---

## 🌌 Galaxy

A **Galaxy** is a collection of Stars—functionally a package repository. It may be hosted remotely over HTTP (no TLS), on a USB stick, in a local directory, or via Git.

### Structure:
```
Galaxy-core/
├── meta.toml
├── packages/
│   ├── zlib-1.2.13.tar.gz
│   └── busybox-1.36.0.tar.gz
├── stars/
│   ├── zlib.toml
│   └── busybox.toml
```

### meta.toml (example):
```toml
name = "core"
description = "Core system packages"
version = "2024.04"
```

Galaxies are synced with the system via `cosmos sync`, and used during installs or updates.

---

## 🌍 Constellation

A **Constellation** is a simple TOML file that defines a group of Stars. It is *not* a package and is *not* tracked in the system state. Constellations act as presets or curated bundles for easy install.

### Example:
```toml
name = "desktop"
description = "Minimal desktop experience"
members = [
  "xorg",
  "wayland",
  "alacritty",
  "firefox"
]
```

Constellations can be installed with:
```
cosmos install --Constellation desktop
```

---

## 👩‍💻 Universe

The **Universe** is the local system's package state. It is defined by `universe.toml` and tracks:
- Installed Stars
- Their versions
- Files installed by each Star

### universe.toml Example:
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

The Universe is updated automatically by Cosmos when Stars are installed or removed. It can be inspected, versioned, or backed up for reproducibility.

---

## ✨ Relationships Summary

| Concept           | Description                                 | Tracked in `universe.toml`? |
|-------------------|---------------------------------------------|-----------------------------|
| **Star**          | A single package definition + install logic | Yes                         |
| **Nebula**        | A dependency-only Star (no install logic)   | Yes                         |
| **Galaxy**        | A remote or local repo of Stars             | No                          |
| **Constellation** | A list of Stars to install together         | No                          |
| **Universe**      | Tracks what is installed + file lists       | Yes (by definition)         |

Each of these concepts supports the Cosmos goal of minimal, reproducible, resilient system builds—from a live ISO to a hand-assembled rootfs.

