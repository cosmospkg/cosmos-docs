# 17 - Frequently Asked Questions (FAQ)

---

## 🤔 Why does Cosmos link to `libm.so.6` and `libgcc_s.so.1` on glibc?

This is normal and expected behavior on **glibc-based systems**.

Cosmos is designed to be as lightweight and portable as possible. When compiled for glibc, it dynamically links to:

- `libc.so.6`: the GNU C library (standard)
- `libm.so.6`: the math library
- `libgcc_s.so.1`: GCC support library

These are all part of a standard glibc runtime environment and are available on **every major Linux distribution**.

---

### 🔧 Why is `libm` needed?

Cosmos uses the `flate2` crate to handle `.tar.gz` package extraction. Internally, this crate (or one of its dependencies) performs basic math operations like floating-point multiplication. On glibc, these are handled by `libm` instead of inlining or statically linking them.

---

### 🌌 Why is `libgcc_s.so.1` required?

This comes from the Rust compiler toolchain, which depends on certain runtime support features provided by GCC's libgcc. This includes things like stack unwinding and low-level math operations.

Unless you're using a custom toolchain, **libgcc is already included** on all glibc-based systems.

---

## 🚀 What about musl builds?

If you build Cosmos using the **musl target**, the result is a:

- **Fully static binary**
- No dynamic dependencies
- Compatible with initramfs, containers, and minimalist distros

You can build it like this:

```bash
rustup target add x86_64-unknown-linux-musl
cargo build --release --target x86_64-unknown-linux-musl
```

Then run `ldd` on the result:

```bash
$ ldd cosmos
    not a dynamic executable
```

---

## 💥 Isn't this a security concern?

Only if your threat model includes **floating-point arithmetic and basic division**.

Linking to `libm` and `libgcc` is completely standard for glibc builds. If you require complete static linking for security, reproducibility, or environment constraints, we recommend using the musl build.

---

### ✅ Summary

| Target | Static? | Linked Libraries |
|--------|---------|------------------|
| `glibc` | No      | `libc`, `libm`, `libgcc` |
| `musl`  | Yes     | None (fully static)        |

Cosmos is built **offline-first** and is functional in both environments. Your math still works. Your packages still install. Your system is still safe.

**You're fine.**

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

## � How small can the binary get?

With a musl static build + stripping, Cosmos CLI can be **under 4 MB**, even with Nova embedded. Most installs will work offline and fit in a bootable initramfs.

---

## 🧪 Why not use TLS or GPG?

Cosmos does not **require** TLS, GPG, or any central trust mechanism.

Instead, you’re expected to:

- Host Galaxies from trusted sources (USB, internal HTTP, S3)
- Manually verify what you install
- Use `cosmos freeze` (coming soon) for lockfile verification

TLS **can** be enabled via the `transport-https` feature. By default, only `http://` and `file://` are supported.

See the [Security Model](./15-Security.md) for full rationale.

---

## 💭 Why is everything named after space stuff?

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