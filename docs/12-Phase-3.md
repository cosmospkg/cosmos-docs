# 12 – Phase 3 Features

This document outlines future enhancements and optional tools for Cosmos that go beyond the core system. These features are not required for the base install or use of Cosmos but are intended to expand its capabilities into a full OS ecosystem.

---

## 6. `cosmosd` (optional daemon, later)
> A lightweight, event-driven daemon for automation and system tasks.

### Purpose:
- Enable scheduled or triggered tasks without requiring a full init system
- Operate entirely opt-in and user-controlled

### Potential Features:
- Auto-update checker
- Background Galaxy sync and prefetching
- Trigger hooks on install/uninstall
- Optional D-Bus or UNIX socket interface for inter-process APIs

### Notes:
- Should never be required to use Cosmos
- Can run as a one-shot daemon or long-lived process depending on system

---

## 7. `cosmos-ui` (optional TUI or web frontend, someday)
> A terminal interface for browsing and interacting with Cosmos.

### TUI Features:
- Package browser for installed Stars and synced Galaxies
- Show descriptions, versions, file lists
- Enable/disable installs, trigger updates, view diffs
- Keyboard-centric navigation

### Web UI (future):
- Read-only status display for server environments
- Could integrate with `cosmosd` as backend

### Tech Options:
- TUI: `ratatui` (Rust), `cursive`
- Web: static export or minimal embedded HTTP server

---

## 8. `cosmos-sdk` / `cosmos-utils` (optional shared crate)
> Shared functionality for Cosmos-related tooling.

### Purpose:
- Reduce duplicated code across CLI, Stellar, Nova
- Centralize logic that doesn't belong in `cosmos-core`

### Common Modules:
- Logging and pretty output
- Config loading and validation
- Common errors, types, and file path handling
- Optional feature flags for ultra-minimal builds


---

## 9. `cosmos-init` / `cosmos-bootstrap`
> A bootstrap tool for building minimal systems using Cosmos from scratch.

### Purpose:
- Install a working userland with only Cosmos and a base Galaxy
- Enable installation of Wombat Linux or similar musl-based systems
- Function inside an initramfs or rescue shell

### Features:
- Unpack base system tarballs
- Resolve and install base Galaxy from local/remote source
- Can work without `/usr`, `/var`, or network

### Future Expansion:
- Could become a full stage 0 → working userland bootstrapper
- Define full installation profiles for automated builds

---

### 🔍 A Note on Git Integration
Cosmos will **not** use `libgit2`, shell out to `git`, or embed Git transport support.

If you want to host Galaxies in Git:
- Clone them manually
- Serve them via raw HTTP or GitHub Pages
- Use Git to version content, but not to sync it

Cosmos will **not** support `git://` as a protocol. You’re in charge of your own sync strategy. No magic.

---

These Phase 3 tools support extending Cosmos from a tool into a platform—one that is flexible enough for distros, embedded systems, recovery shells, or automation pipelines.

Note: Nova is no longer a future feature. It is fully stable and integrated into Cosmos, with install scripts now defaulting to `install.lua` in most flows.
