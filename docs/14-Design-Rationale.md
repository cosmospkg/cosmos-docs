# 14 – Design Rationale and Philosophy

This document explains the "why" behind key design decisions in Cosmos. It exists to clarify intentional tradeoffs and to serve as a personal or public reference when people ask, "why didn't you just use X?"

**TL;DR:**  
Cosmos is a Rust-based, static, musl-first package manager designed to work when everything else breaks *or when you deliberately don't want the rest of it.*  
No Python. No Bash. No TLS *required*. If you can untar it, you can install it.

---

Cosmos was built by someone who stared at a broken Linux install one too many times and thought:
> "What if I could rebuild this entire system without Python, Bash, or crying?"

So here we are.

Cosmos doesn’t care about your desktop theme, your distro war, or your 14-layer init system. It cares about one thing:
> **Can you install a working system using nothing but a static binary and a tarball?**

If yes, you win. If not, you probably ran something written in Python.

But here's the twist:
> Cosmos also works when you're *not* in panic mode. It's just as happy living in an embedded system or on a purpose-built, no-frills Linux box.

---

## 🌌 Why not APT, Pacman, DNF, etc?
> These are built for desktops. Cosmos is built for bare metal and broken installs.

Because those are built for general-purpose desktop Linux distributions. Cosmos is built for:

- Custom distros
- Initramfs environments
- Musl-based systems
- Fully static systems
- Recovery and self-repair tools
- Embedded and headless systems

Those package managers rely on:

- Python
- Bash
- D-Bus
- TLS (and certificate chains)
- Dynamic linking

Cosmos relies on:

- libc

---

## 🦄 Why not shell scripts?
> Because shell scripts are quote mines and hell on wheels.

Shell is powerful, but:

- Unsafe (every space or quote is a landmine)
- Unportable (dash vs bash vs busybox)
- Not predictable (subshells, environment leakage)

Nova gives us:

- A safe, controlled scripting environment
- Cross-platform compatibility
- Clean separation of logic from shell quirks
- The ability to run even on embedded systems without needing a shell at all

---

## 🚀 Why Rust?
> Because C is a land of undefined behavior and segfaults.

- No runtime
- No interpreter
- Memory safe
- Compiles to a static binary
- You can run Cosmos in an initramfs without libc quirks, dynamic libs, or runtime surprises

Rust is a means to an end: predictable binaries that don't break when the system does.

---

## ❌ Why no HTTPS?
> Because sometimes you just want to install a file without bringing half of OpenSSL with it.

- TLS requires runtime libs (OpenSSL, etc.)
- Certificates expire
- Embedded systems don’t need encrypted package downloads

Cosmos uses `http://` and `file://` by default. If you want HTTPS:

- Enable the `transport-https` feature in your build
- Use `cosmos-transport` to extend supported protocols
- Or mirror to USB, S3, or local filesystems

This keeps the base lean while giving you control over the security model.

---

## ⚡ Why not Nix?
> Because Nix is a black hole of complexity.

Nix is brilliant, but also:

- Complex
- Immense
- Based on its own language and model

Cosmos is not trying to do everything. It's trying to do *the right thing* at the right time.

---

## 💡 Why TOML?
> It parses predictably, it reads clean, and it won’t gaslight you like YAML.

- Human-readable
- Easy to parse
- Less brittle than YAML
- More structured than JSON
- YAML is great for writing configs that never parse the same way twice.

---

## ✅ Why modular crates?
> So you can rip Cosmos apart and use it wherever you want.

- So you can embed Cosmos into anything
- So you can build your own TUI, API, init tool, or distro installer
- So Stellar, Nova, and the CLI don’t step on each other

---

## ❌ Why not make it easier?
> Because this isn't for everyone—it's for the ones left fixing systems at 3AM with nothing but tar and despair.

It’s not built for everyone. It’s built for:

- Developers
- Tinkerers
- Custom OS builders
- People who need to fix a system with nothing but a tarball and a dream
- People who deploy Linux to tiny boxes with no runtime to spare

Cosmos is not trying to be the easiest tool. It's trying to be the one that works *when everything else breaks* — or when nothing else belongs there in the first place.