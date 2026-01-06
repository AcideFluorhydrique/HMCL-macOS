# HMCL for macOS

## What is This?

This is a repository for **automatically packaging HMCL on macOS**: it fetches the latest release of `HMCL.jar` from upstream, generates `HMCL.app`, and optionally packages it into a drag-and-drop installable `HMCL.dmg`.

- **Upstream Project**: The main HMCL program comes from the open-source project [HMCL-dev/HMCL](https://github.com/HMCL-dev/HMCL)
- **What This Repository Does**: Only provides macOS packaging and bundling scripts (without modifying the upstream logic), making it easier for users to launch with a double-click or drag to install

> Reminder: This repository does not commit `HMCL.jar` or HMCL source code into the repository; during build, it downloads via GitHub API and caches in `.cache/`.

## Quick Start

Run the following in the root directory of the repository:

```bash
make workflow
```

It will sequentially complete:
- Fetch the latest release via GitHub API and download `HMCL.jar` to `.cache/` (skip if already downloaded)
- Generate `HMCL.icns` from `icon/HMCL.png`
- Create `.output/HMCL.app` (copy jar and icon to `Contents/Resources/`)
- Create `.output/HMCL.dmg` (containing `HMCL.app`, `Applications` symlink, and documentation/license files)

## Output & Cache Directories

- **Output Directory**: `.output/`
  - `HMCL.app`
  - `HMCL.dmg`
  - `HMCL.icns`

- **Cache Directory**: `.cache/`
  - `HMCL-latest.jar`: Fixed entry point (soft link pointing to cached jar filename)
  - `hmcl-latest.json`: Records upstream release info used this time (like `tag_name`, download URLs, etc.)
  - `LICENSE-HMCL.txt`: Upstream license text (downloaded and cached during build)

## Common Commands

- **Only Download JAR (Cache)**:
  ```bash
  make jar
  ```

- **Only Cache Upstream LICENSE**:
  ```bash
  make license
  ```

- **Only Generate icns / app / dmg**:
  ```bash
  make icon
  make app
  make dmg
  ```

- **Install to Local Applications (for testing, backs up existing saves)**:
  ```bash
  make install-app
  ```

## System Dependencies

This repository assumes you are running on macOS and can use built-in system tools:
- `curl` (download)
- `jq` (parse JSON returned by GitHub API)
- `sips` / `iconutil` (generate `.icns`)
- `hdiutil` (generate `.dmg`)
- `make` / `bash`

Among these, `jq` may need to be manually installed (e.g., using Homebrew):

```bash
brew install jq
```

When running `HMCL.app`, the system needs to find `java` (script优先 uses `/usr/libexec/java_home`; otherwise, uses `java` in `PATH`).

## macOS Says "Damaged / Not Secure"

If opening `HMCL.app` prompts "damaged" or "not secure", you can remove the quarantine attribute before opening:

```bash
xattr -rd com.apple.quarantine /Applications/HMCL.app
```

## License & Compliance Statement

- **Scripts/Packaging Files in This Repository**: Use `LICENSE` (MIT License).
- **HMCL (Upstream)**: License terms are subject to [HMCL-dev/HMCL](https://github.com/HMCL-dev/HMCL); this repository includes when packaging DMG:
  - `LICENSE-HMCL.txt` (upstream license text)
  - `SOURCE_CODE.md` (explains how to obtain corresponding source code by `tag_name`)
  - `HMCL-RELEASE.json` (metadata of the release used this time)
  - `INSTALLATION.md` (declaration and installation instructions)

This repository makes no modifications to the upstream program; packaged products are provided "as is" at your own risk.
