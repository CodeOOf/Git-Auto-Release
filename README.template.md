# Your Project Name

> **🎯 Template Repository:** This uses Git-Auto-Release for automated semantic versioning and releases.

---

## 🚀 First-Time Setup

### 1. Clean Up Template Files

```bash
# You can safely remove these template files:
rm LICENSE                   # Replace with your own license
```

**📁 Template Documentation**: All Git-Auto-Release docs are in [`docs/git-auto-release/`](docs/git-auto-release/)

**Choose your approach:**
- **Keep for reference**: Leave `docs/git-auto-release/` as-is
- **Archive**: `mkdir archive; mv docs/git-auto-release archive/`
- **Remove**: `rm -r docs/git-auto-release/`

### 2. Set Up Your Repository

Follow the setup guide: [`docs/git-auto-release/SETUP_GUIDE.md`](docs/git-auto-release/SETUP_GUIDE.md)

**Quick steps:**
1. Configure branch protection (main, release)
2. Set initial VERSION: `echo "0.1.0" > VERSION`
3. Test with a feature branch
4. Review workflow examples

**Quick Start Guide**: [`docs/git-auto-release/QUICKSTART.md`](docs/git-auto-release/QUICKSTART.md) *(10 minutes)*

### 3. Customize This README

Replace this template content with:
- Your project description
- Installation instructions
- Usage examples
- API documentation
- Contributing guidelines

---

## About This Project

<!-- Replace with your project description -->

**Example:**
> ProjectName is a tool for doing X, Y, and Z. It helps developers achieve [specific goal] by providing [key features].

## Features

<!-- Replace with your actual features -->

- ✨ Feature 1
- 🚀 Feature 2
- 🔧 Feature 3

## Installation

<!-- Replace with your installation instructions -->

```bash
# Example installation
npm install your-project
# or
pip install your-project
# or
git clone https://github.com/yourusername/your-project.git
cd your-project
npm install
```

## Quick Start

<!-- Replace with your quick start guide -->

```bash
# Example quick start
your-project init
your-project run --config config.yml
```

## Usage

<!-- Add detailed usage documentation -->

### Basic Usage

```bash
# Example commands
```

### Advanced Usage

```bash
# More examples
```

## Documentation

<!-- Update these links with your actual documentation -->

- [Getting Started](docs/getting-started.md)
- [Configuration](docs/configuration.md)
- [API Reference](docs/api-reference.md)

## Version Control

This project uses **Git-Auto-Release** for automated semantic versioning.

**Current Version**: See [`VERSION`](VERSION) or [Releases](../../releases)

### How Versioning Works

Versions are automatically calculated from branch names:
- `feature/*` branches → **MINOR** version bump (0.X.0)
- `bugfix/*` branches → **PATCH** version bump (0.0.X)
- `alpha`/`beta` branches → **MAJOR** version bump (X.0.0)

**Learn more**: [`docs/git-auto-release/BRANCH_STRATEGY.md`](docs/git-auto-release/BRANCH_STRATEGY.md)

### Workflow Examples

See real scenarios: [`docs/git-auto-release/WORKFLOW_EXAMPLES.md`](docs/git-auto-release/WORKFLOW_EXAMPLES.md)

## Contributing

<!-- Customize for your project -->

We follow the Git-Auto-Release workflow for contributions.

**Quick workflow:**
1. Create a branch: `git checkout -b feature/my-feature` (from `main`)
2. Make changes and commit
3. Push and create a Pull Request to `main`
4. Merge creates automatic version tag

**Full guidelines**: [`docs/git-auto-release/CONTRIBUTING.md`](docs/git-auto-release/CONTRIBUTING.md)

## License

<!-- Replace with your license -->

[MIT License](LICENSE) - See LICENSE file for details

---

## 📚 Git-Auto-Release Documentation

Template documentation is in [`docs/git-auto-release/`](docs/git-auto-release/):

| Guide | Description |
|-------|-------------|
| [📖 Quick Start](docs/git-auto-release/QUICKSTART.md) | 10-minute setup guide |
| [⚙️ Setup Guide](docs/git-auto-release/SETUP_GUIDE.md) | Complete configuration |
| [🌳 Branch Strategy](docs/git-auto-release/BRANCH_STRATEGY.md) | Branching model explained |
| [📋 Workflow Examples](docs/git-auto-release/WORKFLOW_EXAMPLES.md) | Real-world scenarios |
| [🔧 Customization](docs/git-auto-release/CUSTOMIZATION.md) | Adapt to your platform |
| [⚡ Quick Reference](docs/git-auto-release/QUICK_REFERENCE.md) | Command cheat sheet |

**💡 Tip**: Once comfortable with the automation, you can move or remove `docs/git-auto-release/`