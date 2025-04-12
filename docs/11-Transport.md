# 22 – Transport Layer (cosmos-transport)

This document defines the purpose and structure of the `cosmos-transport` crate, which handles remote content fetching in a clean, modular, and Cosmos-aligned way.

> cosmos-transport exists to provide network functionality without changing the trust model of Cosmos. All downloads are assumed to come from already-trusted sources.

---

## 🌐 What is cosmos-transport?

`cosmos-transport` is a standalone crate responsible for fetching remote content used by Cosmos. It handles:

- Downloading `.tar.gz` files during install
- Syncing Galaxy metadata over HTTP
- (Eventually) supporting alternative transports like HTTPS, FTP, or others

By isolating this logic, Cosmos remains modular and auditable.

---

## ✅ Core Philosophy

- Transport is **pluggable**, not assumed
- `cosmos-core` should not link any HTTP libraries directly
- No TLS or HTTPS unless explicitly enabled
- No background syncs, retries, or hidden redirects

---

## 📦 What Lives in This Crate?

### ✅ Current Behavior:
- `fetch_bytes(url: &str) -> Result<Vec<u8>, TransportError>`

Supports:
- `http://` via `ureq`
- TLS is NOT enabled by default

### ❌ Not included:
- `file://` logic – handled in `cosmos-core`
- Caching – handled elsewhere
- Mirrors – planned, not implemented

---
ƒ
## 🔧 Feature Flags

```toml
[features]
default = ["http"]
http = ["ureq"]
tls = ["ureq/tls"]
