# 12 – FFI Integration

This document describes the Foreign Function Interface (FFI) support in Cosmos. While Cosmos is written entirely in Rust, it supports optional C ABI exports for integration into non-Rust projects, C-based systems, or minimal userspace tooling.

---

## ⚙️ FFI Philosophy
- By default, the Cosmos ecosystem is pure Rust
- FFI is provided for external system tools or low-level integrations
- Headers are auto-generated using `cbindgen`
- The `cosmos-core` crate can be built as a shared library (`cdylib`)

---

## 🔧 Building for FFI

In `cosmos-core/Cargo.toml`:
```toml
[lib]
crate-type = ["cdylib"]
```

In your build command:
```bash
cargo build --release --lib
```

This will produce a `.so`, `.dylib`, or `.dll` depending on your platform.

---

## 🔮 Header Generation with cbindgen

To produce a C-compatible header:

1. Add a `cbindgen.toml`:
```toml
[library]
include-version = true
[parse]
parse_deps = false
[output]
language = "C"
```

2. Run:
```bash
cbindgen --config cbindgen.toml --crate cosmos-core --output cosmos.h
```

---

## ⚖️ FFI Exports (Planned)

```c
int cosmos_install(const char* star_name);
int cosmos_uninstall(const char* star_name);
int cosmos_update(const char* star_name);
const char* cosmos_last_error();
```

- Errors are returned as codes, with optional access to `cosmos_last_error()` for string messages
- `const char*` inputs are assumed to be UTF-8 null-terminated strings

---

## 🌐 Use Cases
- Call Cosmos from a shell written in C
- Embed Cosmos into a microkernel userspace
- Integrate with FFI-compatible build systems or init tools
- Future: Cosmos TUI written in Zig or C could use the shared library

---

## ✨ Defaults
- Cosmos compiles as a regular Rust library by default
- FFI is **opt-in** via build and `#[no_mangle]` exports
- No impact on internal ecosystem structure

---

FFI support ensures Cosmos can participate in truly minimal environments, including places where Rust is not the host language, while keeping the core ecosystem clean and fully idiomatic.

