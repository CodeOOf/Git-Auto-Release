# Git Auto Release

> 📖 **Reading Guide**: [Quick Start](docs/git-auto-release/QUICKSTART.md) → [Setup Guide](docs/git-auto-release/SETUP_GUIDE.md) → [Workflow Examples](docs/git-auto-release/WORKFLOW_EXAMPLES.md) → [Branch Strategy](docs/git-auto-release/BRANCH_STRATEGY.md)

**Automated Git version control and release management template**

A complete, production-ready template that automates semantic versioning, tagging, and releases based on your Git branch strategy. Simply push to branches and let the automation handle version bumps, pre-release tags, and production releases.

**This repository uses GitHub Actions** as a reference implementation. The branching strategy and version logic can be adapted to other CI/CD platforms (GitLab CI, Jenkins, etc.).

[![CI/CD Pipeline](https://github.com/CodeOOf/Git-Auto-Release/actions/workflows/ci-cd-versioned.yml/badge.svg)](https://github.com/CodeOOf/Git-Auto-Release/actions/workflows/ci-cd-versioned.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## 🎯 What Does This Template Do?

Automates your entire release workflow with:

- ✅ **Automatic semantic versioning** based on branch type and merge strategy
- ✅ **Pre-release tags** (`-alpha`, `-beta`, `-rc`) during development
- ✅ **Auto version bumps** (MAJOR/MINOR/PATCH) based on merge source
- ✅ **VERSION file auto-updates** throughout the release lifecycle
- ✅ **Release generation** with changelogs
- ✅ **Branch protection** and pull request workflows
- ✅ **Semantic versioning 2.0.0** compliance

**Perfect for projects that need:**
- Reproducible, automated release processes
- Clear development → staging → production promotion paths
- Traceability between commits, versions, and releases
- Zero manual version management

---

## 📋 Table of Contents

- [Quick Start](#-quick-start)
- [How It Works](#-how-it-works)
- [Branch Strategy](#-branch-strategy)
- [Version Automation](#-version-automation)
- [Setup Instructions](#-setup-instructions)
- [Workflow Examples](#-workflow-examples)
- [Customization Guide](#-customization-guide)
- [FAQ](#-faq)

---

## 🚀 Quick Start

**Ready to get started?** Follow the **[Quick Start Guide](docs/git-auto-release/QUICKSTART.md)** to set up automated versioning in under 10 minutes.

The guide covers:
- Using the template or cloning the repository
- Setting up your VERSION file
- Creating required branches
- Configuring branch protection
- Creating your first versioned pull request

For complete setup instructions, see the **[Setup Guide](docs/git-auto-release/SETUP_GUIDE.md)**.

---

## 🔄 How It Works

### The Branch Flow

```
release (production) ← Tags: v1.0.0, v1.0.1, etc.
  ↑
main (staging) ← Tags: v1.0.0-beta, v0.2.0-beta
  ↑ ↑ ↑ ↑ ↑
  │ │ │ │ └─ hotfix (PATCH, from release)
  │ │ │ └─── bugfix/* (PATCH, from main)
  │ │ └───── feature/* (MINOR, from main)
  │ └─────── beta (MAJOR testing, from main)
  └───────── alpha (MAJOR, from main)

All development branches branch FROM and merge TO main
(except hotfix which is from/to release)
```

### Automated Version Bumping

| Merge | Version Bump | Example | Tag Created |
|-------|--------------|---------|-------------|
| `alpha` → `main` | **MAJOR** | 0.1.0 → 1.0.0-alpha | ✅ v1.0.0-alpha |
| `feature/*` → `main` | **MINOR** | 0.1.0 → 0.2.0-beta | ✅ v0.2.0-beta |
| `bugfix/*` → `main` | **PATCH** | 0.1.0 → 0.1.1-beta | ✅ v0.1.1-beta |
| `hotfix` → `release` | **PATCH** | 0.1.0 → 0.1.1 | ✅ v0.1.1 |
| `main` → `release` | Clean version | 1.0.0-beta → 1.0.0 | ✅ v1.0.0 |

### Build Versions (No Tags)

Pushes to branches create **build versions** with commit SHA:

| Branch | Build Version Example |
|--------|----------------------|
| `alpha` | v0.1.0+a3f2b1c8 |
| `beta` | v1.0.0-alpha+7f82b432 |
| `feature/*` | v0.1.0+c8d92a14 |
| `bugfix/*` | v0.1.0+f14e2c91 |

---

## 🌳 Branch Strategy

This template implements a **four-tier branching model** designed for controlled releases:

### Core Branches

#### `release` - Production
- **Purpose**: Live production releases
- **Protection**: Requires 2 reviewers, all tests passing
- **Merges from**: `main` (promotion) or `hotfix` (emergency)
- **Automation**: Creates clean version tags (`v1.0.0`), GitHub releases

#### `main` - Staging/Release Candidate
- **Purpose**: Production-ready code awaiting final release
- **Protection**: Requires 1 reviewer, all tests passing
- **Merges from**: `alpha`, `beta`, `feature/*`, `bugfix/*`
- **Automation**: Creates beta tags (`v1.0.0-beta`)

#### `beta` - Major Release Testing
- **Purpose**: Stabilize major version bumps (for MAJOR releases)
- **Branch from**: `main`
- **Merge to**: `main` (triggers MAJOR bump)
- **Automation**: Creates beta tags (`v1.0.0-beta`)

#### `alpha` - Major Release Development
- **Purpose**: Develop breaking changes (for MAJOR releases)
- **Branch from**: `main`
- **Merge to**: `main` (triggers MAJOR bump)
- **Automation**: Creates alpha tags (`v1.0.0-alpha`), auto-creates beta branch

### Development Branches

#### `feature/*` - New Features (MINOR Bump)
```bash
git checkout main
git pull origin main
git checkout -b feature/user-authentication
# ... develop feature ...
git push origin feature/user-authentication
# Create PR to main
```

#### `bugfix/*` - Bug Fixes (PATCH Bump or Build Metadata)

**During normal development:**
```bash
git checkout main
git pull origin main
git checkout -b bugfix/fix-login-error
# ... fix bug ...
git push origin bugfix/fix-login-error
# Create PR to main → Creates v0.1.1-beta
```

**During beta/alpha testing** (when VERSION contains `-beta` or `-alpha`):
```bash
git checkout main
git pull origin main
git checkout -b bugfix/beta-fix-validation
# ... fix bug found in testing ...
git push origin bugfix/beta-fix-validation
# Create PR to main → Creates v1.0.0-beta.1 (increments build metadata)
# Next bugfix → v1.0.0-beta.2, then .3, etc.
```

This allows tracking multiple bugfixes during beta testing without changing the base version.

#### `hotfix` - Production Emergencies (PATCH Bump)
```bash
git checkout release
git pull origin release
git checkout -b hotfix
# ... fix critical issue ...
git push origin hotfix
# Create PR to release
```

**Complete details**: See [BRANCH_STRATEGY.md](docs/git-auto-release/BRANCH_STRATEGY.md)

---

## 🤖 Version Automation

### The VERSION File

The `VERSION` file at the repository root is the **single source of truth**. The CI/CD pipeline:

1. **Reads** the current version from `VERSION`
2. **Calculates** the next version based on the merge type
3. **Creates** appropriate tags with pre-release suffixes or build metadata
4. **Updates** `VERSION` automatically after merges
5. **Commits** changes back to the repository

### Example Flow: Major Release

```
Initial: VERSION = 0.1.0

1. Developer merges alpha → main
   → CI creates tag: v1.0.0-alpha
   → CI updates VERSION to: 1.0.0-alpha

2. CI creates beta branch from main

3. Developer merges beta → main (after testing)
   → CI creates tag: v1.0.0-beta
   → CI updates VERSION to: 1.0.0-beta

4. Developer merges main → release
   → PR builds show: v1.0.0-rc.1, v1.0.0-rc.2...
   → CI creates tag: v1.0.0
   → CI updates VERSION to: 1.0.0
   → GitHub Release created
```

### Semantic Versioning Compliance

This template strictly follows [Semantic Versioning 2.0.0](https://semver.org/):

- **MAJOR** (X.0.0): Breaking changes (alpha or beta → main merges)
- **MINOR** (0.X.0): New features (feature/* → main merges)
- **PATCH** (0.0.X): Bug fixes (bugfix/* → main or hotfix → release merges)
- **Pre-release**: -alpha, -beta, -rc.N
- **Build metadata**: .N (for bugfixes during beta/alpha testing), +SHA (commit hash for non-tagged builds)

**Branch Type Determines Version Bump:**
- `alpha` or `beta` branches → MAJOR bump
- `feature/*` branches → MINOR bump
- `bugfix/*` branches → PATCH bump (or build metadata .1, .2... during beta/alpha phase)
- `hotfix` branch → PATCH bump

---

## ⚙️ Setup Instructions

**Need to configure your repository?** See the complete **[Setup Guide](docs/git-auto-release/SETUP_GUIDE.md)** for detailed instructions on:

- Creating and protecting branches (`main`, `release`, `alpha`)
- Configuring branch protection rules
- Setting up CI/CD workflows
- Customizing for your platform (GitHub Actions, GitLab CI, Jenkins)
- Testing your setup

---

## 📝 Workflow Examples

### Example 1: Feature Development

```bash
# Start from main
git checkout main
git pull origin main

# Create feature branch
git checkout -b feature/add-user-profile

# Develop feature
echo "console.log('User profile');" > profile.js
git add profile.js
git commit -m "feat(profile): add user profile page"

# Push and create PR to main
git push origin feature/add-user-profile
```

**On GitHub:**
1. Open PR: `feature/add-user-profile` → `main`
2. CI runs: Build version shows `v0.1.0+abc123`
3. Review and merge
4. After merge: Tag `v0.2.0-beta` created automatically

**To release to production:**
```bash
# Create PR: main → release
# After merge, CI creates tag: v0.2.0 (production)
# VERSION file updated to: 0.2.0
```

### Example 2: Major Release

```bash
# Branch from main for major changes
git checkout main
git pull origin main
git checkout -b alpha

# Develop breaking changes
# ... make changes ...
git commit -m "feat!: redesign API (breaking change)"
git push origin alpha

# Create PR: alpha → main
# After merge:
#   - CI creates tag: v1.0.0-alpha
#   - VERSION updated to: 1.0.0-alpha
#   - beta branch created automatically from main

# Checkout beta for testing
git checkout beta
git pull origin beta

# Fix issues found during testing
git commit -m "fix(api): handle edge cases"
git push origin beta

# Merge beta → main
# After merge:
#   - CI creates tag: v1.0.0-beta
#   - VERSION updated to: 1.0.0-beta

# Final release: main → release
# After merge:
#   - CI creates tag: v1.0.0
#   - VERSION updated to: 1.0.0
#   - GitHub Release created
```

### Example 3: Hotfix

```bash
# Start from release (production)
git checkout release
git pull origin release

# Create hotfix branch
git checkout -b hotfix
echo "// Security fix" >> security.js
git commit -m "fix(security): patch vulnerability CVE-2024-001"
git push origin hotfix

# Create PR: hotfix → release
# During PR: Shows v0.1.1-rc.1, v0.1.1-rc.2...
# After merge:
#   - CI creates tag: v0.1.1
#   - VERSION updated to: 0.1.1
#   - Fast-forwards main with the hotfix
#   - All active branches (alpha, beta, feature/*, bugfix/*) automatically sync with main
```

---

## 🎨 Customization Guide

**Want to adapt this for your project?** See the complete **[Customization Guide](docs/git-auto-release/CUSTOMIZATION.md)** for:

- Adapting to other CI/CD platforms (GitLab CI, Jenkins)
- Removing Docker builds
- Customizing build/test commands
- Modifying release notes format
- Adding deployment steps
- Adjusting version calculation logic
- Branch strategy modifications

---

## ❓ FAQ

### Q: How do I skip CI on commits?
**A:** Add `[skip ci]` to your commit message. The workflow already uses this for VERSION updates.

### Q: What if I manually edit the VERSION file?
**A:** The CI/CD will use your manual version as the base for calculations. Ensure it follows semantic versioning.

### Q: Can I use this with GitLab CI or other platforms?
**A:** Yes! While this template uses GitHub Actions as a reference implementation, the branch strategy and versioning logic can be adapted to GitLab CI, Jenkins, or other CI/CD platforms. See [CUSTOMIZATION.md](docs/git-auto-release/CUSTOMIZATION.md) for guidance.

### Q: How do I handle merge conflicts in VERSION?
**A:** Always accept the more recent version (higher number). The CI/CD will correct it on the next merge.

### Q: Can I customize the pre-release suffixes?
**A:** Yes, edit the suffix strings (`-alpha`, `-beta`, `-rc`) in the workflow YAML file.

### Q: How does beta testing work with bugfixes?
**A:** During beta/alpha testing (when VERSION contains `-beta` or `-alpha`), bugfix merges increment build metadata (`.1`, `.2`, etc.) instead of bumping the patch version. This allows tracking multiple test iterations: `v1.0.0-beta`, `v1.0.0-beta.1`, `v1.0.0-beta.2`. When promoted to production, the clean version is used: `v1.0.0`. See [Workflow Examples](docs/WORKFLOW_EXAMPLES.md#3-beta-testing-with-bugfixes) for details.

### Q: What about monorepos?
**A:** You'll need to adapt the workflow to handle multiple VERSION files or use a more complex versioning strategy.

### Q: How do I rollback a release?
**A:** Delete the tag, revert the merge commit, and follow the normal PR process again.

---

## 📚 Additional Resources

- **[Quick Start Guide](docs/git-auto-release/QUICKSTART.md)** - Get started in 10 minutes
- **[Setup Guide](docs/git-auto-release/SETUP_GUIDE.md)** - Complete setup instructions
- **[Workflow Examples](docs/git-auto-release/WORKFLOW_EXAMPLES.md)** - Real-world usage scenarios
- **[Branch Strategy](docs/git-auto-release/BRANCH_STRATEGY.md)** - Detailed branching model
- **[Customization Guide](docs/git-auto-release/CUSTOMIZATION.md)** - Adapt to your needs
- **[Quick Reference](docs/git-auto-release/QUICK_REFERENCE.md)** - Command cheat sheet
- **Semantic Versioning Spec**: [https://semver.org/](https://semver.org/)

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

Inspired by Git Flow, GitHub Flow, and semantic versioning best practices. Built for teams that want **automated, reproducible, and traceable releases**.

---

**Ready to automate your releases?** Star this repository and start using the template! 🚀
