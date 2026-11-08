---
sidebar_position: 2.5
title: "Platform Support"
description: "Which operating systems, distribution methods, and features Seraph Agent 3.0 supports."
---

# Platform Support

Seraph Agent 3.0 publishes source installers for macOS, Windows, Linux, and WSL2, plus an in-tree Nix flake. This fork does not currently publish packaged desktop installers, Docker images, or a Termux APT repository. Upstream Hermes Agent distributions may support additional platforms; those are not Seraph releases.

---

## Tier 1

We strive to never break installations and updates for these. Issues & regressions in Tier 1 are our first priority and take precedence over other platforms.

| OS / Architecture | Installation method | Notes |
| --- | --- | --- |
| **macOS** (Apple Silicon) | [`install.sh`](./installation.md) | Source install; packaged desktop builds are not published by this fork. |
| [**Windows 10 / 11**](../user-guide/windows-native.md) (x86_64, aarch64) | [`install.ps1`](./installation.md) | Source install; packaged MSIX builds are not published by this fork. |
| **Linux / [WSL2](../user-guide/windows-wsl-quickstart.md)** (x86_64, aarch64) | [`install.sh`](./installation.md) | Source install. |

---

## Tier 2

These platforms are maintained in-tree only as a best effort.
Releases may break them, and we can't promise we'll fix them promptly when they break.

PRs will be accepted to fix issues with them, but they will take precedence below fixing issues with Tier 1 platforms.

| OS / Architecture | Installation method | Notes |
| --- | --- | --- |
| **Nix** (macOS, Linux, NixOS) | [Nix & NixOS guide](./nix-setup.md) | The fork's flake provides CLI, desktop, and NixOS packages; best-effort support. |

### Build targets and support priority

The Seraph desktop application is available as source in this repository, but
this fork does not publish desktop installers or automatic-update feeds. Use
the source installer above for a supported command-line installation.

## Unsupported

These platforms and distribution methods are **not currently supported by this fork**.
Upstream Hermes Agent may provide separate releases for some of them. Contributions
that add support to Seraph are welcome.

- Android / Termux (the linked APT guide describes the upstream Hermes Agent package, not a Seraph release)
- installs via the AUR (we might upstream patches if it helps out &lt;3)
- 32-bit x86 macOS, and Intel x86_64 macOS desktop bundles (none are built).
- installs via `pypi` (e.g. `uv tool install hermes-agent`, `pip install hermes-agent`, etc.)
- installs via `brew` (`brew install hermes-agent`)

If you are using an unsupported distribution method, please read the [the installation guide](./installation.md) to learn how to switch to a supported one.
