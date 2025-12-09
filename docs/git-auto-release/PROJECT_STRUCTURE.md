# Git-Auto-Release Project Structure

```
Git-Auto-Release/
├── .github/
│   └── workflows/
│       └── ci-cd-versioned.yml          # Main CI/CD workflow
├── docs/
│   └── git-auto-release/
│       ├── BRANCH_STRATEGY.md           # Detailed branch strategy
│       ├── CUSTOMIZATION.md             # How to customize the template
│       ├── LICENSE                      # MIT License (template license)
│       ├── PROJECT_STRUCTURE.md         # This file - project structure guide
│       ├── QUICKSTART.md                # 5-minute initial setup guide
│       ├── README.md                    # Documentation index
│       ├── REFERENCE_GUIDE.md           # Comprehensive reference guide
│       ├── SETUP_GUIDE.md               # Complete setup instructions
│       └── WORKFLOW_EXAMPLES.md         # Real-world usage examples
├── CHECKLIST.md                         # Pre-release checklist
├── CONTRIBUTING.md                      # Contribution guidelines
├── CONTRIBUTING.template.md             # Template for user projects
├── LICENSE                              # MIT License (repository license)
├── README.md                            # Main project documentation
├── README.template.md                   # Template for user projects
└── VERSION                              # Current version (e.g., 1.0.0-beta)
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

#### `CHECKLIST.md`
- **Purpose**: Pre-release checklist
- **Location**: Root directory
- **Usage**: Verify all steps before major releases

#### `CONTRIBUTING.md` & `CONTRIBUTING.template.md`
- **Purpose**: Contribution guidelines (template provides starting point for user projects)
- **Location**: Root directory
- **Contents**:
  - Code of conduct
  - Development workflow
  - PR process
  - Branch strategy reference

#### `README.template.md`
- **Purpose**: Template README for user projects
- **Location**: Root directory
- **Usage**: Copy and customize for your project

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

#### `QUICKSTART.md`
- **Purpose**: Fast 5-minute initial setup guide
- **Location**: docs/git-auto-release/
- **Contents**:
  - Quick repository setup
  - Essential commands
  - Basic commit format
  - Next steps
- **Note**: For daily usage reference, see `REFERENCE_GUIDE.md`

#### `REFERENCE_GUIDE.md`
- **Purpose**: Comprehensive reference guide (for established projects)
- **Location**: docs/git-auto-release/
- **Contents**:
  - What is this template?
  - Key features and use cases
  - Decision tree for daily tasks
  - Workflow commands (feature, bugfix, hotfix, release)
  - Version progression tables
  - Commit message format (Conventional Commits)
  - Git command reference
  - Troubleshooting quick fixes
  - FAQ (13+ common questions)
  - Customization points
  - License & support

#### `BRANCH_STRATEGY.md`
- **Purpose**: Detailed branch strategy documentation
- **Location**: docs/git-auto-release/
- **Contents**:
  - Branch types and purposes
  - Version bump logic
  - Release flows (major, minor, patch, hotfix)
  - Mermaid diagrams
  - Workflow details
- **Note**: Commit message conventions are in `REFERENCE_GUIDE.md`

#### `PROJECT_STRUCTURE.md`
- **Purpose**: This file - explains project organization
- **Location**: docs/git-auto-release/
- **Contents**: File structure, descriptions, and relationships

#### `LICENSE`
- **Purpose**: MIT License for the template itself
- **Location**: docs/git-auto-release/
- **Note**: Your projects can use any license you choose



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
    └── → docs/git-auto-release/
        ├── README.md (documentation index)
        ├── QUICKSTART.md (5-minute setup)
        ├── REFERENCE_GUIDE.md (comprehensive reference + FAQ)
        ├── BRANCH_STRATEGY.md (detailed strategy)
        ├── SETUP_GUIDE.md (detailed configuration)
        ├── WORKFLOW_EXAMPLES.md (step-by-step scenarios)
        ├── CUSTOMIZATION.md (advanced customization)
        ├── PROJECT_STRUCTURE.md (this file)
        └── LICENSE (template license)
```

---

## Usage Workflow

### For Template Users

1. **Read**: `README.md` for overview
2. **Setup**: `docs/git-auto-release/QUICKSTART.md` for fast initial setup (5 min)
3. **Reference**: `docs/git-auto-release/REFERENCE_GUIDE.md` for daily commands and FAQ
4. **Configure**: `docs/git-auto-release/SETUP_GUIDE.md` for detailed configuration
5. **Customize**: `.github/workflows/ci-cd-versioned.yml` as needed

### For Contributors

1. **Read**: `CONTRIBUTING.md` (root directory)
2. **Reference**: `docs/git-auto-release/BRANCH_STRATEGY.md`
3. **Follow**: Commit conventions in `docs/git-auto-release/REFERENCE_GUIDE.md`
4. **Use**: `docs/git-auto-release/REFERENCE_GUIDE.md` for daily commands

### For Maintainers

1. **Understand**: `docs/git-auto-release/BRANCH_STRATEGY.md` thoroughly
2. **Monitor**: GitHub Actions workflows
3. **Review**: PRs according to strategy
4. **Manage**: VERSION file (automatic)
5. **Check**: `CHECKLIST.md` before major releases

---

## Customization Points

### Must Customize

1. **README.md**: Use `README.template.md` as starting point
2. **.github/workflows/ci-cd-versioned.yml**: Adjust build/test commands
3. **CONTRIBUTING.md**: Use `CONTRIBUTING.template.md` for your project

### Optional Customization

1. **.github/workflows/ci-cd-versioned.yml**: Docker builds, deployment steps
2. **docs/git-auto-release/BRANCH_STRATEGY.md**: Adjust to your team's needs
3. **VERSION**: Set initial version (default: 0.1.0)

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
- `docs/git-auto-release/*.md` - Documentation improvements
- `.github/workflows/ci-cd-versioned.yml` - Workflow enhancements
- `CHECKLIST.md` - Pre-release checklist updates

### Updated by Contributors

- `docs/git-auto-release/*.md` - Documentation fixes/improvements
- `CONTRIBUTING.md` - Contribution guideline improvements

---

## Adding New Files

### Adding New Documentation

1. Create file in `docs/git-auto-release/`
2. Link from `docs/git-auto-release/README.md`
3. Link from main `README.md` if relevant
4. Update `PROJECT_STRUCTURE.md` to document it

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

- **Getting Started**: [README.md](../../README.md) - Main overview
- **Documentation Index**: [docs/git-auto-release/README.md](README.md)
- **Fast Setup**: [QUICKSTART.md](QUICKSTART.md) - 5 minute setup
- **Daily Usage**: [REFERENCE_GUIDE.md](REFERENCE_GUIDE.md) - Comprehensive reference + FAQ
- **Branch Strategy**: [BRANCH_STRATEGY.md](BRANCH_STRATEGY.md) - Detailed branching model
- **Detailed Setup**: [SETUP_GUIDE.md](SETUP_GUIDE.md) - Full configuration
- **Customization**: [CUSTOMIZATION.md](CUSTOMIZATION.md) - Advanced topics

---

**This structure is designed for clarity and ease of use. Everything has its place!** 📁
