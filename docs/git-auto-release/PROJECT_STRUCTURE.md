# Git-Auto-Release Project Structure

```
Git-Auto-Release/
├── .github/
│   └── workflows/
│       └── ci-cd-versioned.yml    # Main CI/CD workflow
├── docs/
│   ├── README.md                  # Documentation index
│   ├── SETUP_GUIDE.md            # Complete setup instructions
│   ├── WORKFLOW_EXAMPLES.md      # Real-world usage examples
│   ├── CUSTOMIZATION.md          # How to customize the template
│   └── QUICK_REFERENCE.md        # Daily usage cheat sheet
├── BRANCH_STRATEGY.md            # Detailed branch strategy
├── LICENSE                       # MIT License
├── README.md                     # Main project documentation
└── VERSION                       # Current version (e.g., 0.1.0)
```

---

## File Descriptions

### Root Files

#### `VERSION`
- **Purpose**: Single source of truth for the current version
- **Format**: Semantic version (e.g., `0.1.0`, `1.0.0-alpha`, `1.0.0-beta`)
- **Managed by**: CI/CD (auto-updated after merges)
- **Usage**: Read by workflow to calculate next version

#### `README.md`
- **Purpose**: Main project documentation
- **Contents**:
  - What the template does
  - Quick start guide
  - How it works
  - Branch strategy overview
  - Setup instructions
  - FAQ

#### `BRANCH_STRATEGY.md`
- **Purpose**: Detailed branch strategy documentation
- **Contents**:
  - Branch types and purposes
  - Version bump logic
  - Release flows (major, minor, patch, hotfix)
  - Mermaid diagrams
  - Commit message conventions
  - Workflow details

#### `LICENSE`
- **Purpose**: MIT License
- **Usage**: Defines project licensing terms

---

### `.github/workflows/`

#### `ci-cd-versioned.yml`
- **Purpose**: Main CI/CD workflow
- **Triggers**:
  - Push to any branch
  - Pull requests to `main`, `alpha`, `release`
- **Jobs**:
  1. `version` - Calculate semantic version
  2. `build` - Build and test application
  3. `docker` - Build and publish Docker images (optional)
  4. `release` - Create GitHub release
  5. `update-version` - Update VERSION file
  6. `summary` - Display build summary

**Key Features**:
- Automatic version calculation
- Branch-based version bumping
- Tag creation
- Release generation
- VERSION file management

---

### `docs/`

#### `README.md`
- **Purpose**: Documentation index
- **Contents**:
  - Links to all documentation
  - Key concepts summary
  - Common tasks
  - Troubleshooting

#### `SETUP_GUIDE.md`
- **Purpose**: Complete setup instructions
- **Contents**:
  - Initial repository setup
  - Branch protection configuration
  - GitHub Actions secrets
  - Workflow customization
  - Testing the setup
  - Troubleshooting

#### `WORKFLOW_EXAMPLES.md`
- **Purpose**: Real-world usage scenarios
- **Contents**:
  - Feature development
  - Bug fixes
  - Major releases
  - Minor releases
  - Patch releases
  - Hotfixes
  - Parallel development
  - Rollback procedures

#### `CUSTOMIZATION.md`
- **Purpose**: Guide for adapting the template
- **Contents**:
  - Workflow modifications
  - Version calculation logic
  - Build/test command changes
  - Docker customization
  - Release notes formatting
  - Deployment integration
  - Branch strategy adjustments

#### `QUICK_REFERENCE.md`
- **Purpose**: Daily usage command cheat sheet (for established projects)
- **Contents**:
  - Decision tree for daily tasks
  - Workflow commands (feature, bugfix, hotfix, release)
  - Version progression tables
  - Commit message examples
  - Git command reference
  - Troubleshooting quick fixes
- **Note**: For initial setup, see `QUICKSTART.md`

---

#### `Dockerfile`
- **Purpose**: Example Docker configuration
- **Usage**: Copy and customize for your application

#### `.gitignore`
- **Purpose**: Recommended Git ignore patterns
- **Usage**: Copy to your project root

#### `CONTRIBUTING.md`
- **Purpose**: Template for contribution guidelines
- **Contents**:
  - Code of conduct
  - Development workflow
  - Commit message guidelines
  - PR process
  - Branch strategy for contributors

---

## How Files Work Together

### Version Calculation Flow

