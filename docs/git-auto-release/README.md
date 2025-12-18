

📖 **Navigation**: [Quick Start](QUICKSTART.md) | [Setup Guide](SETUP_GUIDE.md) | [Reference Guide](REFERENCE_GUIDE.md) | [Branch Strategy](BRANCH_STRATEGY.md) | [Customization](CUSTOMIZATION.md) | [Project Structure](PROJECT_STRUCTURE.md)

# Git Auto Release - Documentation

## Table of Contents

1. [Getting Started](#getting-started)
2. [Core Concepts](#core-concepts)
3. [Customization](#customization)
4. [Visual References](#visual-references)
5. [Key Concepts](#key-concepts)

---

Welcome to the documentation for Git-Auto-Release, a template for automated semantic versioning and release management using GitHub Actions (or any CI/CD platform).

**Quick Links:**
- **[Quick Start Guide](QUICKSTART.md)** – Get up and running fast
- **[Setup Guide](SETUP_GUIDE.md)** – Full installation and configuration
- **[Reference Guide](REFERENCE_GUIDE.md)** – Daily usage, commands, and FAQ
- **[Branch Strategy](BRANCH_STRATEGY.md)** – Branching model details
- **[Customization Guide](CUSTOMIZATION.md)** – Adapting to your CI/CD
- **[Project Structure](PROJECT_STRUCTURE.md)** – File and folder overview

---

## 🔑 Key Concepts (Summary)

- **Semantic Versioning**: MAJOR (breaking), MINOR (features), PATCH (fixes). See [Reference Guide](REFERENCE_GUIDE.md#key-features).
- **Branch Model**: Parallel development, PRs to `main`, releases from `main` to `release`. See [Branch Strategy](BRANCH_STRATEGY.md).
- **CI/CD Automation**: Version bumping, tagging, and changelogs are fully automated.

For details, see the linked guides above.

### Automated Version Bumping

| Source Branch | Target Branch | Version Bump | Example Tag |
|--------------|---------------|--------------|-------------|
| `alpha` | `main` | MAJOR | v1.0.0-alpha |
| `beta` | `main` | MAJOR | v1.0.0-beta |
| `feature/*` | `main` | MINOR | v0.2.0-beta |
| `bugfix/*` | `main` | PATCH | v0.1.1-beta |
| `main` | `release` | Clean version | v1.0.0 |
| `hotfix` | `release` | PATCH | v1.0.1 |

---

## 🚀 Quick Start

### 1. Set Up Your Project Files

```bash
# Copy template files
cp README.template.md README.md
cp CONTRIBUTING.template.md CONTRIBUTING.md

# Set initial version
echo "0.1.0" > VERSION

# Customize README.md and CONTRIBUTING.md for your project
```

### 2. Create Branch Structure

```bash
# Create release branch
git checkout main
git checkout -b release
git push -u origin release

# Create alpha/beta only if needed for breaking changes
git checkout main
git checkout -b alpha
git push -u origin alpha
```

### 3. Configure Branch Protection

- **main**: Require PR, 1 approval, status checks
- **release**: Require PR, 2 approvals, status checks

See [Setup Guide](SETUP_GUIDE.md) for detailed instructions.

### 4. Test Your Setup

```bash
# Create a feature
git checkout main
git checkout -b feature/test
echo "test" > test.md
git add test.md
git commit -m "feat: test feature"
git push origin feature/test

# Create PR to main, merge, and verify tag created
```

---

## 📖 Managing This Documentation

When adopting this template for your project, choose one option:

### Option 1: Keep for Reference
```bash
# No action needed - leave docs/git-auto-release/ as-is
```

### Option 2: Archive
```bash
# Move to archive folder
mkdir -p archive
mv docs/git-auto-release archive/
```

### Option 3: Remove
```bash
# Delete once familiar with the workflow
rm -r docs/git-auto-release/
```

---

## 🛠️ Platform Compatibility

This template uses **GitHub Actions** as reference but works with:
- **GitLab CI/CD** - See [Customization Guide](CUSTOMIZATION.md)
- **Jenkins** - Adaptable pipeline examples provided
- **Other CI/CD platforms** - Generic version logic included

---

## 📄 License

This template is released under the **MIT License** - see [LICENSE](LICENSE) for full details.

You are free to use, modify, and distribute this template in any project (commercial or personal). **Your projects built with this template can use any license you choose.**

---

## 📞 Need Help?

- 📖 Read the [Setup Guide](SETUP_GUIDE.md)
- 🐛 Check [GitHub Issues](https://github.com/CodeOOf/Git-Auto-Release/issues)
- 💬 Join [Discussions](https://github.com/CodeOOf/Git-Auto-Release/discussions)
- ⚡ Use [Reference Guide](REFERENCE_GUIDE.md) for daily workflow commands

---

**Ready to get started?** → [Quick Start Guide](QUICKSTART.md)