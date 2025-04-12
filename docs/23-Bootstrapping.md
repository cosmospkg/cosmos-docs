# 23 – Bootstrapping a System with Cosmos

This guide walks through how to build a working root filesystem using Cosmos alone. It assumes you have a Cosmos binary, a Galaxy source (HTTP, file, or USB), and a directory to install into.

---

## 🌜 What Is Bootstrapping?
Bootstrapping is the act of building a usable system from near-zero. With Cosmos, this means:

- No shell
- No package manager
- No userland
- Just a tarball or static binary and a goal

Cosmos enables you to go from "nothing but libc" to a minimal, functional user environment.

---

## ✅ Requirements
- Statically compiled `cosmos` binary (no interpreter or dynamic libs)
- Galaxy source (USB, HTTP, Git clone, etc.)
- Writable mount point (e.g. `/mnt/wombat`)

---

## 🔨 Install Process
```bash
# Optional: create target root
mkdir -p /mnt/wombat

# somewhere earlier, you add a local/mounted Galaxy

# Install base system
cosmos install --root /mnt/wombat core-stack

# Bind system dirs for chroot
mount --bind /dev /mnt/wombat/dev
mount --bind /proc /mnt/wombat/proc
mount --bind /sys /mnt/wombat/sys

# Optional: copy /etc/passwd or minimal shadow configs
cp -r /etc/skel /mnt/wombat/etc/

# Chroot in
chroot /mnt/wombat /bin/sh
```

---

## 🔄 What to Install First
```bash
cosmos install core-stack
cosmos install net-tools
cosmos install busybox
```

A full Constellation file (e.g. `bootstrap.toml`) can define this list.

---

## 🌐 Use Cases
- Minimal Linux from scratch builds
- Custom distro bootstrapping
- Initramfs builds
- Embedded rescue environments

---

## 🚫 What Cosmos Does *Not* Do
- Partition drives
- Configure bootloaders
- Handle systemd or runit setup

Cosmos installs files and packages—nothing more. It is your tool for building a system, not for booting one.

---

## 🫰 Pro Tips
- Test bootstrap flows inside a container or VM before bare metal
- Keep a Galaxy synced to USB in case your network goes down
- Use a Nebula to define your base stack for repeatable installs
- Use `cosmos freeze` (future) to snapshot what you just installed

---

With Cosmos, you can go from a blank directory to a bootable rootfs with only a few commands—no interpreters, no curl, no Python, no tears.

