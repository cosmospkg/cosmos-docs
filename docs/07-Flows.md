# 07 – Flows

This document outlines the core operational flows of the Cosmos package system, covering installation, uninstallation, updates, and syncing. Cosmos keeps behavior explicit and modular: CLI tools handle orchestration, while core libraries handle deterministic actions like installs and file tracking.

---

## ⬆️ Install Flow (`cosmos install <star>`)

### Summary:
Install a Star and its dependencies, supporting local or remote Galaxies, with optional offline mode.

### Steps:
1. **CLI** loads system `config.toml` and current `universe.toml`
2. **CLI** loads all cached **Galaxies** (repositories)
3. **CLI** searches for the requested Star across all Galaxies
4. **CLI** selects the matching Galaxy + Star and passes it to `install_star()`
5. **Core** checks if the Star is already installed in the Universe
6. **Core** resolves and installs dependencies recursively:
   - Skip if already installed
   - Otherwise, locate the dependency and install it the same way
7. **Core** downloads the `.tar.gz` if needed (unless in offline mode)
8. 8. **Core** extracts the tarball to a temporary directory and runs the installation script (if any) from that location

9. **Core** updates `universe.toml` with installed files

---

### ⚡ Offline Mode
- `--offline` flag blocks any HTTP or file downloads
- If required files are missing from the cache or local repo, install fails

---

### 🚜 Local vs Remote Galaxies
- **Local Galaxies** (e.g. `file:///` or mounted path): read directly from disk
- **Remote Galaxies**: read from cache or fetch from `url` as needed

---

### 🔍 Dependency Resolution
- Recursively installs each dependency
- Finds the first galaxy with a matching version of the required Star.

---

### ✨ Notes
- Nebulae (meta Stars) are processed the same way but do not extract files or run scripts
- Dependencies are resolved by searching Galaxies in priority order, returning the first match that satisfies the constraint.
- CLI must ensure the target Star and Galaxy are found before calling core install
- Core handles everything after that: caching, extraction, install script, tracking
- Tarballs are always extracted to a temporary directory before installation; Nova scripts execute from this temp directory and install files into `install_root`

---

This split ensures the CLI can be customized per distro/environment, while the core logic stays portable, pure, and deterministic.

## ❌ Uninstall Flow (`cosmos uninstall <star>`)

### Summary:
Remove a Star and its files from the system.

### Steps:
1. Load system `config.toml` and `universe.toml`
2. Check if the Star is installed
3. If not found, return an error
4. Delete all recorded file paths
5. Remove the Star entry from `universe.toml`

### Notes:
- Cosmos does **not** track reverse dependencies
- If uninstall breaks another Star, that's on the user
- Nebulae uninstall instantly as they have no files

---

## ↩️ Update Flow (`cosmos update <star>`)

### Summary:
Upgrade a Star if a newer version is available in any synced Galaxy.

### Steps:
1. Load `universe.toml` and Galaxy cache
2. Compare installed version to available versions
3. If newer version matches semver constraints:
    1. Uninstall the current version
    2. Install the new version via standard flow

### Notes:
- If the Star is not installed, fallback to `install`
- Cosmos does not automatically update all dependencies (manual per-Star)
- Future enhancement: `cosmos upgrade` to update all upgradable Stars

---

## 🧰 Sync Flow (`cosmos sync`)

### Summary:
Download Galaxy metadata, Stars, and (optionally) packages to prepare for installation and offline use.

### Steps:
1. Load `config.toml` and extract the `[galaxies]` table
2. For each Galaxy:
   - If the URL is local (`file://`, relative, or `/`) → skip
   - If the URL is HTTPS → abort (not supported)
   - Otherwise download `meta.toml`
3. Depending on sync level:
   - `--stars`: download each listed `star.toml`
   - `--full`: also download each tarball listed in its `source` field (if HTTP)
4. Store all results in `{cache_dir}/galaxies/<name>/`

### Notes:
- Cosmos does **not** sync local/mounted Galaxies—they are read directly
- TLS is not supported—HTTP only
- `meta.toml` must include a `[stars]` table with version strings
- Sync is one-shot and explicit—no background updates, no auto-indexing

---


These flows define the fundamental operations of Cosmos, emphasizing transparency, reproducibility, and total independence from external system dependencies.

