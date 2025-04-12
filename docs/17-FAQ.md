# 23 - Frequently Asked Questions (FAQ)

---

## 🤔 Why does Cosmos link to `libm.so.6` on glibc?

This is normal and expected.

Cosmos uses `flate2` for handling `.tar.gz` packages. That crate performs simple math operations, and due to how **glibc** handles math internally, some functions are dynamically linked to `libm.so.6`.

This only affects **glibc builds**. If you build Cosmos for **musl** (`x86_64-unknown-linux-musl`), it will be **fully static** and contain no dynamic dependencies.

### Summary:
- **glibc builds:** dynamically link `libc.so.6` and `libm.so.6` (expected)
- **musl builds:** fully static, `ldd` returns "not a dynamic executable"

---

## 💥 Isn’t this a security concern?

Only if your threat model includes trigonometry.

Linking `libm` doesn't affect the stability, reproducibility, or offline usage of Cosmos. It’s the same math library that ships with every glibc-based system.

If you need a **truly static** binary for initramfs, recovery, or embedded environments, we recommend using the `musl` target.

```bash
rustup target add x86_64-unknown-linux-musl
cargo build --release --target x86_64-unknown-linux-musl
```

---

## 🧱 What if I want to strip Cosmos down even more?

Set this in `.cargo/config.toml`:

```toml
[profile.release]
panic = "abort"
```

This can remove `libgcc_s.so.1` from glibc builds.
Use `strip` to remove debug symbols and reduce size:

```bash
strip ./target/release/cosmos-cli
```

---

## 🛰️ How small can the binary get?

With a musl static build + stripping, Cosmos CLI can be **under 4 MB**, even with Nova embedded. Most installs will work offline and fit in a bootable initramfs.

---

## 🧪 Why not use TLS or GPG?

Cosmos does not ship with TLS, GPG, or any central trust mechanism. You’re expected to:

- Host Galaxies from trusted sources (USB, internal HTTP, S3)
- Manually verify what you install
- Use `cosmos freeze` (coming soon) for lockfile verification

See the [Security Model](./15-Security.md) for full rationale.

---

## 🪐 Why is everything named after space stuff?

Because it’s internally consistent, poetic, and structurally accurate:

- Cosmos = the full system
- Galaxies = repositories
- Stars = packages
- Nebulae = meta-packages
- Constellations = install presets
- Universe = installed state
- Nova = installer scripting engine

It sounds fun, but it actually makes the entire system easy to reason about. You don’t need to memorize terminology. You just need to understand space.

---

## 🐚 Can I use Cosmos in my initramfs or rescue USB?

Absolutely. That’s what it’s built for.

The entire system is designed to:
- Be static (especially with musl)
- Work offline
- Install to any rootfs
- Run without Python, Bash, or shared libraries

If you can boot a shell and run a binary, you can use Cosmos.

---

## 🧪 Does Cosmos do dependency resolution?

Yes — minimal and predictable.

- Uses semver-style constraints defined in `star.toml`
- Resolves across configured Galaxies in order
- Does not re-resolve dependencies on update
- No graph solving, no “intelligent” magic

You get reproducibility over reactivity.

---

## 🧠 What’s the difference between Stars, Nebulae, and Constellations?

| Type          | Purpose                            |
|---------------|-------------------------------------|
| Star          | A package (with tarball and install logic) |
| Nebula        | A dependency-only meta-package     |
| Constellation | A user-defined install preset      |

Nebulae have no install script or files — they just express dependencies. Constellations are external TOML presets, not tracked in the system state.

---

## 🔍 Where is the package database?

There isn’t one. Cosmos tracks installs using a plain TOML file: `universe.toml`.

It logs:
- Installed Stars
- Their versions
- Their installed file paths

You can inspect it, edit it, back it up, and diff it manually. No SQLite, no JSON soup, no binary blobs.

---

## 👽 What if I want to contribute a FAQ question?

PRs welcome. Especially sarcastic ones.
