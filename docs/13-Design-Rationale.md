# 13 – Design Rationale and Philosophy

This document explains the "why" behind key design decisions in Cosmos. It exists to clarify intentional tradeoffs and to serve as a personal or public reference when people ask, "why didn't you just use X?"

---

Cosmos was built by someone who stared at a broken Linux install one too many times and thought:
> "What if I could rebuild this entire system without Python, Bash, or crying?"

So here we are.

Cosmos doesn’t care about your desktop theme, your distro war, or your 14-layer init system. It cares about one thing:
> **Can you install a working system using nothing but a static binary and a tarball?**

If yes, you win. If not, you probably ran something written in Python.

---

## 🌌 Why not APT, Pacman, DNF, etc?
Because those are built for general-purpose desktop Linux distributions. Cosmos is built for:
- Custom distros
- Initramfs environments
- Musl-based systems
- Fully static systems
- Recovery and self-repair tools

Those package managers rely on:
- Python
- Bash
- D-Bus
- TLS
- Dynamic linking

Cosmos relies on:
- libc

---

## 🦄 Why not shell scripts?
Shell is powerful, but:
- Unsafe (every space or quote is a landmine)
- Unportable (dash vs bash vs busybox)
- Not predictable (subshells, environment leakage)

Nova gives us:
- A safe, controlled scripting environment
- Cross-platform compatibility
- Clean separation of logic from shell quirks

---

## 🚀 Why Rust?
- No runtime
- No interpreter
- Memory safe
- Compiles to a static binary
- You can run Cosmos in an initramfs without libc quirks, dynamic libs, or runtime surprises

Rust is a means to an end: predictable binaries that don't break when the system does.

---

## ❌ Why no HTTPS?
- TLS requires runtime libs (OpenSSL, etc.)
- Certificates expire
- Embedded systems don’t need encrypted package downloads

You can:
- Use HTTP over trusted networks
- Mirror to USB, S3, or local filesystems
- Add your own transport security if needed

---

## ⚡ Why not Nix?
Nix is brilliant, but also:
- Complex
- Immense
- Based on its own language and model

Cosmos is not trying to do everything. It's trying to do *the right thing* at the right time.

---

## 💡 Why TOML?
- Human-readable
- Easy to parse
- Less brittle than YAML
- More structured than JSON

---

## ✅ Why modular crates?
- So you can embed Cosmos into anything
- So you can build your own TUI, API, init tool, or distro installer
- So Stellar, Nova, and the CLI don’t step on each other

---

## 🚫 Why not make it easier?
Cosmos is not trying to be the easiest tool. It's trying to be the one that works *when everything else breaks*.

It’s not built for everyone. It’s built for:
- Developers
- Tinkerers
- Custom OS builders
- People who need to fix a system with nothing but a tarball and a dream

