# 21 – Crate Policy

Cosmos is built to run in minimal, musl-based, interpreter-free environments with zero dynamic linking. This document defines what crates are allowed and what constraints they must follow to be included in the Cosmos ecosystem.

---

## 🧱 Build Target Philosophy

- **musl-first**, **libc-agnostic**
- If Rust can target it, Cosmos should build on it
- All code must build with **just `cargo build --release`**
- No external build systems, no `make`, no `pkg-config`, no `cc` crates

---

## ✅ Allowed Crate Behavior

All crates must:

- Compile statically (no dynamic linking)
- Be **pure Rust**, or optional FFI that can be disabled
- Avoid build scripts that invoke system tools
- Build with **no extra linker args** under musl or glibc

---

## 🔐 Cryptography & Hashing

Only **pure Rust** implementations allowed.

### ✅ Approved:
- `sha2` (for SHA-256/SHA-512)
- `blake3`
- `digest`
- `md-5`
- `hex`

### ❌ Forbidden:
- `ring` (requires C + custom asm)
- `openssl` / `native-tls` (requires dynamic libs)
- `libgit2`, `libz`, or anything with a `-sys` suffix

---

## 📦 Compression & Archiving

### ✅ Safe:
- `flate2` with `features = ["miniz_oxide"]` and `default-features = false`
- `tar`

### ❌ Unsafe:
- `flate2` with `zlib` feature
- Any crate that uses system-level compressors

---

## 🛠 Feature Flag Rules

- Optional features are great. Required features with native code are not.
- Default features must be **fully statically linkable**
- All crates should have the ability to compile cleanly for:
  - `x86_64-unknown-linux-musl`
  - `aarch64-unknown-linux-musl`
  - `x86_64-unknown-linux-gnu`

---

## 🔬 Crate Audit Tips

Before adding a new crate:
```bash
cargo tree -e normal
```
Check for:

- `-sys` crates
- Dynamic linking
- Build scripts (`build.rs`) doing sketchy stuff
- Optional features that need to be disabled

---

## 🧼 Our Build Environment

- `cargo build --release` must be enough
- No external toolchains, no linker magic
- CI or local builds should always work under:
  ```bash
  RUSTFLAGS="-C target-feature=+crt-static"
  ```

---

## ✅ Summary

| Requirement           | Policy                              |
|------------------------|--------------------------------------|
| TLS Support            | ❌ None (Cosmos does not do TLS)     |
| Dynamic Linking        | ❌ Forbidden                         |
| Shell Usage            | ❌ Forbidden                         |
| C Libraries            | ❌ Forbidden unless **optional** and unused |
| Cargo Build-Only       | ✅ Required                          |
| musl Compatibility     | ✅ Required                          |
| glibc Compatibility    | ✅ Nice to have                      |

---

Cosmos should build anywhere Rust does, with **nothing but `cargo build` and a dream**.
