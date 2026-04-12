# updateall

```text
 _   _ ____  ____    _  _____ _____    _    _     _     
| | | |  _ \|  _ \  / \|_   _| ____|  / \  | |   | |    
| | | | |_) | | | |/ _ \ | | |  _|   / _ \ | |   | |    
| |_| |  __/| |_| / ___ \| | | |___ / ___ \| |___| |___ 
 \___/|_|   |____/_/   \_\_| |_____/_/   \_\_____|_____|
```

Update scripts for keeping machines current across Linux and macOS, with optional Nix and Multipass support.

## Repository Contents

- `updateall`: Debian and Ubuntu update script (stable)
- `updateall_macos_experimental`: macOS update script (experimental)
- `LICENSE`
- `README.md`

## Script Overview

### `updateall` (Debian/Ubuntu)

This script runs with resilient error handling and continues even if a step fails.

It performs:

1. APT maintenance:
- `apt-get update`
- `apt-get upgrade -y`
- `apt-get full-upgrade -y`
- `apt-get autoremove -y`
2. Optional kernel cleanup when available:
- `purge-old-kernels -y`
3. Optional Snap update:
- `snap refresh`
4. Optional Flatpak update:
- `flatpak update -y`
5. Optional Nix updates (if installed):
- `nix-channel --update` (when available)
- `nix profile upgrade --all` (when available)
- `nix-env -u '*'` (when available)
- `nix-collect-garbage -d`
6. Optional Multipass updates (if installed):
- Discovers all containers automatically
- Runs `updateall` inside each container
- Restores each container to its original state (running or stopped)

### `updateall_macos_experimental` (macOS)

Status: **Experimental**

This script is functional but still evolving.

It performs:

1. macOS system updates:
- `softwareupdate -i -a`
2. Optional Homebrew maintenance:
- `brew update`
- `brew upgrade`
- `brew upgrade --cask`
- `brew autoremove`
- `brew cleanup`
3. Optional Mac App Store updates:
- `mas upgrade`
4. Optional Nix updates (if installed):
- `nix-channel --update` (when available)
- `nix profile upgrade --all` (when available)
- `nix-env -u '*'` (when available)
- `nix-collect-garbage -d`
5. Optional Multipass updates (if installed):
- Discovers all containers automatically
- Runs `updateall` inside each container
- Restores each container to its original state (running or stopped)

## Usage

Run from this repository directory.

### Debian/Ubuntu

```bash
chmod +x updateall
./updateall
```

### macOS (experimental)

```bash
chmod +x updateall_macos_experimental
./updateall_macos_experimental
```

## Notes

- Run scripts as a normal user. `sudo` will be used when required.
- If `nix`, `flatpak`, `snap`, `mas`, or `multipass` are not installed, those sections are skipped.
- Multipass updates assume each container has an `updateall` command available in its PATH.
