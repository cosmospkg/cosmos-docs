# 10 – Nova Scripting Engine

Nova is the optional, portable scripting engine for Cosmos. It is designed to replace shell-based `install_script` fields in `star.toml` with a safe, cross-platform alternative powered by Lua.

Nova provides a constrained API surface tailored for packaging use cases: building, copying, and linking files during installation.

Nova scripts are not standalone. They are invoked by Cosmos during install, with the runtime context passed in (e.g. real root path).

---

## 🧑‍💻 Goals
- Replace raw shell scripts with safer, deterministic logic
- Avoid external interpreter dependencies (Nova is embedded)
- Allow packaging logic to be readable and portable
- Make error handling and file safety first-class citizens

---

## ⚖️ Core Principles
- Nova scripts are written in **Lua 5.4+**
- They are run inside a sandboxed Cosmos environment
- They do not have access to arbitrary system utilities (no `os.execute`)
- All filesystem operations are relative to the **real root**, which is passed by Cosmos
- Nova never decides where files go—the package author does

---

## 📄 Basic Script Layout
```lua
function build()
  run({"make"})
end

function install()
  copy("bin/tool", "/usr/bin/tool")
  symlink("/usr/bin/tool", "/usr/bin/t")
end
```

---

## ⚖️ Runtime Environment
Cosmos executes Nova scripts inside a controlled environment.

### Functions Provided:

#### `run(command: array of strings)`
Runs a system command in the current working directory.

- Inherits stdout/stderr
- Returns exit code
- Raises error if non-zero

#### `copy(from: string, to: string)`
Copies a file from `from` (relative to working dir) to `to` (absolute path under real root).

#### `symlink(target: string, linkname: string)`
Creates a symlink from `linkname` to `target`, both rooted in the real root.

#### `mkdir(path: string)`
Creates a directory if it doesn't exist.

#### `chmod(path: string, mode: number)`
Sets file permissions (octal).

#### `exists(path: string)`
Returns true if a file exists under real root.

---

## 🏠 Root Behavior
Cosmos will pass an install root path (e.g. `/`, or `/mnt/wombat-root`) to the Nova runtime. All file operations are resolved **relative to this root**, enforced internally.

This enables:

- Real system installs
- Chroot builds
- Fake-root dry runs for package testing

---

## 🤖 Error Handling
- Any failure in `run`, `copy`, etc. halts the script
- Cosmos will abort the install and clean up temp dirs
- Future: line numbers and error tracing for better debugging

---

## 🌐 Future Features
- `env(key, value)`: set environment variables for future `run()` calls
- `log()`: structured logging support
- `filemap()`: declare expected outputs for validation

Nova gives Cosmos packages a portable, audit-friendly install logic system that works on any environment from Alpine to initramfs—no Bash required.

## ⚠️ Trust & Safety Warning
> Nova is sandboxed in spirit, not at the syscall level. It aims to provide a clean API and scoped file operations, but:

- run() executes real binaries on your system
- Nova does not chroot, isolate namespaces, or drop privileges
- Scripts can still execute destructive commands if written maliciously
- `--safe` flag is an upcoming feature that disables `run()` and other unsafe operations
