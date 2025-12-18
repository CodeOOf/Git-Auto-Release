
📖 **Navigation**: [README](README.md) | **Quick Start** | [Setup Guide →](SETUP_GUIDE.md)

# 🚀 Quick Start - Git Auto Release

## Table of Contents

1. [Step 1: Get the Template](#step-1-get-the-template-1-min)
2. [Step 2: Configure Your Repo](#step-2-configure-your-repo-1-min)
3. [Step 3: Set Up CI/CD](#step-3-set-up-cicd-2-min)
4. [Step 4: Try a Release](#step-4-try-a-release-1-min)
5. [FAQ](#faq)

---

Get automated versioning running in **under 5 minutes**! For full details, see the [Setup Guide](SETUP_GUIDE.md).

## Step 1: Get the Template (1 min)

### Option A: Use Template on GitHub (Recommended)

1. Click **"Use this template"** on GitHub
2. Name your repository
3. Create repository
4. Clone it:

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
cd YOUR_REPO
```

### Option B: Clone and Customize

```bash
# Clone with your project name
git clone https://github.com/CodeOOf/Git-Auto-Release.git my-project
cd my-project

# Remove Git history
rm -rf .git

# Set initial version
echo "0.1.0" > VERSION

# Replace template files with your own
rm README.md CONTRIBUTING.md
mv README.template.md README.md
mv CONTRIBUTING.template.md CONTRIBUTING.md

# Edit README.md and CONTRIBUTING.md with your project details

# Initialize new Git repo
git init
git checkout -b main
git add .
git commit -m "chore: initialize project from Git-Auto-Release template"

# Add your remote and push
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main

# Create release branch
git checkout -b release
git push -u origin release
git checkout main
```

---

## Step 2: Branch Protection (2 min)

**GitHub → Settings → Branches → Add rule:**

**`main` branch:**
- ✅ Require pull request
- ✅ Require 1 approval
- ✅ Require status checks

**`release` branch:**
- ✅ Require pull request  
- ✅ Require 2 approvals

---

## Step 3: Test It (2 min)

```bash
# Create feature
git checkout -b feature/test-automation
echo "# Test" > test.md
git add test.md
git commit -m "feat: add test feature"
git push origin feature/test-automation
```

**On GitHub:**
- Create PR: `feature/test-automation` → `main`
- Merge it

**Result:**
- VERSION updates to `0.2.0-beta`
- Tag `v0.2.0-beta` created automatically

```bash
# Verify
git checkout main
git pull
cat VERSION        # Shows: 0.2.0-beta
git tag -l         # Shows: v0.2.0-beta
```

---

## 🎉 Done! You're Automated!

### What Happens Automatically:

| Action | Result |
|--------|--------|
| `feature/*` → `main` | MINOR bump: `v0.2.0-beta` |
| `bugfix/*` → `main` | PATCH bump: `v0.1.1-beta` |
| `main` → `release` | Release: `v0.2.0` + GitHub Release |

---

## Daily Usage

### Add Feature
```bash
git checkout main && git pull
git checkout -b feature/my-feature
# ... code ...
git commit -m "feat(scope): description"
git push origin feature/my-feature
# Create PR → main → merge → auto-tagged!
```

### Fix Bug
```bash
git checkout main && git pull
git checkout -b bugfix/fix-issue
# ... fix ...
git commit -m "fix(scope): description"
git push origin bugfix/fix-issue
# Create PR → main → merge → auto-tagged!
```

### Release Production
```bash
# Create PR: main → release
# Merge → Creates v0.2.0 tag + GitHub Release
```

---

## Commit Format

Use structured commit messages for clarity:

```
<type>(scope): <description>
```

**Example**: `feat(auth): add OAuth2 support`

> 📖 **For detailed commit guidelines, types, and examples**, see [Reference Guide - Commit Message Format](REFERENCE_GUIDE.md#commit-message-format)

---

## Next Steps

- ⚡ [Reference Guide](REFERENCE_GUIDE.md) - Daily workflow commands and FAQ
- 📚 [Branch Strategy](BRANCH_STRATEGY.md) - Detailed branching model
- 🎨 [Customization](CUSTOMIZATION.md) - Adapt to your stack
- 📋 [Workflow Examples](WORKFLOW_EXAMPLES.md) - Real scenarios

---

**⏱️ Time to First Tag**: ~5 minutes  
**🔄 Maintenance**: Zero - fully automated! ✨
---

## Table of Contents

1. [Step 1: Get the Template](#step-1-get-the-template-1-min)
2. [Step 2: Configure Your Repo](#step-2-configure-your-repo-1-min)
3. [Step 3: Set Up CI/CD](#step-3-set-up-cicd-2-min)
4. [Step 4: Try a Release](#step-4-try-a-release-1-min)
5. [FAQ](#faq)
