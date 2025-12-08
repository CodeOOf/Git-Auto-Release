# Git-Auto-Release Template - Overview

**A GitHub Actions template for fully automated semantic versioning and release management.**

---

## 🎯 What Is This?

Git-Auto-Release automates your version control workflow using GitHub Actions:

1. **Automatic Version Bumping**: MAJOR/MINOR/PATCH based on branch merges
2. **Semantic Versioning**: Full semver 2.0.0 compliance with pre-release tags
3. **Branch-Based Strategy**: Parallel branching model with clear promotion paths
4. **Zero Maintenance**: No manual VERSION file edits or tag creation
5. **Production Ready**: Includes GitHub Releases, changelogs, and optional Docker support

---

## 📚 Documentation Guide

### Getting Started
- 🚀 **[QUICKSTART.md](QUICKSTART.md)** - Get running in 5 minutes (Option A: GitHub template, Option B: Manual setup)
- 📘 **[SETUP_GUIDE.md](SETUP_GUIDE.md)** - Detailed configuration and advanced options

### Daily Usage (For Established Projects)
- ⚡ **[QUICK_REFERENCE.md](QUICK_REFERENCE.md)** - Command cheat sheet and decision tree
- 📖 **[WORKFLOW_EXAMPLES.md](WORKFLOW_EXAMPLES.md)** - Real-world step-by-step scenarios

### Understanding & Customization
- 🌳 **[BRANCH_STRATEGY.md](BRANCH_STRATEGY.md)** - Parallel branching model explained
- 🎨 **[CUSTOMIZATION.md](CUSTOMIZATION.md)** - Adapt for GitLab, Bitbucket, Jenkins, etc.
- 📂 **[PROJECT_STRUCTURE.md](PROJECT_STRUCTURE.md)** - Template file organization

---

## 📁 What You Get (Template Files)

When you use this template, you receive:

```
Your-Project/
├── .github/workflows/
│   └── ci-cd-versioned.yml          # Automated versioning workflow
├── docs/git-auto-release/           # All template documentation (keep, archive, or remove)
├── README.template.md              # Starter for your project README
├── CONTRIBUTING.template.md        # Starter for your contribution guidelines
├── CHECKLIST.md                    # Implementation checklist
└── VERSION                         # Semantic version tracker (auto-managed)
```

**Note**: Template documentation is in `docs/git-auto-release/` - you can keep it for reference, move to archive, or delete once familiar.

---

## ✨ Key Features

### Automated Versioning
- ✅ **MAJOR bump**: `alpha` → `main` (breaking changes)
- ✅ **MINOR bump**: `feature/*` → `main` (new features)
- ✅ **PATCH bump**: `bugfix/*` → `main` (bug fixes)
- ✅ **Pre-release tags**: `-alpha`, `-beta` suffixes
- ✅ **Build metadata**: `+SHA` for dev builds

### Branch Strategy
- ✅ **Parallel branching**: All branches from `main` (except hotfix from `release`)
- ✅ **Alpha branch**: For breaking changes only
- ✅ **Beta branch**: Auto-created after alpha merge
- ✅ **Release branch**: Production-ready code
- ✅ **Hotfix support**: Emergency production fixes

### CI/CD Automation
- ✅ **Merge commit detection**: Supports both merge and squash merges
- ✅ **VERSION file updates**: Automatic after PR merges
- ✅ **Tag creation**: Auto-tagged with proper semver
- ✅ **GitHub Releases**: Generated with changelogs
- ✅ **Branch syncing**: Keeps branches up-to-date

### Template Design
- ✅ **Placeholder build steps**: Customize for your stack
- ✅ **Commented examples**: Docker, deployment guides
- ✅ **Platform-agnostic**: Works with any language
- ✅ **Documentation included**: All template docs in `docs/git-auto-release/`

---

## 🎨 Version Flow Example

