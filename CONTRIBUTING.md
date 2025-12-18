



# Contributing to Git Auto Release

Thank you for your interest! We welcome:

- Bug reports and feature requests (open an issue)
- Pull requests for code, docs, or workflow improvements

---

## Testing Requirement

**Before submitting a merge request (pull request) to `main` from your fork, you must follow and complete the steps in [TESTING_PLAN.md](./TESTING_PLAN.md) to verify all core functionality and workflows. This ensures your contribution is reliable and meets project standards.**
# Contributing to Git Auto Release

📖 **Navigation**: [← README](README.md) | **Contributing**

Thank you for your interest in improving Git Auto Release! This template helps teams automate their version control and release processes.

## Ways to Contribute

- 🐛 Report bugs
- 💡 Suggest features or improvements
- 📝 Improve documentation
- 🔧 Submit bug fixes
- ✨ Add new examples
- 🎨 Improve workflow logic

---

## Getting Started

### Fork and Clone

```bash
git clone https://github.com/YOUR_USERNAME/Git-Auto-Release.git
cd Git-Auto-Release
```

### Create a Branch

```bash
# For new features or improvements (branch from main)
git checkout main
git pull origin main
git checkout -b feature/your-improvement

# For bug fixes (branch from main)
git checkout main
git pull origin main
git checkout -b bugfix/your-fix
```

---

## Development Guidelines

### For Documentation Changes

1. Update the relevant `.md` file
2. Ensure links work correctly
3. Keep formatting consistent
4. Test any code examples

### For Workflow Changes

1. Test in a separate test repository first
2. Document the change in `docs/CUSTOMIZATION.md`
3. Update version calculation logic carefully
4. Provide before/after examples

### For Example Files

1. Add examples directly to relevant documentation files
2. Ensure examples are clear and well-commented
3. Test the example in a real project

---

## Commit Guidelines

Follow conventional commits:

```bash
feat(docs): add example for Go projects
fix(workflow): correct version calculation for hotfix branches
docs(readme): clarify setup instructions
```

Types:
- `feat` - New feature or improvement
- `fix` - Bug fix
- `docs` - Documentation only
- `refactor` - Code restructuring
- `test` - Testing improvements
- `chore` - Maintenance

---

## Pull Request Process

1. **Update documentation** if you changed functionality
2. **Test your changes** thoroughly
3. **Create PR** to `main` branch
4. **Describe changes** clearly in PR description
5. **Respond to feedback** from reviewers

**Note**: PRs should target `main` branch. The automation will handle version bumps based on your branch name (`feature/*` or `bugfix/*`).

---

## Testing Your Changes

### Test Workflow Changes

Before submitting workflow changes, thoroughly test them using a private test repository. See the [Testing Guide](#testing-guide) below for detailed instructions.

### Test Documentation Changes

1. Preview Markdown rendering
2. Verify all links work
3. Check code examples execute correctly
4. Ensure formatting is consistent

---

## Testing Guide

### Setting Up a Test Repository

To safely test workflow changes without affecting the main repository, create a private test repository from this template:

#### 1. Create Test Repository from Template

```bash
# Option A: Using GitHub CLI
gh repo create your-username/test-git-auto-release --template CodeOOf/Git-Auto-Release --private --clone

# Option B: Via GitHub Web UI
# 1. Go to https://github.com/CodeOOf/Git-Auto-Release
# 2. Click "Use this template" → "Create a new repository"
# 3. Name it "test-git-auto-release"
# 4. Set visibility to "Private"
# 5. Click "Create repository"
# 6. Clone your new repository locally
```

#### 2. Initialize Test Repository

```bash
cd test-git-auto-release

# Set initial VERSION
echo "0.1.0" > VERSION

# Commit and push
git add VERSION
git commit -m "chore: initialize VERSION file"
git push origin main

# Set up branch protection for main (optional but recommended)
# Go to Settings → Branches → Add branch protection rule
# - Require pull request reviews before merging
# - Require status checks to pass before merging
```

#### 3. Copy Your Modified Workflows

If you're testing workflow changes from your development branch:

```bash
# Copy modified workflow from your dev branch
cp /path/to/Git-Auto-Release/.github/workflows/ci-cd-versioned.yml .github/workflows/

# Commit and push
git add .github/workflows/ci-cd-versioned.yml
git commit -m "test: update workflow for testing"
git push origin main
```

---

### Comprehensive Test Scenarios

Follow these test scenarios in order to verify all functionality. Each scenario includes expected results.

#### Test 1: Feature Branch (Normal Development)

**Scenario**: Add a new feature during normal development

```bash
# Starting VERSION: 0.1.0

# Create feature branch
git checkout main
git pull origin main
git checkout -b feature/test-feature

# Make changes
echo "# Feature Test" > feature.md
git add feature.md
git commit -m "feat: add test feature"
git push origin feature/test-feature
```

**Expected Results**:
- ✅ Push to `feature/test-feature`: Version shows `v0.1.0+<SHA>` (build version)
- ✅ Create PR to main: Version shows `v0.1.0+<SHA>` (build version)
- ✅ Merge PR: 
  - VERSION file updates to `0.2.0-beta`
  - Creates tag `v0.2.0-beta` (after VERSION commit)


# Contributing to Git Auto Release

Thank you for your interest! We welcome:

- Bug reports and feature requests (open an issue)
- Pull requests for code, docs, or workflow improvements

---

## How to Contribute

1. **Fork and clone** this repo
2. **Create a branch**: `feature/your-feature` or `bugfix/your-fix`
3. **Make your changes** (code, docs, or workflows)
4. **Follow commit style**: `type(scope): message` (e.g. `feat(ci): add new job`)
5. **Open a PR to `main`**
6. Respond to feedback and update as needed

---

## PR & Commit Guidelines

- Use clear, conventional commit messages: `feat:`, `fix:`, `docs:`, `chore:`, etc.
- Keep PRs focused and well-described
- Update docs/tests if you change functionality

---

## Branch & Workflow Rules

- All changes go through PRs to `main` (no direct commits)
- Use `feature/*`, `bugfix/*`, `hotfix`, `alpha`, `beta`, `release` as appropriate
- The automation will handle version bumps and tags

---

## Code of Conduct

Be respectful, constructive, and welcoming.

---

## License

By contributing, you agree your work is MIT licensed.

---

**Thanks for making Git-Auto-Release better! 🚀**
- ✅ Push to `alpha`: Version shows `v0.2.1-beta+<SHA>` (build version)