```
1. Developer creates PR: feature/my-feature → alpha
2. Workflow reads: VERSION file (e.g., 0.1.0)
3. Workflow calculates: Build version (v0.1.0+SHA)
4. Tests run, PR shows build version
5. PR merged to alpha

6. Developer creates PR: alpha → main
7. Workflow detects: Source is feature/* branch
8. Workflow calculates: v0.2.0-beta (MINOR bump)
9. PR merged to main
10. Workflow creates: Tag v0.2.0-beta
11. Workflow updates: VERSION to 0.2.0-beta
12. Workflow commits: VERSION back to repo
```

### Branch Strategy Integration

```
BRANCH_STRATEGY.md (rules) 
    ↓
ci-cd-versioned.yml (automation)
    ↓
VERSION file (source of truth)
    ↓
Git tags (releases)
    ↓
GitHub Releases (artifacts)
```

### Documentation Hierarchy

```
README.md (overview)
    ├── Quick start
    ├── → BRANCH_STRATEGY.md (detailed strategy)
    └── → docs/
        ├── QUICKSTART.md (initial setup)
        ├── SETUP_GUIDE.md (detailed configuration)
        ├── WORKFLOW_EXAMPLES.md (step-by-step scenarios)
        ├── CUSTOMIZATION.md (advanced customization)
        └── QUICK_REFERENCE.md (daily usage cheat sheet)
```

---

## Usage Workflow

### For Template Users

1. **Read**: `README.md` for overview
2. **Setup**: `docs/QUICKSTART.md` for fast initial setup (5 min)
3. **Configure**: `docs/SETUP_GUIDE.md` for detailed configuration
4. **Daily Use**: `docs/QUICK_REFERENCE.md` for commands and workflows
5. **Customize**: `.github/workflows/ci-cd-versioned.yml` as needed

### For Contributors

1. **Read**: `CONTRIBUTING.md`
2. **Reference**: `BRANCH_STRATEGY.md`
3. **Follow**: Commit conventions
4. **Use**: `docs/QUICK_REFERENCE.md` for daily commands

### For Maintainers

1. **Understand**: `BRANCH_STRATEGY.md` thoroughly
2. **Monitor**: GitHub Actions workflows
3. **Review**: PRs according to strategy
4. **Manage**: VERSION file (automatic)

---

## Customization Points

### Must Customize

1. **README.md**: Update project name, description, badges
2. **ci-cd-versioned.yml**: Adjust build/test commands
3. **CONTRIBUTING.md**: Update project-specific details

### Optional Customization

1. **ci-cd-versioned.yml**: Docker builds, deployment steps
2. **BRANCH_STRATEGY.md**: Adjust to your team's needs
3. **VERSION**: Set initial version

### Usually Keep As-Is

1. **Version calculation logic** (unless needed)
2. **Branch protection recommendations**
3. **Documentation structure**

---

## File Maintenance

### Auto-Updated by CI/CD

- `VERSION` - Updated after every merge to main/release
- Git tags - Created automatically
- GitHub Releases - Generated automatically

### Updated by Maintainers

- `README.md` - Project-specific updates
- `BRANCH_STRATEGY.md` - Strategy adjustments
- `docs/*.md` - Documentation improvements
- `ci-cd-versioned.yml` - Workflow enhancements

### Updated by Contributors

- `docs/*.md` - Documentation fixes/improvements

---

## Adding New Files

### Adding New Documentation

1. Create file in `docs/`
2. Link from `docs/README.md`
3. Link from main `README.md` if relevant

### Adding New Workflows

1. Create in `.github/workflows/`
2. Document in `docs/CUSTOMIZATION.md`
3. Update `README.md` if significant

---

## Best Practices

1. **Keep VERSION file simple**: Single line, semantic version only
2. **Document all customizations**: Update relevant docs when changing workflow
3. **Keep docs in sync**: Update all affected docs when changing strategy
4. **Test workflow changes**: Use a test repository first

---

## Quick Navigation

- **Getting Started**: [README.md](README.md)
- **Fast Setup**: [docs/QUICKSTART.md](QUICKSTART.md) - 5 minute setup
- **Detailed Setup**: [docs/SETUP_GUIDE.md](SETUP_GUIDE.md) - Full configuration
- **Daily Usage**: [docs/QUICK_REFERENCE.md](QUICK_REFERENCE.md) - Command cheat sheet
- **Customization**: [docs/CUSTOMIZATION.md](CUSTOMIZATION.md) - Advanced topics

---

**This structure is designed for clarity and ease of use. Everything has its place!** 📁
