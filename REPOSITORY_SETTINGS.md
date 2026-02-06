# Repository Settings Recommendations

This document outlines recommended settings for SQLoot repositories to ensure security, quality, and consistency.

## General Settings

### Basic Information
- ✅ Add a clear description
- ✅ Add relevant topics/tags
- ✅ Include a website URL (if applicable)
- ✅ Add a license (MIT recommended)
- ✅ Add a README.md

### Features
- ✅ Enable Issues
- ✅ Enable Discussions (for community Q&A)
- ✅ Disable Wiki (use README/docs folder instead)
- ✅ Disable Projects (use GitHub Projects at org level)
- ✅ Enable Sponsorships (FUNDING.yml)

### Pull Requests
- ✅ Allow squash merging (recommended)
- ✅ Allow merge commits
- ❌ Disable rebase merging (to preserve history)
- ✅ Automatically delete head branches after merge
- ✅ Enable PR auto-merge

## Branch Protection Rules

### Main Branch (`main`)

**Require a pull request before merging:**
- ✅ Required approvals: 1 (for small teams) or 2 (for critical repos)
- ✅ Dismiss stale PR approvals when new commits are pushed
- ✅ Require review from Code Owners (when CODEOWNERS exists)
- ❌ Restrict who can dismiss pull request reviews (not needed for small teams)

**Require status checks before merging:**
- ✅ Require status checks to pass
- ✅ Require branches to be up to date before merging
- Required checks:
  - CI / Quality Gates
  - CodeQL (if enabled)
  - Security Scan (if applicable)

**Other restrictions:**
- ✅ Require conversation resolution before merging
- ✅ Require signed commits (recommended for security-critical repos)
- ❌ Require linear history (optional, depends on workflow)
- ❌ Lock branch (only for archived projects)
- ❌ Restrict pushes (allow all with push access)

**Rules applied to administrators:**
- ✅ Include administrators (recommended, but can be disabled for faster emergency fixes)

## Security & Analysis

### Security
- ✅ Enable Private vulnerability reporting
- ✅ Enable Dependabot alerts
- ✅ Enable Dependabot security updates
- ✅ Enable Dependabot version updates (via dependabot.yml)

### Code Scanning
- ✅ Enable CodeQL analysis
- ✅ Configure CodeQL for all supported languages
- ✅ Run on push and pull_request events
- ✅ Run weekly scheduled scans

### Secret Scanning
- ✅ Enable secret scanning
- ✅ Enable push protection (prevents accidental secret commits)

## Actions Permissions

### General
- ✅ Allow all actions and reusable workflows
- Alternative: Allow SQLoot actions and select non-SQLoot actions

### Workflow Permissions
- ✅ Read repository contents and packages permissions (default)
- Only enable "Read and write" for specific workflows that need it

### Fork Pull Request Workflows
- ✅ Require approval for first-time contributors
- ✅ Require approval for all outside collaborators

## Code Owners (CODEOWNERS)

Create a `CODEOWNERS` file in the root or `.github/` directory:

```
# Default owners
* @maintainer-username

# Specific paths
/docs/ @doc-team
/.github/ @devops-team
/security/ @security-team
```

## Webhooks & Integrations

### Recommended Integrations
- CodeQL (built-in)
- Dependabot (built-in)
- Optional:
  - Renovate Bot (alternative to Dependabot)
  - Codecov (for test coverage)
  - Sentry (for error tracking)

## GitHub Apps

### Recommended Apps
- **GitHub Actions** - CI/CD automation
- **Dependabot** - Dependency management
- **CodeQL** - Security analysis

## Rulesets (Beta)

Consider using Repository Rulesets for more advanced scenarios:
- Enforce commit signing
- Restrict file path changes
- Restrict commits from non-verified users
- Require workflows to pass

## Templates

Ensure these templates exist:
- ✅ Issue templates (bug report, feature request)
- ✅ Pull request template
- ✅ Contributing guidelines
- ✅ Code of conduct
- ✅ Security policy

## Access Control

### Team Permissions
- **Maintainers**: Admin access
- **Core Contributors**: Write access
- **External Contributors**: No direct access (PRs only)

### Branch Protection Bypass
Only allow for:
- Emergency hotfixes (with proper documentation)
- Automated release processes (via GitHub Apps/Actions)

## Monitoring

### Regular Reviews
- Monthly: Review open issues and PRs
- Quarterly: Review security alerts and dependencies
- Annually: Review and update all policies and templates

### Metrics to Track
- PR merge time
- Issue resolution time
- Security vulnerabilities
- Code coverage
- CI/CD success rate

## Implementation Checklist

- [ ] Configure branch protection for `main`
- [ ] Add CODEOWNERS file
- [ ] Enable security features
- [ ] Set up Dependabot
- [ ] Configure CodeQL
- [ ] Add required status checks
- [ ] Configure Actions permissions
- [ ] Review and update templates
- [ ] Document repository-specific settings

---

**Note**: These are recommendations. Adjust based on your team size, project maturity, and specific requirements.
