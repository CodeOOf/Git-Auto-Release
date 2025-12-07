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
