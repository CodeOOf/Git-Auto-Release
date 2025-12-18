
📖 **Navigation**: [README](../../README.md) | [Quickstart](QUICKSTART.md) | [Reference Guide](REFERENCE_GUIDE.md) | **FAQ**

# Frequently Asked Questions (FAQ)

Find answers to common questions and practical troubleshooting tips for Git Auto Release. For deep technical details, see the [Reference Guide](REFERENCE_GUIDE.md). If your question isn't answered here, please open an issue or discussion!

---

## Table of Contents

- [How do I skip CI?](#how-do-i-skip-ci)
- [Can I use this with GitLab or Jenkins?](#can-i-use-this-with-gitlab-or-jenkins)
- [How do I customize versioning?](#how-do-i-customize-versioning)
- [What if I commit directly to main?](#what-if-i-commit-directly-to-main)
- [Is changelog generation supported?](#is-changelog-generation-supported)
- [Why did my workflow not run?](#why-did-my-workflow-not-run)
- [Why is the version wrong after a merge?](#why-is-the-version-wrong-after-a-merge)
- [How do I fix VERSION file conflicts?](#how-do-i-fix-version-file-conflicts)
- [How do I fix a failed CI test?](#how-do-i-fix-a-failed-ci-test)
- [Why did my workflow fail with "tag already exists"?](#why-did-my-workflow-fail-with-tag-already-exists)
- [How do I rollback a release?](#how-do-i-rollback-a-release)
- [How do I get help?](#how-do-i-get-help)

---

## How do I skip CI?
Add `[skip ci]` to your commit message to skip the CI/CD pipeline for that commit.

## Can I use this with GitLab or Jenkins?
Yes! While the default setup uses GitHub Actions, you can adapt the workflow logic for other CI/CD platforms.

## How do I customize versioning?
Edit the workflow YAML and the logic in the `VERSION` file to suit your project's needs.

## What if I commit directly to main?
Direct commits to `main` are blocked by policy and will fail CI. Always use feature/bugfix branches and pull requests.

## Is changelog generation supported?
Not yet, but it's on the roadmap. See the README for details.

## Why did my workflow not run?
Check that your workflow file exists in `.github/workflows/`, GitHub Actions is enabled, and branch protection rules allow workflows. See the Reference Guide for more troubleshooting.

## Why is the version wrong after a merge?
Check your branch name matches the expected pattern:
- `feature/*` → MINOR bump
- `bugfix/*` → PATCH bump
- `alpha` → MAJOR bump
- `hotfix` → PATCH bump (from release)
See the Reference Guide for full versioning logic.

## How do I fix VERSION file conflicts?
Always choose the higher version number:
```bash
git checkout --theirs VERSION
git add VERSION
git commit
```
See the Reference Guide for more details.

## How do I fix a failed CI test?
Run tests locally first (`npm test`, `python -m pytest`, etc.). Check the CI logs for details. Re-run failed jobs if needed.

## Why did my workflow fail with "tag already exists"?
Someone may have manually created the tag. Delete it locally and remotely, then re-run:
```bash
git tag -d v1.0.0
git push origin :refs/tags/v1.0.0
```


## How do I rollback a release?

### Beta/Alpha Release Rollback
If you merged a beta/alpha branch to `main` and need to undo it:

1. Find the merge commit hash:
	```bash
	git log --oneline main
	# Look for the merge commit you want to revert
	```
2. Revert the merge commit:
	```bash
	git revert -m 1 <merge-commit-hash>
	git push origin main
	```
3. (Optional) Delete any incorrect tags:
	```bash
	git tag -d v1.0.0-beta v1.0.0-alpha
	git push origin :refs/tags/v1.0.0-beta :refs/tags/v1.0.0-alpha
	```

### Production Release Rollback
If you need to fix a production release:

**Option 1: Hotfix Branch**
1. Create a hotfix branch from `release`:
	```bash
	git checkout release
	git pull origin release
	git checkout -b hotfix
	# Make your fix
	git add .
	git commit -m "fix: production hotfix"
	git push origin hotfix
	# Open PR: hotfix → release
	```
2. Merge the PR. This will bump the patch version and create a new release tag.

**Option 2: Revert Production Release**
1. Find the release merge commit hash:
	```bash
	git log --oneline release
	# Find the merge commit to revert
	```
2. Revert the merge commit:
	```bash
	git revert -m 1 <merge-commit-hash>
	git push origin release
	```
3. (Optional) Delete the incorrect release tag:
	```bash
	git tag -d v1.0.0
	git push origin :refs/tags/v1.0.0
	```

See the Reference Guide for advanced rollback scenarios and edge cases.

## How do I get help?
Check the [Reference Guide](REFERENCE_GUIDE.md) for developer details, or open an issue/discussion for support.

---

For more technical details, see the [Reference Guide](REFERENCE_GUIDE.md). If you encounter a question while reading, check here or open an issue!