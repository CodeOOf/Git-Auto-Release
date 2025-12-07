# Contributing to [Your Project Name]

> **📝 Template File**: Copy this to `CONTRIBUTING.md` and customize for your project.

Thank you for considering contributing to [Your Project Name]!

## Getting Started

### Prerequisites

<!-- List what contributors need installed -->
- Git
- [Language/runtime version]
- [Other dependencies]

### Setup

```bash
# Clone the repository
git clone https://github.com/yourusername/your-project.git
cd your-project

# Install dependencies
[your install commands]
```

## Development Workflow

This project uses **Git-Auto-Release** for automated semantic versioning.

### 1. Create a Branch

Choose the right branch type for your change:

```bash
# New features (MINOR version: 0.X.0)
git checkout main
git pull origin main
git checkout -b feature/your-feature-name

# Bug fixes (PATCH version: 0.0.X)
git checkout main
git pull origin main
git checkout -b bugfix/issue-description

# For breaking changes (MAJOR version: X.0.0) - create from alpha
git checkout alpha  # or create alpha from main if it doesn't exist
git checkout -b feature/breaking-change
```

### 2. Make Changes

```bash
# Make your changes
# Write/update tests
# Ensure all tests pass

git add .
git commit -m "feat: add new capability"
# or
git commit -m "fix: resolve issue with X"
```

**Commit Message Convention:**
- `feat:` New feature
- `fix:` Bug fix
- `docs:` Documentation only
- `test:` Adding tests
- `refactor:` Code restructuring
- `chore:` Maintenance

### 3. Submit Pull Request

```bash
git push origin feature/your-feature-name
```

Create a Pull Request targeting `main` (or `alpha` for breaking changes).

**Automated Versioning:**
- Versions are automatically calculated on merge
- `feature/*` branches bump MINOR version
- `bugfix/*` branches bump PATCH version
- Merges to `alpha` bump MAJOR version

## Code Standards

<!-- Customize these for your project -->

### Style Guide

- Follow [your style guide]
- Run linter before committing
- Format code consistently

### Testing

```bash
# Run tests
[your test command]

# Run with coverage
[your coverage command]
```

**Requirements:**
- All features must have tests
- Maintain minimum X% coverage
- All tests must pass

## Pull Request Checklist

Before submitting:
- [ ] Code follows style guidelines
- [ ] Tests added/updated and passing
- [ ] Documentation updated (if needed)
- [ ] Commit messages follow convention
- [ ] Branch name follows pattern (`feature/*`, `bugfix/*`, etc.)

## Need Help?

- 📖 Read [Git-Auto-Release documentation](docs/git-auto-release/)
- 🌳 Review [Branch Strategy](docs/git-auto-release/BRANCH_STRATEGY.md)
- 📋 See [Workflow Examples](docs/git-auto-release/WORKFLOW_EXAMPLES.md)
- 🐛 Open an [issue](../../issues)

---

**Thank you for contributing! 🙏**

---

## Pull Request Process

1. **Update documentation** if you changed functionality
2. **Test your changes** thoroughly
3. **Create PR** to `alpha` branch
4. **Describe changes** clearly in PR description
5. **Respond to feedback** from reviewers

---

## Testing Your Changes

### Test Workflow Changes

1. Create a test repository
2. Copy your modified workflow
3. Test all scenarios:
   - Feature branches
   - Bugfix branches
   - Major releases
   - Hotfixes
4. Verify version calculations are correct
5. Check tag creation works

### Test Documentation Changes

1. Preview Markdown rendering
2. Verify all links work
3. Check code examples execute correctly
4. Ensure formatting is consistent

---

## Areas That Need Help

- **More language examples**: Go, Rust, Java, etc.
- **Deployment examples**: AWS, Azure, GCP, Kubernetes
- **CI/CD alternatives**: GitLab CI, Jenkins, CircleCI
- **Documentation improvements**: Clearer explanations, more diagrams
- **Bug fixes**: Issues reported by users
- **Testing**: More comprehensive test scenarios

---

## Questions?

- Open an issue for discussion
- Check existing issues and PRs
- Read the documentation thoroughly

---

## Code of Conduct

Be respectful, constructive, and welcoming. We're all here to learn and improve!

---

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

**Thank you for helping make Git-Auto-Release better! 🚀**
