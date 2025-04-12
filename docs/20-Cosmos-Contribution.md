# 20 – Contributing to Cosmos (Project Guidelines)

This guide is for contributors to the Cosmos codebase itself—including `cosmos-core`, `cosmos-cli`, `stellar`, `nova`, and related tooling.

---

## 🛠️ Project Philosophy
- Cosmos is modular, minimal, and predictable
- Contributions should avoid feature bloat or scope creep
- Every crate should do one thing well and be testable in isolation

---

## 🎓 Getting Started
1. Clone the repo
2. Use the latest stable Rust (`rustup update`)
3. Build the workspace:
   ```bash
   cargo build --workspace
   ```
4. Run all tests:
   ```bash
   cargo test --workspace
   ```
5. Optional: install `cargo-make` or `just` if you're using task runners

---

## 🔨 Code Guidelines
- Follow Rust 2021 edition idioms
- Use `thiserror` for error enums
- Favor `Result<T, CosmosError>` over panics
- Don’t use `unwrap()` unless you *really* mean it
- All FFI functions must be `#[no_mangle]` and use C types

---

## 🔄 Pull Requests
- One logical change per PR
- PR title should summarize purpose
- Document new CLI commands, options, or config fields
- Include tests where applicable
- Update relevant markdown docs in `/docs`

---

## 📊 Feature Flags
- Use `#[cfg(feature = "ffi")]` for optional components
- Don’t enable features by default unless essential

---

## ✏️ Docs and Comments
- Public functions should have doc comments
- Internal helpers can use line comments for clarity
- Markdown docs should be updated when adding major features

---

## 🌐 Licensing and Attribution
- All contributions are MIT licensed
- Add your name to the AUTHORS file if you wish
- Respect the project's tone: technical, clear, with dry humor allowed

---

## 🌟 Where to Help
- Finish Nova runtime logic
- Improve dependency resolution logic
- Add `stellar lint` / `stellar test` features
- Write sample Stars for testing and examples
- Write CLI integration tests

---

Cosmos is built for clarity and longevity. Contributions that make it more robust, understandable, and portable are always welcome.

