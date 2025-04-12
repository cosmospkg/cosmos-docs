# 14 – Security Model

Cosmos takes a pragmatic approach to security. It prioritizes simplicity, verifiability, and deterministic behavior over complex trust chains or opaque cryptographic systems. This document outlines the current and planned security considerations.

---

Let’s get something out of the way:
> If you’re mad there's no TLS, no GPG, and no certificate pinning, you might be looking for the wrong package manager.

Cosmos is built for systems that are already trusted, constrained, or purpose-built. If you're bootstrapping from 
nothing or managing an offline system, TLS and GPG are often just extra ways for things to break.

Instead, Cosmos gives you:
- Plain files
- Predictable layouts
- Auditable integrity

Security is something you layer on *if* and *when* you need it.

---

## 🔐 Trust Model
- Trust is placed in the source of the Galaxy you sync
- Galaxies are expected to be hosted by trusted parties (e.g. your own USB, S3, HTTP mirror, or Git repo)
- There is **no central verification authority**

---

## ❌ No TLS Required
- Cosmos intentionally does **not require HTTPS** or OpenSSL
- This removes runtime SSL dependencies (libssl, cert bundles, etc.)
- Galaxies can be served from:
  - Plain HTTP
  - USB drives
  - File paths
  - IPFS or Git

> If you want encrypted transport, you can use `scp`, `rsync`, `tailscale`, or just unplug your damn machine.

> If you don’t trust your Galaxy source, you have no security.

You are responsible for trusting the transport medium.

---

## 🔢 Integrity
- Future: hashes of downloaded Star tarballs can be included in `star.toml`
- Future: `cosmos verify` command will compare installed files to original tarball
- Future: `cosmos freeze` lockfiles will pin exact versions and hashes

---

## ✨ Nova Safety
- Nova is a **restricted Lua runtime** with no `os.execute`, no global `io`, and no raw filesystem access
- All file ops go through Cosmos' internal API (e.g. `copy`, `run`, `symlink`)
- All paths are forced under a real root prefix passed in by Cosmos

---

## ⚠️ Responsibility Statement
> Cosmos does not attempt to "secure" things with signatures or crypto if the system using it is already untrusted.

This philosophy mirrors tools like Alpine and Suckless:
- Focus on small, auditable parts
- Encourage system owners to vet their inputs
- Let users opt into higher security layers (signed hashes, reproducible builds) without mandating them

---

## 🛡️ Future Ideas
- Optional GPG-signed `meta.toml`
- Signed `universe.lock.toml` for immutable system reproducibility
- Support for hash pinning and binary transparency (optional, not core)

Cosmos aims to be secure by design, not by bureaucracy.

