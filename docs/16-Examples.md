# 16 – Examples and Usage

This document provides step-by-step examples of how to use Cosmos in common real-world workflows. If you're the kind of person who skips all the theory and just wants to see what happens when you hit "go," this one's for you.

Cosmos won't hold your hand, but it will hand you a static binary and nod respectfully.

---

## 🔧 Installing a Single Star
```bash
cosmos install zlib
```
- Resolves dependencies
- Downloads tarball from synced Galaxy
- Runs install script or Nova (if defined)
- Updates `universe.toml`

It's like `apt`, except if it breaks, you can still uninstall it without praying to the Python gods.

---

## ✨ Installing a Nebula
```bash
cosmos install dev-base
```
Where `dev-base.star.toml`:
```toml
name = "dev-base"
type = "nebula"

[dependencies]
zlib = ">=1.2"
busybox = "^1.36"
gcc = "^13"
```
Installs all dependencies, installs nothing directly. It exists purely to summon other Stars.

---

## 🚀 Installing a Constellation
```bash
cosmos install --constellation ./constellations/desktop.toml
```
Contents:
```toml
name = "desktop"
description = "Desktop environment"
members = [
  "xorg",
  "firefox",
  "alacritty"
]
```
Installs listed Stars in order. Skips any already installed. No fluff.

---

## 🤺 Building a Star with Stellar
```bash
stellar new-star hello-world
# edit files/star.toml and add files/
stellar build-star ./hello-world
```
Creates `./dist/hello-world-1.0.0.tar.gz`. If it breaks, check your paths. And your soul.

---

## 🤔 Nova Script Example
```lua
function build()
  run("make")
end

function install()
  copy("bin/tool", "/usr/bin/tool")
  symlink("/usr/bin/tool", "/usr/bin/t")
end
```
Nova doesn’t care about your shell aliases. It just wants to copy files and go home.

---

## 🤖 Offline Install from USB
```bash
# Sync Galaxy from USB
cosmos sync --from file:///mnt/usb/galaxies/core

# Install normally from cache
cosmos install busybox
```
No internet. No Python. No excuses.

---

These examples show how Cosmos can be used both interactively and in scripting or automation workflows. Its minimalism enables usage in embedded systems, recovery shells, or fully custom distros. And when the world ends, you'll still be able to install zlib.