```
feature/login → main
  ├─ Merge: Creates v0.2.0-beta tag
  └─ Result: Ready for staging/testing

main → release (when ready)
  ├─ Merge: Creates v0.2.0 tag + GitHub Release
  └─ Result: Production release
```

---

## 📊 Version Bump Summary

| Action | From | To | Tag Created |
|--------|------|-----|-------------|
| Merge `feature/*` to `main` | 0.1.0 | 0.2.0-beta | v0.2.0-beta |
| Merge `bugfix/*` to `main` | 0.1.0-beta | 0.1.1-beta | v0.1.1-beta |
| Merge `alpha` to `main` | 0.9.0 | 1.0.0-alpha | v1.0.0-alpha |
| Merge `beta` to `main` | 1.0.0-alpha | 1.0.0-beta | v1.0.0-beta |
| Merge `main` to `release` | 0.2.0-beta | 0.2.0 | v0.2.0 + Release |
| Merge `hotfix` to `release` | 1.0.0 | 1.0.1 | v1.0.1 + Release |

---

## 🛠️ Customization Points

### Easy Customizations
- Change placeholder build/test commands for your language
- Remove Docker examples if not needed
- Adjust branch protection rules
- Modify release notes format

### Advanced Customizations
- Adapt workflow for GitLab CI, Bitbucket Pipelines, Jenkins
- Change version bump logic
- Add deployment steps
- Integrate with external tools

See [CUSTOMIZATION.md](CUSTOMIZATION.md) for platform-specific guides.

---

## 🎯 Common Use Cases

Perfect for:
- ✅ **SaaS applications**: Clear staging → production promotion path
- ✅ **Open source projects**: Transparent and predictable releases
- ✅ **Enterprise software**: Controlled, auditable version control
- ✅ **Microservices**: Consistent versioning across services
- ✅ **APIs**: Breaking changes clearly marked with MAJOR bumps
- ✅ **Libraries**: Semantic versioning for package dependents

---
- [Conventional Commits](https://www.conventionalcommits.org/)
- [GitHub Flow](https://docs.github.com/en/get-started/quickstart/github-flow)

---

## 📝 Next Steps

### For This Repository (Template Itself)

1. ✅ **Set up GitHub Actions** - Enable workflows
2. ✅ **Create initial branches** - main, alpha, release
3. ✅ **Add branch protection** - Follow setup guide
4. ✅ **Test the workflow** - Make a test feature

### For Template Users

1. **Use this template** - Click "Use this template" on GitHub
2. **Follow setup guide** - [`docs/SETUP_GUIDE.md`](docs/SETUP_GUIDE.md)
3. **Customize** - Adapt for your project
4. **Start developing** - Create features with automated releases!

---

## 📞 Support

- **Documentation**: All guides in [`docs/`](docs/)
- **Issues**: [GitHub Issues](https://github.com/CodeOOf/Git-Auto-Release/issues)
- **Discussions**: [GitHub Discussions](https://github.com/CodeOOf/Git-Auto-Release/discussions)

---

## 📄 License

This template is released under the **MIT License** - see [LICENSE](LICENSE) for full text.

You are free to:
- ✅ Use this template for commercial or personal projects
- ✅ Modify and adapt to your needs  
- ✅ Distribute and share

**Your project built with this template can use any license you choose.**

---

## 🤝 Contributing & Support

**Contributions welcome!** See [CONTRIBUTING.md](../../CONTRIBUTING.md) for guidelines.

**Need help?**
- 📖 Documentation: Complete guides in [docs/git-auto-release/](.)
- 🐛 Issues: [GitHub Issues](https://github.com/CodeOOf/Git-Auto-Release/issues)
- 💬 Discussions: [GitHub Discussions](https://github.com/CodeOOf/Git-Auto-Release/discussions)

---

**⏱️ Setup Time**: ~5 minutes  
**🔄 Maintenance**: Zero - fully automated!  
**💪 Production Ready**: Use immediately!

