# 🚀 Quick Start - Git Auto Release

📖 **Navigation**: [← README](README.md) | **Quick Start** | [Setup Guide →](docs/SETUP_GUIDE.md)

Get up and running with automated versioning in under 10 minutes!

---

## Step 1: Get the Template (1 min)

### Option A: Use Template on GitHub
1. Click **"Use this template"** button
2. Name your repository
3. Click **"Create repository from template"**
4. Clone your new repo

### Option B: Clone Directly
```bash
git clone https://github.com/CodeOOf/Git-Auto-Release.git my-project
cd my-project
rm -rf .git
git init
```

---

## Step 2: Make It Your Own (2 min)

### Reorganize Documentation

```bash
# Create the template documentation folder
mkdir -p docs/git-auto-release

# Move all template documentation
mv README.md docs/git-auto-release/
mv QUICKSTART.md docs/git-auto-release/
mv BRANCH_STRATEGY.md docs/git-auto-release/
mv CONTRIBUTING.md docs/git-auto-release/

# Use the project README template
mv README.template.md README.md

# Note: Keep docs/SETUP_GUIDE.md, docs/WORKFLOW_EXAMPLES.md, etc. in docs/git-auto-release/
# They are already referenced correctly
```

### Update Your README

Edit `README.md` with your project details:
- Project name and description
- Setup instructions
- Documentation links

**The template documentation is now in `docs/git-auto-release/`** - you can keep it, move it, or delete it later.

---

## Step 3: Configure Initial Version (1 min)

```bash
# Set your starting version
echo "0.1.0" > VERSION
git add VERSION
git commit -m "chore: set initial version"
```

---

## Step 3: Create Required Branches (2 min)

```bash
# Create and push main branch
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main

# Create and push release branch
git checkout -b release
git push -u origin release

# Go back to main for development
git checkout main
```

**Note**: Alpha and beta branches are created as needed for major releases. Feature branches are created from main.

---

## Step 4: Set Up Branch Protection (3 min)

Go to **GitHub → Your Repo → Settings → Branches**:

### Protect `main` branch:
1. Click **"Add branch protection rule"**
2. Branch name: `main`
3. Check ✅ **Require pull request before merging**
4. Set **1** required approval
5. Check ✅ **Require status checks to pass**
6. Click **"Create"**

### Protect `release` branch:
1. Click **"Add branch protection rule"** again
2. Branch name: `release`
3. Check ✅ **Require pull request before merging**
4. Set **2** required approvals
5. Click **"Create"**

---

## Step 5: Test Your Setup (3 min)

```bash
# Create a test feature from main
git checkout main
git pull origin main
git checkout -b feature/test-automation

# Make a change
echo "# Test Feature" > test.md
git add test.md
git commit -m "feat(test): verify automated versioning works"

# Push and create PR
git push origin feature/test-automation
```

**On GitHub:**
1. Go to **Pull Requests** tab
2. Click **"New pull request"**
3. Base: `main` ← Compare: `feature/test-automation`
4. Create PR
5. Wait for checks to pass
6. Merge!

---
## Step 6: See the Magic! ✨

### View Build Version
Check the GitHub Actions logs - you'll see version like `v0.1.0+abc123`

### Check Your First Tag
After the merge to main completes:
```bash
git fetch --tags
git tag -l
# You should see: v0.2.0-beta
```

---

## 🎉 Done! You're Ready!

### What You've Accomplished:
- ✅ Set up automated versioning
- ✅ Created proper branch structure
- ✅ Configured branch protection
- ✅ Tested the workflow
- ✅ Created your first version tag

### Next Steps:

**For Daily Development:**
```bash
# Create features from main
git checkout main
git pull origin main
git checkout -b feature/my-feature
git commit -m "feat(module): add feature"
# PR to main → merge → auto-tag (v0.X.0-beta)

# Release features to production
# PR: main → release → auto-tag (v0.X.0) + GitHub Release
``` **Workflow Examples**: [`docs/WORKFLOW_EXAMPLES.md`](docs/WORKFLOW_EXAMPLES.md)
- ⚡ **Quick Reference**: [`docs/QUICK_REFERENCE.md`](docs/QUICK_REFERENCE.md)
- 🎨 **Customization**: [`docs/CUSTOMIZATION.md`](docs/CUSTOMIZATION.md)

---

## Common First Tasks

### Add a Feature
```bash
git checkout alpha
git checkout -b feature/user-login
# ... develop ...
### Add a Feature
```bash
git checkout main
git pull origin main
git checkout -b feature/user-login
# ... develop ...
git commit -m "feat(auth): add user login"
git push origin feature/user-login
# Create PR to main
``` checkout -b bugfix/fix-validation
### Fix a Bug
```bash
git checkout main
git pull origin main
git checkout -b bugfix/fix-validation
# ... fix bug ...
git commit -m "fix(validation): handle empty input"
git push origin bugfix/fix-validation
# Create PR to main
```. Merge features to alpha
### Make a Production Release
```bash
# 1. Develop features on feature/* branches from main
# 2. Merge features to main (creates v0.X.0-beta tags)
# 3. Test on staging
# 4. PR: main → release (creates v0.X.0 tag + GitHub Release)
```
## Customization (Optional)

### For Node.js Projects
Edit `.github/workflows/ci-cd-versioned.yml`:
```yaml
- name: Setup Node
  uses: actions/setup-node@v4
  with:
    node-version: '20'
    
- name: Install
  run: npm ci
  
- name: Build
  run: npm run build
  
- name: Test
  run: npm test
```

### For Python Projects
```yaml
- name: Setup Python
  uses: actions/setup-python@v5
  with:
    python-version: '3.11'
    
- name: Install
  run: pip install -r requirements.txt
  
- name: Test
  run: pytest
```

See [`docs/CUSTOMIZATION.md`](docs/CUSTOMIZATION.md) for more languages!

---

## Need Help?

- 📚 **Read the docs**: [`docs/`](docs/) folder
- ❓ **FAQ**: [`README.md#faq`](README.md#-faq)
- 🐛 **Issues**: [GitHub Issues](https://github.com/CodeOOf/Git-Auto-Release/issues)
- 💬 **Discussions**: [GitHub Discussions](https://github.com/CodeOOf/Git-Auto-Release/discussions)

---

## Version Progression Cheat Sheet

| Action | Version Change | Example |
|--------|---------------|---------|
| Feature merged | 0.1.0 → 0.2.0-beta | feat → alpha → main |
| Bug merged | 0.1.0 → 0.1.1-beta | fix → alpha → main |
| Release to prod | 0.2.0-beta → 0.2.0 | main → release |
| Hotfix | 1.0.0 → 1.0.1 | hotfix → release |
| Major release | 0.9.0 → 1.0.0-alpha | alpha → main |

---

**That's it! Start automating your releases! 🚀**

---

**Time to First Tag**: < 10 minutes  
**Time to Production Release**: < 30 minutes  
**Maintenance Time**: Zero - it's all automated! ✨
