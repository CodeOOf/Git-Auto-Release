# Git-Auto-Release Setup Guide & Checklist

> **📋 Complete setup guide** - Follow the steps and check off items as you go. Delete this file when done.

This guide helps you set up Git-Auto-Release for your project with a complete checklist to track your progress.

---

## 📁 Understanding the Template Files

### Files You'll Customize

| File | Purpose | Action |
|------|---------|--------|
| `README.template.md` | Project README template | ✏️ Copy to `README.md` and customize |
| `CONTRIBUTING.template.md` | Contribution guidelines template | ✏️ Copy to `CONTRIBUTING.md` and customize |
| `LICENSE` | License file | ✏️ Replace with your license |
| `VERSION` | Current version number | ✅ **Keep** - Required for automation |

### Files to Replace

| File | What It Is | Action |
|------|-----------|--------|
| `README.md` | Git-Auto-Release docs | 🗑️ Replace with your README |
| `CONTRIBUTING.md` | Git-Auto-Release contribution guide | 🗑️ Replace with yours |
| `CHECKLIST.md` | This file | 🗑️ Delete when setup complete |

### Template Documentation

Located in `docs/git-auto-release/` - reference material explaining the automation:
- `QUICKSTART.md` - 10-minute setup guide
- `SETUP_GUIDE.md` - Complete instructions
- `BRANCH_STRATEGY.md` - Branching model
- `WORKFLOW_EXAMPLES.md` - Real scenarios
- `CUSTOMIZATION.md` - Platform adaptation
- `QUICK_REFERENCE.md` - Command cheat sheet

**Choose one:**
- ✅ Keep for reference
- 📦 Archive: `mkdir archive; mv docs/git-auto-release archive/`
- 🗑️ Delete: `rm -r docs/git-auto-release/`

---

## 🚀 Setup Steps & Checklist

### Step 1: Initial Project Setup

```bash
# Copy template files to create your own
cp README.template.md README.md
cp CONTRIBUTING.template.md CONTRIBUTING.md

# Windows PowerShell:
Copy-Item README.template.md README.md
Copy-Item CONTRIBUTING.template.md CONTRIBUTING.md

# Set initial version
echo "0.1.0" > VERSION

# Customize README.md and CONTRIBUTING.md for your project
```

**Checklist:**
- [ ] Copied `README.template.md` to `README.md`
- [ ] Copied `CONTRIBUTING.template.md` to `CONTRIBUTING.md`
- [ ] Customized `README.md` with your project details
- [ ] Customized `CONTRIBUTING.md` with your guidelines
- [ ] Set initial `VERSION` to `0.1.0` (or your preferred version)
- [ ] Replaced `LICENSE` with your chosen license

### Step 2: Repository Setup

```bash
# Initialize if needed
git init
git add .
git commit -m "chore: initial commit"

# Push to GitHub/GitLab
git remote add origin https://github.com/yourusername/your-repo.git
git push -u origin main
```

**Checklist:**
- [ ] Repository created and pushed to remote
- [ ] `main` branch exists and pushed

### Step 3: Create Branch Structure

```bash
# Create release branch
git checkout main
git checkout -b release
git push -u origin release

# Create alpha branch (optional - only for breaking changes)
git checkout main
git checkout -b alpha
git push -u origin alpha

# Return to main
git checkout main
```

**Checklist:**
- [ ] `release` branch created and pushed
- [ ] `alpha` branch created and pushed (if needed for breaking changes)

### Step 4: Configure Branch Protection

Navigate to repository settings → Branches → Add branch protection rule

**Main Branch Protection:**
- [ ] Branch protection rule created for `main`
- [ ] "Require pull request before merging" enabled
- [ ] Required approvals: **1**
- [ ] "Require status checks to pass" enabled
- [ ] Status checks: `Build & Test`, `Calculate Version`
- [ ] "Require conversation resolution" enabled

**Release Branch Protection:**
- [ ] Branch protection rule created for `release`
- [ ] "Require pull request before merging" enabled
- [ ] Required approvals: **2**
- [ ] "Require status checks to pass" enabled
- [ ] Status check: `Calculate Version`
- [ ] "Require conversation resolution" enabled

