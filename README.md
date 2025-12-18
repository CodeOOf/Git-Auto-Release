
# Git Auto Release

[![CI/CD Pipeline](https://github.com/CodeOOf/Git-Auto-Release/actions/workflows/ci-cd-versioned.yml/badge.svg)](https://github.com/CodeOOf/Git-Auto-Release/actions/workflows/ci-cd-versioned.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## ✨ Introduction

Welcome to **Git Auto Release** – your team's new best friend for automated, reliable, and hassle-free software releases!

Are you tired of manual version bumps, forgotten tags, and release chaos? This project brings order and joy to your Git workflow by automating semantic versioning, tagging, and changelog generation. Whether you're a solo developer or a fast-moving team, Git Auto Release helps you:

- Ship features and fixes with confidence
- Keep your release history clean and traceable
- Focus on building, not on release busywork

Perfect for open source projects, startups, and any team that wants to spend less time on release management and more time coding. If you love automation, transparency, and best practices, you're in the right place!

---

> **Note:** Changelog generation is not yet implemented, but it's on the roadmap for future releases.

---

**Automated semantic versioning and release management for Git projects.**

---

## 🛣️ Roadmap

- Changelog generation (planned)
- More CI/CD integrations
- Customizable versioning rules

## 🚀 Quick Start

1. **Fork or use this template**
2. Edit the `VERSION` file (start with `0.1.0`)
3. Push to branches: `feature/*`, `bugfix/*`, `alpha`, `beta`, `main`, `release`
4. Let CI/CD handle version bumps, tags, and releases

See [Quick Start Guide](docs/git-auto-release/QUICKSTART.md) for details.

---

## 🔑 Key Features

- Automatic version bumping (MAJOR/MINOR/PATCH)
- Pre-release tags: `-alpha`, `-beta`, `-rc`
- VERSION file auto-managed
- Release notes and changelogs
- Works with GitHub Actions (or adapt for any CI/CD)

---

## 🌳 Branching Model (Summary)

- `main`: Staging, all features/bugfixes merge here
- `release`: Production, only from `main` or `hotfix`
- `alpha`/`beta`: Major changes/testing
- `feature/*`, `bugfix/*`: Normal development

See [Branch Strategy](docs/git-auto-release/BRANCH_STRATEGY.md) for full details.

---

## 📚 Documentation

- [Quick Start](docs/git-auto-release/QUICKSTART.md)
- [Setup Guide](docs/git-auto-release/SETUP_GUIDE.md)
- [Reference Guide](docs/git-auto-release/REFERENCE_GUIDE.md)
- [Branch Strategy](docs/git-auto-release/BRANCH_STRATEGY.md)
- [Customization](docs/git-auto-release/CUSTOMIZATION.md)

---

## ❓ FAQ

See the [FAQ in the documentation](docs/git-auto-release/FAQ.md) for answers to common questions and troubleshooting tips.

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

Inspired by Git Flow, GitHub Flow, and semantic versioning best practices. Built for teams that want **automated, reproducible, and traceable releases**.

---

**Ready to automate your releases?** Star this repository and start using the template! 🚀
