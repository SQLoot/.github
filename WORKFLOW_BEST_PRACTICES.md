# GitHub Actions Workflow Best Practices

This document outlines best practices for creating and maintaining GitHub Actions workflows in SQLoot repositories.

## General Principles

### Security First
- ✅ Use pinned action versions with SHA hashes for production
- ✅ Limit workflow permissions to minimum required
- ✅ Never log secrets or sensitive data
- ✅ Use GitHub's built-in secrets management
- ✅ Enable dependency review for workflows

### Performance
- ✅ Cache dependencies appropriately
- ✅ Use concurrent jobs when possible
- ✅ Optimize checkout depth (shallow clones)
- ✅ Cancel redundant workflow runs
- ✅ Use matrix strategies for parallel testing

### Maintainability
- ✅ Use reusable workflows for common tasks
- ✅ Keep workflows DRY (Don't Repeat Yourself)
- ✅ Document complex workflows
- ✅ Use consistent naming conventions
- ✅ Regularly update action versions

## Workflow Template Guidelines

### CI Workflow

**Triggers:**
```yaml
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
```

**Required Jobs:**
- Linting and formatting
- Type checking (for TypeScript)
- Unit tests
- Integration tests (if applicable)

**Optimizations:**
```yaml
# Cancel in-progress runs for PRs
concurrency:
  group: ${{ github.workflow }}-${{ github.event.pull_request.number || github.ref }}
  cancel-in-progress: true
```

### Security Workflow

**Triggers:**
```yaml
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  schedule:
    - cron: '0 0 * * 0'  # Weekly on Sunday
```

**Required Checks:**
- Dependency auditing
- Secret scanning
- SAST (Static Application Security Testing)
- License compliance

**Permissions:**
```yaml
permissions:
  security-events: write
  contents: read
```

### Release Workflow

**Triggers:**
```yaml
on:
  push:
    tags:
      - 'v*'
```

**Best Practices:**
- Automated changelog generation
- Semantic versioning
- Asset signing
- Docker image publishing (if applicable)
- NPM package publishing (if applicable)

## Action Version Pinning

### Development
```yaml
# OK for templates and examples
- uses: actions/checkout@v4
```

### Production
```yaml
# Recommended: Pin to SHA with version comment
- uses: actions/checkout@b4ffde65f46336ab88eb53be808477a3936bae11  # v4.1.1
```

**Why:** 
- Prevents supply chain attacks
- Ensures reproducible builds
- Protects against malicious updates

## Permissions Model

### Principle of Least Privilege

**Default (read-only):**
```yaml
permissions:
  contents: read
```

**Specific grants only when needed:**
```yaml
permissions:
  contents: write      # For releases, commits
  pull-requests: write # For PR comments
  issues: write        # For issue comments
  security-events: write # For CodeQL
```

**Never use:**
```yaml
permissions: write-all  # ❌ Too permissive
```

## Secrets Management

### Do's
✅ Use GitHub secrets for sensitive data
✅ Use environment-specific secrets
✅ Rotate secrets regularly
✅ Use OIDC for cloud provider auth (AWS, Azure, GCP)
✅ Mask secrets in logs automatically

### Don'ts
❌ Never echo secrets in workflow logs
❌ Don't use secrets in PR builds from forks
❌ Don't store secrets in code or config files
❌ Don't use weak encryption

### Example
```yaml
steps:
  - name: Deploy
    env:
      API_KEY: ${{ secrets.API_KEY }}
    run: |
      # API_KEY is automatically masked in logs
      ./deploy.sh
```

## Caching Strategy

### Bun/Node.js Dependencies
```yaml
- name: Setup Bun
  uses: oven-sh/setup-bun@v2
  with:
    bun-version: latest

- name: Cache dependencies
  uses: actions/cache@v4
  with:
    path: ~/.bun/install/cache
    key: ${{ runner.os }}-bun-${{ hashFiles('**/bun.lockb') }}
    restore-keys: |
      ${{ runner.os }}-bun-
```

### Build Artifacts
```yaml
- name: Cache build
  uses: actions/cache@v4
  with:
    path: |
      dist/
      .tsbuildinfo
    key: ${{ runner.os }}-build-${{ hashFiles('src/**') }}
```

## Matrix Testing

### Multi-platform Testing
```yaml
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest, macos-latest]
    node-version: [18, 20, 21]
    exclude:
      # Exclude specific combinations if needed
      - os: windows-latest
        node-version: 18
```

### Fail-fast vs Continue on Error
```yaml
strategy:
  fail-fast: false  # Continue running other jobs if one fails
  matrix:
    # ...
```

## Environment Protection

### Production Deployments
```yaml
environment:
  name: production
  url: https://example.com
```

**Configure in GitHub:**
- Required reviewers
- Wait timer
- Deployment branches (main only)

## Workflow Reusability

### Reusable Workflow
```yaml
# .github/workflows/reusable-test.yml
on:
  workflow_call:
    inputs:
      node-version:
        required: true
        type: string

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: ${{ inputs.node-version }}
      - run: npm test
```

### Calling Reusable Workflow
```yaml
# .github/workflows/ci.yml
jobs:
  test:
    uses: ./.github/workflows/reusable-test.yml
    with:
      node-version: '20'
```

## Error Handling

### Continue on Error
```yaml
- name: Optional check
  continue-on-error: true
  run: npm run optional-check
```

### Conditional Steps
```yaml
- name: Deploy
  if: github.ref == 'refs/heads/main' && success()
  run: ./deploy.sh
```

## Monitoring and Notifications

### Status Badges
```markdown
![CI](https://github.com/SQLoot/repo/workflows/CI/badge.svg)
```

### Notifications
- Use GitHub's built-in notifications
- Consider Slack/Discord webhooks for critical failures
- Set up email alerts for security issues

## Common Pitfalls

### ❌ Don't
```yaml
# Hardcoded secrets
env:
  API_KEY: "sk_live_12345..."

# Too permissive
permissions: write-all

# No version pinning
- uses: actions/checkout@latest

# Unsafe script injection
run: echo "${{ github.event.issue.title }}"
```

### ✅ Do
```yaml
# Use GitHub secrets
env:
  API_KEY: ${{ secrets.API_KEY }}

# Minimal permissions
permissions:
  contents: read

# Pinned versions
- uses: actions/checkout@v4

# Safe variable usage
run: |
  TITLE="${{ github.event.issue.title }}"
  echo "${TITLE}"
```

## Debugging Workflows

### Enable Debug Logging
Set repository secrets:
- `ACTIONS_STEP_DEBUG`: `true`
- `ACTIONS_RUNNER_DEBUG`: `true`

### Local Testing
Use [act](https://github.com/nektos/act) to test workflows locally:
```bash
act -j test  # Run the 'test' job
```

## Performance Optimization

### Shallow Clones
```yaml
- uses: actions/checkout@v4
  with:
    fetch-depth: 1  # Only fetch the latest commit
```

### Sparse Checkout
```yaml
- uses: actions/checkout@v4
  with:
    sparse-checkout: |
      src/
      tests/
```

### Job Artifacts
```yaml
- name: Upload artifact
  uses: actions/upload-artifact@v4
  with:
    name: build-output
    path: dist/
    retention-days: 7  # Clean up after 7 days
```

## Compliance and Auditing

### Audit Logs
- Review workflow runs regularly
- Monitor for unusual patterns
- Check for unauthorized access

### Required Status Checks
Configure in branch protection:
- All tests must pass
- Security scans must pass
- No high/critical vulnerabilities

## Template Quality Checklist

- [ ] Clear, descriptive workflow name
- [ ] Appropriate triggers configured
- [ ] Minimal permissions granted
- [ ] Dependencies cached
- [ ] Versions pinned (for production)
- [ ] Secrets handled securely
- [ ] Error handling implemented
- [ ] Documentation included
- [ ] Tested in isolation
- [ ] Follows SQLoot conventions

## Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [GitHub Actions Security Best Practices](https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions)
- [Awesome Actions](https://github.com/sdras/awesome-actions)

---

**Note**: Keep workflows simple, secure, and maintainable. Regular reviews and updates are essential.