### Step 5: Configure GitHub Actions / CI/CD

**Checklist:**
- [ ] GitHub Actions enabled (Settings → Actions → General)
- [ ] Workflow file exists: `.github/workflows/ci-cd-versioned.yml`
- [ ] Workflow permissions: "Read and write" (Settings → Actions → General)
- [ ] Customized build/test commands for your project
- [ ] Removed unused sections (e.g., Docker if not needed)

### Step 6: Test the Setup

```bash
# Create a test feature
git checkout main
git pull origin main
git checkout -b feature/test-setup

# Make a change
echo "# Test" > test.md
git add test.md
git commit -m "feat: test automated versioning"
git push origin feature/test-setup

# Create PR to main and merge
```

**Checklist:**
- [ ] Created test feature branch
- [ ] Made commit with conventional message (`feat:`, `fix:`, etc.)
- [ ] Pushed feature branch
- [ ] Created PR to `main`
- [ ] GitHub Actions ran successfully
- [ ] Build version displayed (e.g., `v0.1.0+abc123`)
- [ ] Merged PR to `main`
- [ ] Tag created automatically (e.g., `v0.2.0-beta`)
- [ ] `VERSION` file updated automatically

### Step 7: Test Production Release (Optional)

```bash
# Create PR from main to release
git checkout main
git pull origin main

# Create PR: main → release, then merge
```

**Checklist:**
- [ ] Created PR from `main` to `release`
- [ ] CI/CD ran successfully
- [ ] Merged to `release`
- [ ] Production tag created (e.g., `v0.2.0`)
- [ ] GitHub Release created with changelog
- [ ] `VERSION` file shows production version

### Step 8: Clean Up Template Files

```bash
# Remove template files
rm README.template.md CONTRIBUTING.template.md

# Choose what to do with docs/git-auto-release/
# Keep, archive, or delete (see options above)

# Delete this checklist when done
rm CHECKLIST.md
```

**Checklist:**
- [ ] Removed `*.template.md` files
- [ ] Decided what to do with `docs/git-auto-release/`
- [ ] Ready to delete this checklist file

---

## 🎯 Quick Verification Commands

```bash
# Check branches
git branch -a | grep -E "(main|alpha|release)"

# Check VERSION file
cat VERSION

# Check workflow exists
ls .github/workflows/ci-cd-versioned.yml

# Check tags (after first release)
git fetch --tags
git tag -l
```

---

## 🚨 Troubleshooting

**Workflow not running:**
- Check GitHub Actions is enabled (Settings → Actions)
- Verify workflow YAML syntax
- Check workflow permissions

**Version not calculating:**
- Ensure `VERSION` file exists with valid semver (e.g., `0.1.0`)
- Verify branch names match patterns (`feature/*`, `bugfix/*`)
- Review workflow logs

**Tags not created:**
- Ensure workflow completed successfully
- Check workflow has write permissions
- Verify merge was to `main` or `release`

**Branch protection issues:**
- Check rules are configured correctly
- Ensure status check names match workflow job names
- Verify you're not bypassing as admin

See [Complete Troubleshooting Guide](docs/git-auto-release/SETUP_GUIDE.md#7-troubleshooting) for more help.

---

## ✨ Setup Complete!

When all items are checked:
- 🎉 Your automated release system is ready!
- 🚀 Start developing: `git checkout -b feature/your-feature`
- 📦 Releases happen automatically on merge
- ⏱️ Save hours of manual version management

**Delete this file when done:**
```bash
rm CHECKLIST.md
```

---

## 📚 Additional Resources

- 📖 [Documentation](docs/git-auto-release/)
- 🚀 [Quick Start Guide](docs/git-auto-release/QUICKSTART.md)
- 🌳 [Branch Strategy](docs/git-auto-release/BRANCH_STRATEGY.md)
- 📋 [Workflow Examples](docs/git-auto-release/WORKFLOW_EXAMPLES.md)
- 🐛 [GitHub Issues](https://github.com/CodeOOf/Git-Auto-Release/issues)
- 💬 [Discussions](https://github.com/CodeOOf/Git-Auto-Release/discussions)

---
**Print this checklist and check off items as you go! 📋✅**
