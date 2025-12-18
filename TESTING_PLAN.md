# Git Auto Release – Comprehensive Testing Plan

This document provides a step-by-step verification plan for contributors to ensure all core workflows, versioning, and policy checks work as intended before submitting changes.

---


## Sequential Test Scenarios

Follow these steps in order to fully verify the workflow and versioning logic. Each step lists the starting version, actions, and expected results (tags, VERSION file changes).

---

### 1. Feature Branch (Normal Development)
**Start:** VERSION = `0.1.0`
1. Create `feature/first-feature` branch from `main`
2. Make changes, push, open PR to `main`
3. **Expect:**
	- Build version: `v0.1.0+<SHA>`
	- After merge: VERSION = `0.2.0-beta`, tag `v0.2.0-beta`

### 2. Bugfix Branch (Normal Development)
**Start:** VERSION = `0.2.0-beta`
1. Create `bugfix/first-fix` branch from `main`
2. Make changes, push, open PR to `main`
3. **Expect:**
	- Build version: `v0.2.0-beta+<SHA>`
	- After merge: VERSION = `0.2.1-beta`, tag `v0.2.1-beta`

### 3. Major Release (Alpha Phase)
**Start:** VERSION = `0.2.1-beta`
1. Create `alpha` branch from `main`
2. Make breaking changes, push, open PR to `main`
3. **Expect:**
	- Build version: `v0.2.1-beta+<SHA>`
	- After merge: VERSION = `1.0.0-alpha`, tag `v1.0.0-alpha`, beta branch auto-created

### 4. Feature/Bugfix During Alpha
**Start:** VERSION = `1.0.0-alpha`
1. Create `feature/alpha-feature` or `bugfix/alpha-fix` from `main`
2. Make changes, push, open PR to `main`
3. **Expect:**
	- Build version: `v1.0.0-alpha+<SHA>`
	- After merge: VERSION = `1.0.0-alpha.1`, tag `v1.0.0-alpha.1`

### 5. Beta Phase (Stabilization)
**Start:** VERSION = `1.0.0-alpha.N` (after alpha phase)
1. Switch to `beta` branch, make stabilization changes, push, open PR to `main`
2. **Expect:**
	- Build version: `v1.0.0-alpha.N+<SHA>`
	- After merge: VERSION = `1.0.0-beta`, tag `v1.0.0-beta`

### 6. Feature/Bugfix During Beta
**Start:** VERSION = `1.0.0-beta`
1. Create `feature/beta-feature` or `bugfix/beta-fix` from `main`
2. Make changes, push, open PR to `main`
3. **Expect:**
	- Build version: `v1.0.0-beta+<SHA>`
	- After merge: VERSION = `1.0.0-beta.1`, tag `v1.0.0-beta.1` (increment `.N` for each merge)

### 7. Production Release
**Start:** VERSION = `1.0.0-beta.N` (on `main`)
1. Create PR from `main` to `release`
2. **Expect:**
	- During PR: RC tags `v1.0.0-rc.1`, `v1.0.0-rc.2`, ...
	- After merge: VERSION = `1.0.0` (clean), tag `v1.0.0`, main/release synced

### 8. Hotfix (Production Bug)
**Start:** VERSION = `1.0.0` (on `release`, after first production release)
1. Create `hotfix/critical-fix` branch from `release`
2. Make fix, push, open PR to `release`
3. **Expect:**
	- Build version: `v1.0.0+<SHA>`
	- After merge: VERSION = `1.0.1`, tag `v1.0.1`, main auto-synced to `1.0.1`

### 9. Direct Commit to Main (Policy Violation)
**Start:** Any VERSION
1. Attempt direct commit to `main`
2. **Expect:**
	- CI fails with policy violation
	- Build version still created (e.g., `vX.Y.Z+<SHA>`) but workflow fails

---

## Verification Checklist
- [ ] All tags created as expected
- [ ] VERSION file updated after each merge
- [ ] Build versions show `VERSION+SHA`
- [ ] Preview versions during PRs are correct
- [ ] Beta branch auto-created after alpha merge
- [ ] `.N` increments work during alpha/beta
- [ ] Policy violation detection works
- [ ] No unexpected workflow failures

---

For more details or troubleshooting, see the main documentation or open an issue.
