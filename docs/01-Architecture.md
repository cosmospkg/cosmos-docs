# 01 – Cosmos Architecture

Cosmos is built as a modular, layered system with a clear separation of responsibilities. It is designed to function in environments ranging from minimal embedded systems to full desktop Linux setups, while maintaining reliability and transparency.

---

## 🧱 Layered Overview

```
+-----------------------------+
|      cosmos-cli             | ← User interface (CLI)
+-----------------------------+
|      cosmos-core            | ← Core logic: install, uninstall, deps
+-----------------------------+
|      cosmos-transport       | ← Remote fetch handling (HTTP required, HTTPS optional)
+-----------------------------+
|      cosmos-universe        | ← Installed system state
+-----------------------------+
|      stellar (tooling)      | ← Maintainer tools for building packages
+-----------------------------+
|      nova (optional)        | ← Lua-based scripting engine for portability
+-----------------------------+
|  Filesystem / libc / tar.gz |
+-----------------------------+
```

---

## 🔧 Crates / Components

### 1. `cosmos-core`
> Core logic for everything: installs, dependency resolution, interacting with constellation indexes and the universe.

- Accepts `Star` and `Galaxy` objects
- Handles:
  - Dependency resolution (with semver support)
  - Downloading and extracting tarballs
    - **Delegates all remote fetching to `cosmos-transport`**
    - **Handles `file://` paths directly**
- Exposes `install_star`, `uninstall_star`, `update_star` functions

---

### 2. `cosmos-transport`
> A required subcrate responsible for all non-`file://` fetches.

- Used by `cosmos-core` to fetch Stars, Galaxies, and tarballs
- Dispatches by protocol:
  - `http://` → supported by default (via `ureq`)
  - `https://` → **opt-in via `https` feature or `transport-https` feature in `comos-core`**
  - `ipfs://`, `ftp://`, etc. → available via additional features
  - `file://` → *not* handled here (native to `cosmos-core`)
- Built by default with `http`. Custom builds can slim it down:

```toml
default-features = false
features = ["http"]
```

---

### 3. `cosmos-cli`
> Terminal interface for users, using `clap`.

Implements commands like:

- `cosmos install foo`
- `cosmos uninstall foo`
- `cosmos update foo`
- `cosmos status`
- `cosmos sync`

The CLI **calls into `cosmos-core`** and reads config from a central TOML file.

---

### 4. `cosmos-universe`
> A standalone crate for tracking system state.

- Defines `universe.toml`
- Tracks:
    - All installed stars
    - Version and file path information
- Called by `cosmos-core` to read/write state
- No side effects: it is **pure data access**

---

### 5. `stellar`
> Maintainer-side tool for **creating and building packages**.

Functions:

- Create new `star.toml` scaffolds
- Build package tarballs
- Run pre- and post-build tests
- Lint and validate packages

Future versions may integrate with `nova`.

---

### 6. `nova` *(optional)*
> Portable scripting engine using **Lua**.

Designed to replace raw shell scripts with something safer and more portable.

- Runs inside install contexts
- Exposes helper functions like `run("make")`, `copy()`, etc.
- Optional and pluggable: not required for stars to function

---

## 📂 Filesystem Expectations

- Cosmos **can install packages from**:
    - HTTP endpoints (enabled by default)
    - HTTPS (optional via feature)
    - Local file paths (cache/mounted disk/USB)
- Cosmos uses a cache directory for all downloaded packages
- Install directory is fully configurable
- No FHS enforcement unless desired (can run inside `/cosmos`, `/mnt/sysroot`, etc.)

---

## 🥪 Stateless CLI, Stateful System

- The **CLI is stateless**
    - It only operates via config files and universe data
- The **system state** is managed by `cosmos-universe`
    - Easily inspectable and replaceable
    - No central database or binary blob

---

## 🔧 Example Flow

### `cosmos install zlib`

1. CLI parses command
2. CLI loads config + constellations
3. `cosmos-core::install_star()` is called
4. Dependencies are resolved recursively
5. Star is downloaded (or used from cache)
6. Star is extracted and script is run
7. `cosmos-universe` is updated with star info
