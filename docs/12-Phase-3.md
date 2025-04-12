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

## 10. `cosmos-utils` (minimal core command set)
> A BusyBox-style bundle of essential CLI tools written in Rust, designed to be used inside minimal or recovery environments.

### Purpose:
- Provide basic utilities (`ls`, `cp`, `rm`, `echo`, `mkdir`, etc.) for systems without BusyBox or coreutils
- Act as a lightweight fallback for file ops, inspection, and manual recovery
- Fully static and shell-free — complements Cosmos/Nova usage in rescue shells or initramfs
- Centralize low-level commands for future Cosmos bootstrapping tools

### Design Goals:
- Single binary with `argv[0]` dispatch (like BusyBox)
- Small, dependency-free Rust tools that mirror expected behavior
- Pretty, Cosmos-style output (errors, logs, confirmations)
- Built to integrate well with `nova` and `cosmos-bootstrap`

### Example Commands:
- `cosmos-utils ls`  
- `cosmos-utils cp`  
- `cosmos-utils echo`  
- `cosmos-utils rm`  
- `cosmos-utils mkdir`  
- …or use via symlink: `ls -> cosmos-utils`, `rm -> cosmos-utils`

### Extras (Optional):
- Dry-run mode for destructive ops (`--pretend`)
- Snarky or dry-humored warnings (if enabled)
- Nova helper integration (`nova shell`-safe)

### Future Expansion:
- Include in Galaxy presets for recovery shells
- Optionally bundled into static rescue tarballs
- `--coreutils` Cargo feature flag to include or exclude utilities as needed

Cosmos-utils allows for fully shell-less systems with just Cosmos, Stellar, and Nova. Combined with `cosmos-bootstrap`, it enables entirely scriptable installs using only Cosmos-native tools.

---

These Phase 3 tools support extending Cosmos from a tool into a platform—one that is flexible enough for distros, embedded systems, recovery shells, or automation pipelines.

Note: Nova is no longer a future feature. It is fully stable and integrated into Cosmos, with install scripts now defaulting to `install.lua` in most flows.
