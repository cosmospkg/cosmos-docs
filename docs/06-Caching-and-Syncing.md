# 06 – Caching and Syncing

This document outlines how Cosmos handles syncing Galaxies and caching packages. It defines the levels of sync available, when things are fetched, and how to control offline behavior.

Cosmos does not run a daemon or maintain background state. Everything is initiated and controlled explicitly by CLI commands.

---

## ✨ Sync Levels
Cosmos provides tiered sync strategies based on user intent:

### `cosmos sync`
- Default sync behavior
- Downloads only `meta.toml` files from configured Galaxies
- Fastest, lowest bandwidth

### `cosmos sync --stars`
- Also downloads all `star.toml` files listed in each Galaxy's `meta.toml`
- Does **not** fetch tarballs
- Allows faster `install` calls later

### `cosmos sync --full`
- Fetches `meta.toml`, all `star.toml`s, and all `.tar.gz` packages if:
    - The `source` is cacheable (e.g., HTTP, relative file path)
- Use this to fully prime a system for offline use

---

## 🌊 Install Behavior

### `cosmos install foo`
- Looks for `foo.toml` in cached Galaxy dirs
- If not found, tries to download it on-demand (unless `--offline`)
- Checks for tarball in cache
- If missing and source is cacheable (HTTP), downloads it on-demand

### `cosmos install foo --offline`
- Will **fail** if either the `star.toml` or the package tarball is missing from cache
- Ensures a completely offline installation attempt

---

## 🚄 Local vs Remote Galaxies

### Local or Mounted Galaxies
- Sources like `file:///mnt/usb/galaxies/core` or `./galaxies/devtools`
- Treated as "live" directories
- **No caching is performed**
- `star.toml` and packages are read directly from disk
- Ideal for development, USB installs, or bundled Galaxies

### Remote Galaxies (HTTP/S3)
- Sources like `http://mirror.example.org/core`
- Always synced into the local cache
- `cosmos sync` determines what gets downloaded
- `cosmos install` pulls missing files if allowed

Cosmos will **never try to cache** a local Galaxy. If it’s already on disk, it’s trusted and read as-is.

---

## 🚫 What Cosmos Will Not Do
- No auto-sync on startup
- No background updates
- No index daemon
- No guessing what you need

Cosmos only syncs what you ask it to. No more, no less.

---

## 🏑 Future Enhancements (Optional)
- `--diff`: show what changed since last sync
- `--dry-run`: preview what would be downloaded
- `--prune`: delete unused packages from cache
- Configurable per-Galaxy sync level (e.g. `core = "full"`, `extra = "meta"`)

---

## ✅ Summary
| Command                 | What it does                             |
|------------------------|------------------------------------------|
| `cosmos sync`          | Fetches `meta.toml` only                 |
| `cosmos sync --stars`  | Fetches `meta.toml` + all `star.toml`s   |
| `cosmos sync --full`   | Fetches everything (meta, stars, tarballs) |
| `cosmos install foo`   | Pulls star and package if missing        |
| `--offline`            | Fails if not already cached              |

Cosmos sync is fully user-controlled and deterministic. No guesswork. No magic. No network calls you didn’t ask for.

