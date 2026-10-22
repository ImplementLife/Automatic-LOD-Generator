# LOD Generator

Fork version: `2.2.2-fork.1`

[![Unity](https://img.shields.io/badge/Unity-2021.3%2B-black.svg)](https://unity.com/)
[![License](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](LICENSE)

A fork of the original Auto LOD Generator, extended with custom LOD workflow improvements for multi-mesh and per-submesh control.

<img src=".github/screenshot.png" width="100%">

## About this fork

This project keeps the original GPLv3 codebase and adds custom workflow-focused improvements for personal and project-specific development.

Key fork additions:
- per-submesh / per-mesh LOD customization support
- editor-side controls for custom LOD tuning
- reduced upstream automation noise for a cleaner personal fork workflow

This repo is maintained by ImplementLife and is based on the original work by the upstream project author, manaporkun.

## Installation

### Unity Package Manager

1. Open Window > Package Manager
2. Click + > Add package from disk
3. Select the repository root folder containing package.json

### Dependency

This package expects the UnityMeshSimplifier dependency to be available in the project. If it is not installed automatically, install it manually from the upstream dependency source.

## Usage

1. Open Tools > Auto LOD Generator
2. Select a mesh-bearing object
3. Choose a preset or adjust LOD values
4. Generate the LOD group

### Custom fork feature

When a mesh has multiple submeshes, you can enable per-submesh tuning in the editor and configure LOD quality independently for each section.

## Features

- static mesh LOD generation
- skinned mesh support
- composite model support
- custom preset support
- batch processing
- asset save support
- custom per-submesh LOD tuning in this fork

## License and attribution

This project remains under GPLv3.

- Maintainer / fork: ImplementLife
- Based on original work: manaporkun / Auto LOD Generator

See [LICENSE](LICENSE) for the full text.

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history and release notes.

---

## License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for details.

---

## Credits

- Built on [UnityMeshSimplifier](https://github.com/Whinarn/UnityMeshSimplifier/) by Mattias Edlund
