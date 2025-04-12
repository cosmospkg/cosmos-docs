# 16 – Galaxies

Galaxies are where Stars go to live (or die). A Galaxy in Cosmos is a plain folder that contains:

- A `meta.toml` file with basic info
- A list of available Star packages
- An optional `/packages/` directory full of compressed tarballs
- A `/stars/` directory full of individual `star.toml` files

Galaxies are designed to be as dumb and static as possible—because the smartest systems are the ones that survive a power outage.

---

## 🖊️ Example `meta.toml`
```toml
name = "core"
description = "Core packages for Cosmos systems"
version = "2024.04.17"
[stars]
musl = "1.2.4"
busybox = "1.36.0"
zlib = "1.2.13"
```

Yes, that's all it takes. Cosmos doesn't need a daemon, indexer, or full-time YAML whisperer to read this. Just serve it statically.

---

## 🔀 Example Directory Layout
```
https://mirror.example.org/galaxies/core/
├── meta.toml
├── stars/
│   ├── musl.toml
│   ├── busybox.toml
│   ├── zlib.toml
│   └── cosmos-cli.toml
└── packages/
    ├── musl-1.2.4.tar.gz
    ├── busybox-1.36.0.tar.gz
    └── zlib-1.2.13.tar.gz
```

This can be served from:

- S3
- GitHub Pages
- Your personal server
- A USB stick you found in a drawer from 2008
- **A Git repo you cloned manually** (Cosmos will never do this for you)

---

## 🚫 Rules of Hosting a Galaxy
Managing a Galaxy sounds cool until you realize it means:

- **Maintaining packages** regularly
- **Keeping `meta.toml` in sync** with the actual contents
- **Testing packages** so they don’t destroy someone’s root directory
- **Updating checksums** when you change a tarball
- **Being up 24/7** if others depend on it

You break it? You bought it.

If you're not ready to maintain your own galaxy, don't. Use someone else's. Fork one. Remix one. Or go build Stars and let someone else host them.

---

## 🌌 The Official Galaxy
If Cosmos ever gets an official Galaxy, it will probably be hosted on:

- S3 or a static file server
- With no TLS
- With a massive readme warning you not to blindly trust it

This is because Cosmos believes in:

- Simplicity
- Reproducibility
- You being responsible for your own system

---

## 🌊 Final Notes
Galaxies are boring by design. That makes them reliable. If it can't be served over `python3 -m http.server`, it's probably too complicated.

### 🔍 About Git
If you want to use Git, that’s fine—**clone the Galaxy yourself**. Cosmos doesn't use `git clone`, `libgit2`, or any other dynamic Git transport. Serve the raw files, or keep them local. No Git magic here.

Remember: if Git can host your entire repo, it can probably host your entire Galaxy.

Cosmos doesn't care what you host on—just that it works.
