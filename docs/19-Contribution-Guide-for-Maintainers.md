# 19 – Maintaining Galaxies and Stars

This guide is for maintainers of Cosmos Galaxies and package authors who want to build Stars (or Nebulae) that others can install.

---

## 🌟 Your Responsibilities
If you're maintaining a Galaxy or publishing Stars, you are expected to:
- Keep `meta.toml` up to date (names, versions, descriptions, star list)
- Update or rebuild tarballs when package contents change
- Recalculate checksums if hashes are implemented
- Test packages before publishing them (install + uninstall)
- Use SemVer responsibly for Star versioning
- Use `YYYY.MM.DD` format for Galaxy versions in `meta.toml`
- Consider archiving versions of your Galaxy for long-term reproducibility (3–6 months minimum is recommended)

If that sounds like a lot of work—it is. Cosmos assumes you take ownership seriously.

---

## ⚙️ Creating a Star
Use `stellar new-star <name>` to scaffold a new package:
- Edit `star.toml` to define name, version, and dependencies
- Add an `install.sh` or `nova.lua` script
- Place files in a `files/` directory relative to the star

Then:
```bash
stellar build-star ./your-package
```
This creates a `.tar.gz` that installs under Cosmos.

---

## ✨ Making a Nebula
A Nebula is a dependency-only Star:
```toml
name = "core-stack"
type = "nebula"
version = "1.0.0"

[dependencies]
musl = "^1.2.4"
busybox = "^1.36"
```
Nebulae should have no `source`, no `install_script`, and no `files/`.

---

## 📄 Structuring a Galaxy
```toml
# meta.toml
name = "core"
description = "Minimal bootable stack"
version = "2024.04.17"
stars = ["musl", "busybox", "zlib"]
```

```
core-galaxy/
├── meta.toml
├── stars/
│   ├── musl.toml
│   ├── busybox.toml
│   └── zlib.toml
├── packages/
│   ├── musl-1.2.4.tar.gz
│   └── busybox-1.36.0.tar.gz
```

You can host this via:
- S3 (static file bucket)
- GitHub Pages
- Any static HTTP server
- A USB stick you left plugged in since last fall

---

## ❌ Common Mistakes
- Forgetting to bump versions after a change
- Publishing a tarball with the wrong contents
- Mismatching star name vs filename
- Forgetting to update `meta.toml`
- Publishing a Galaxy without testing anything

Cosmos won’t stop you. But it will laugh quietly when your Galaxy breaks.

---

## 🚀 Release Checklist
- [ ] Does every Star have a valid `star.toml`?
- [ ] Are the tarballs named correctly?
- [ ] Is `meta.toml` version bumped?
- [ ] Are dependencies valid?
- [ ] Can everything install cleanly with no internet?
- [ ] Has this Galaxy been archived or tagged in Git for historical reference?

If yes: congratulations. You made a Galaxy.

If no: go fix it before someone files a bug at 3AM.

---

## 🤖 Publishing and Hosting Tips
- Use Git tags for Galaxy versions (`core-2024.04.17`)
- Pin Galaxy metadata to specific Star versions
- Use symlinks for "latest"
- Mirror Galaxies to USB, IPFS, Git, or whatever works for your threat model

---

Maintaining Galaxies is an act of service. Do it well, or don’t do it at all.
