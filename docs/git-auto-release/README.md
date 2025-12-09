# Git Auto Release - Documentation

> **Template Documentation**: This folder contains all Git-Auto-Release documentation. When adopting this template, you can keep, archive, or delete this folder.

Complete documentation for setting up and using automated Git versioning and releases.

> **❓ Have questions?** Check the [FAQ in Reference Guide](REFERENCE_GUIDE.md#-frequently-asked-questions)

---

## 📚 Documentation Index

### Getting Started
- **[Quick Start Guide](QUICKSTART.md)** - Get up and running in 10 minutes
- **[Setup Guide](SETUP_GUIDE.md)** - Complete installation and configuration
- **[Reference Guide](REFERENCE_GUIDE.md)** - Comprehensive reference with daily workflow commands and version cheat sheet (for established projects)
  - **[FAQ](REFERENCE_GUIDE.md#-frequently-asked-questions)** - Common questions and troubleshooting

### Core Concepts
- **[Branch Strategy](BRANCH_STRATEGY.md)** - Detailed branching model and workflows
- **[Workflow Examples](WORKFLOW_EXAMPLES.md)** - Real-world usage scenarios

### Customization
- **[Customization Guide](CUSTOMIZATION.md)** - Adapting the template to your needs (GitLab CI, Jenkins, etc.)
- **[Project Structure](PROJECT_STRUCTURE.md)** - Understanding the repository layout

### Visual References
> **Note**: Visual diagrams are integrated throughout the documentation for contextual learning:
> - **Branch diagrams** in [Branch Strategy](BRANCH_STRATEGY.md)
> - **Workflow flowcharts** in [Workflow Examples](WORKFLOW_EXAMPLES.md)
> - **CI/CD architecture** in [Setup Guide](SETUP_GUIDE.md)
> - **Quick decision tree** in [Reference Guide](REFERENCE_GUIDE.md) (for daily usage)

---

## 🔑 Key Concepts

### Semantic Versioning

Git-Auto-Release follows [Semantic Versioning 2.0.0](https://semver.org/):

- **MAJOR** (X.0.0) - Breaking changes (via `alpha`/`beta` branches)
- **MINOR** (0.X.0) - New features, backwards-compatible (via `feature/*` branches)
- **PATCH** (0.0.X) - Bug fixes (via `bugfix/*` or `hotfix` branches)

**Pre-release tags:**
- `-alpha` - Early breaking changes development
- `-beta` - Feature complete, testing phase
- `-rc.N` - Release candidate (during main → release PR)

**Build metadata:**
- `+SHA` - Commit hash for development builds
- `.N` - Build iteration during beta testing (e.g., `v1.0.0-beta.1`, `.2`, `.3`)

### Branch Model

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