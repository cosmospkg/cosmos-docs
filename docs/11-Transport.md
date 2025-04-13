# 11 – Transport Layer (cosmos-transport)

This document defines the purpose and structure of the `cosmos-transport` crate, which handles remote content fetching in a clean, modular, and Cosmos-aligned way.

> `cosmos-transport` exists to provide network functionality **without changing the trust model** of Cosmos. All downloads are assumed to come from already-trusted sources.

---

## 🌐 What is cosmos-transport?

`cosmos-transport` is a standalone crate responsible for fetching remote content used by Cosmos. It handles:

- Downloading `.tar.gz` files during install
- Syncing Galaxy metadata over HTTP
- Optionally supporting alternative transports like HTTPS, FTP, or IPFS

By isolating this logic, Cosmos remains modular, auditable, and easy to reason about.

---

## ✅ Core Philosophy

- Transport is **pluggable**, not assumed
- `cosmos-core` should not link any HTTP libraries directly
- `cosmos-transport` is always compiled, but only minimal features are enabled by default
- **HTTP is required** and included by default
- **HTTPS and TLS are optional features**
- No background syncs, retries, or hidden redirects

---

## 📦 What Lives in This Crate?

### ✅ Current Behavior:
- `fetch_bytes(url: &str) -> Result<Vec<u8>, TransportError>`

Supports:
- `http://` via `ureq` (enabled by default)
- `https://` via `ureq/tls` (optional via feature flag)

### ❌ Not included:
- `file://` logic – handled in `cosmos-core`
- Caching – handled elsewhere
- Mirrors – planned, not implemented

---

## 🔧 Feature Flags

```toml
[features]
default = ["http"]
http = ["ureq"]
tls = ["ureq/tls"]
```

- `cosmos-transport` is always used by `cosmos-core`
- Only the `http` feature is included by default
- The `https` capability must be enabled manually via the `transport-https` feature flag in `cosmos-core`

---

This design allows Cosmos to support remote access in constrained systems **without requiring TLS, shared libraries, or complex network stacks**. Future protocols can be implemented behind feature flags without affecting the trust model, performance, or predictability of the core system.

